# ADR-0001: Estratégia de multi-tenancy

- **Status**: Proposta
- **Data**: 2026-04-28
- **Decisores**: a definir
- **Tags**: arquitetura, persistência

## Contexto

O sistema servirá múltiplos clientes (tenants). Mesmo começando com 1 (Seven Resíduos), precisamos modelar multi-tenancy desde o dia 1 para evitar refatoração custosa depois.

Opções consideradas:

1. **`tenant_id` em todas as tabelas + RLS** (PostgreSQL Row-Level Security)
2. **Schema-per-tenant** (cada tenant tem seu schema separado)
3. **Database-per-tenant** (cada tenant tem seu DB completo)

## Comparação

| Critério | Opção 1 (tenant_id + RLS) | Opção 2 (schema) | Opção 3 (DB) |
|---|---|---|---|
| Custo de migration | 1 migration | N migrations | N migrations |
| Onboarding novo tenant | 1 INSERT | criar schema + migrations | provisionar DB |
| Backup granular por tenant | Trabalhoso (filtrar) | Trivial | Trivial |
| Cross-tenant analytics | Trivial | Difícil (UNION) | Inviável |
| Isolamento de segurança | Forte se RLS bem feita | Forte por design | Total |
| Volume alto de tenants (1000+) | OK | Pesado | Inviável |
| Custo de infra | Baixo | Médio | Alto |
| Compliance / dados sensíveis | OK | Melhor | Melhor |

## Decisão

**Opção 1: `tenant_id` em todas as tabelas + Row-Level Security do PostgreSQL.**

## Justificativa

- Volume previsto baixo (poucos tenants no MVP, projeção < 50).
- Time pequeno (1-2 devs) — custo operacional de schema-per-tenant é caro.
- Cross-tenant analytics é provável no futuro (benchmarking entre clientes).
- RLS bem configurado oferece isolamento forte e auditável.
- Migrations únicas reduzem chance de drift entre tenants.

## Consequências

### Positivas
- Onboarding de novo tenant é uma operação atômica simples.
- Backup full do DB cobre todos.
- Schema único de fácil entendimento.
- Mudanças de schema aplicadas a todos os tenants ao mesmo tempo (consistência).

### Negativas / Mitigações
- **Risco de isolamento**: bug que esqueça `tenant_id` em filtro vaza dados.
  - **Mitigação**: RLS no DB como segunda camada de defesa. ESLint custom rule para forçar `tenant_id` em todas as queries.
- **Performance em volume alto**: tabelas grandes ficam pesadas.
  - **Mitigação**: índices `(tenant_id, ...)` em todas tabelas. Particionamento por `tenant_id` quando necessário.
- **Backup parcial**: restore de 1 tenant requer script.
  - **Mitigação**: aceitável para MVP. Documentar runbook.
- **GDPR / LGPD "right to be forgotten"**: deletar dados de 1 tenant exige queries específicas.
  - **Mitigação**: helper centralizado `deleteTenant(id)` com lista exaustiva de tabelas.

## Implementação

Detalhes em [`02-domain/multi-tenancy.md`](../../02-domain/multi-tenancy.md).

## Revisão

Reavaliar em **6 meses pós-launch** ou quando atingirmos 100 tenants ou requisito enterprise de isolamento total.
