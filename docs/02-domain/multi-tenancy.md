# Multi-tenancy

## Decisão (a confirmar via [ADR-0001](../06-roadmap/adrs/0001-multi-tenancy.md))

Estratégia recomendada: **`tenant_id` em todas as tabelas + Row-Level Security (RLS) no PostgreSQL**.

## Por quê (e não schema-per-tenant)

| Critério | `tenant_id` + RLS | Schema-per-tenant |
|---|---|---|
| Custo de migrations | 1 migration roda em todas | N migrations, propenso a drift |
| Onboarding novo tenant | Insert numa tabela | Criar schema + rodar migrations |
| Backup/restore granular | Mais trabalhoso | Trivial |
| Isolamento (segurança) | RLS forte se bem configurado | Isolamento total por design |
| Cross-tenant analytics | Trivial | Difícil |
| Volume de tenants alto | OK | Pesado (>100 schemas vira problema) |

Para nosso caso (poucos tenants previstos, time pequeno, custo de infra baixo), `tenant_id` + RLS vence.

## Implementação

### 1. Toda tabela de domínio tem `tenant_id`

```sql
CREATE TABLE document (
  id UUID PRIMARY KEY,
  tenant_id UUID NOT NULL REFERENCES tenant(id),
  -- ...
);

CREATE INDEX document_tenant_idx ON document(tenant_id);
```

### 2. RLS habilitado

```sql
ALTER TABLE document ENABLE ROW LEVEL SECURITY;

CREATE POLICY tenant_isolation ON document
  FOR ALL
  USING (tenant_id = current_setting('app.tenant_id', true)::uuid);
```

### 3. API seta `app.tenant_id` por conexão

```ts
// pseudocódigo
async function withTenantContext(req, fn) {
  const conn = await pool.acquire();
  await conn.query(`SET LOCAL app.tenant_id = $1`, [req.user.tenant_id]);
  try {
    return await fn(conn);
  } finally {
    pool.release(conn);
  }
}
```

### 4. Super-admin bypass

Bypass do RLS apenas para um role específico do banco usado por scripts internos:

```sql
ALTER TABLE document FORCE ROW LEVEL SECURITY;
GRANT BYPASSRLS TO seven_super;
```

A API **nunca** usa o role super.

## Identificação do tenant na request

Ordem de precedência:

1. **Subdomínio** — `<tenant-slug>.seven.app` ou `seven.app/<tenant-slug>` (quando aplicável)
2. **Cookie de sessão** — payload contém `tenant_id` validado pelo gateway
3. **Header explícito** — `X-Tenant-Id` para chamadas server-to-server (com auth especial)

## Edge cases

- **Usuário em múltiplos tenants**: 1 conta de e-mail pode ter múltiplos `(user_id, tenant_id)`. UI mostra seletor. Sessão guarda o `tenant_id` ativo.
- **Convite cross-tenant**: usuário existente é "anexado" ao novo tenant criando novo registro `user` (mesmo e-mail, novo `tenant_id`).
- **Migração de dados entre tenants**: explicitamente proibido na API. Apenas via script com auditoria manual.

## Uniqueness com tenant

Nunca usamos `UNIQUE(email)`. Sempre `UNIQUE(tenant_id, email)`:

```sql
CREATE UNIQUE INDEX user_email_per_tenant
  ON "user"(tenant_id, email);
```

## Testes

- Suite de testes E2E **obrigatória** valida que tenant A nunca consegue ler dado de tenant B (mesmo com IDs adivinhados).
- Migrations rodam em DB de staging com 2+ tenants populados antes de promover para prod.
