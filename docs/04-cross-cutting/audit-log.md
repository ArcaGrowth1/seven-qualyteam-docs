# Audit Log

## Princípios

1. **Imutável** — só insere, nunca atualiza nem deleta.
2. **Atomic com a transação** — log e mudança vivem na mesma transação. Se a mudança falha, o log também não é gravado.
3. **PII mascarada** — e-mail parcial (`b***@y.com`), senhas nunca, CPF nunca em texto pleno.
4. **Diferencial** — guarda `before` e `after` em JSONB para rastrear o que mudou.

## Schema

```sql
CREATE TABLE audit_log (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id UUID NOT NULL REFERENCES tenant(id),
  user_id UUID REFERENCES "user"(id),
  action TEXT NOT NULL,                  -- ex: 'document.published'
  entity_type TEXT NOT NULL,             -- ex: 'document'
  entity_id UUID,
  before JSONB,
  after JSONB,
  ip_address INET,
  user_agent TEXT,
  metadata JSONB,                        -- contexto adicional (ex: módulo, motivo)
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX audit_log_tenant_entity_idx ON audit_log(tenant_id, entity_type, entity_id);
CREATE INDEX audit_log_tenant_user_idx ON audit_log(tenant_id, user_id);
CREATE INDEX audit_log_created_at_idx ON audit_log(created_at DESC);
```

## Convenções de `action`

`<modulo>.<entidade>.<verbo>`

Exemplos:
- `admin.user.created`
- `admin.user.deactivated`
- `admin.role.permissions_changed`
- `docs.document.published`
- `docs.controlled_copy.revoked`
- `nc.occurrence.created`
- `nc.rnc.escalated`
- `nc.rnc.plan_approved`
- `risk.unit_config.locked`
- `auth.login.success`

## Como gravar

Helper compartilhado:

```ts
async function audit(tx, params: {
  tenantId: string;
  userId: string | null;
  action: string;
  entityType: string;
  entityId?: string;
  before?: object;
  after?: object;
  metadata?: object;
}) {
  await tx.insert(auditLog).values({
    ...params,
    ipAddress: ctx.req.ip,
    userAgent: ctx.req.userAgent,
  });
}

// uso
await db.transaction(async (tx) => {
  const before = await tx.query.user.findById(id);
  const after = await tx.update(user).set({ active: false }).where(eq(user.id, id)).returning();
  await audit(tx, {
    tenantId: ctx.user.tenantId,
    userId: ctx.user.id,
    action: 'admin.user.deactivated',
    entityType: 'user',
    entityId: id,
    before,
    after: after[0],
  });
});
```

## Tela de consulta

**Path**: `/audit-log` (no app Admin) · **Permissão**: `admin.audit.read`

Filtros: período, usuário, tipo de entidade, action (autocomplete), entidade específica (id).

Linhas mostram: timestamp, usuário, action, entidade afetada, IP. Clicando expande o diff `before` vs `after`.

## Retenção

- Padrão: 5 anos (requisito de auditoria ISO).
- Particionamento mensal (`audit_log_2026_04`) quando volume passar de 10M registros.
- Arquivamento para storage frio (R2/S3 cold) após 1 ano.

## O que NÃO logar

- GETs de leitura comum (ruído).
- Mudanças de UI (filtros, ordenação).
- Tracking analítico (vai para PostHog, não audit).

## O que SEMPRE logar

- Auth events (login/logout/reset).
- CRUD em entidades de domínio (documents, NCs, riscos, oportunidades).
- Mudanças em permissões / grupos / usuários.
- Aprovações, publicações, encerramentos.
- Configurações de módulos (matriz, períodos, origens).
- Distribuição/revogação de cópias controladas.
