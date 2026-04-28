# ADR-0002: Monólito modular vs microsserviços

- **Status**: Proposta
- **Data**: 2026-04-28
- **Tags**: arquitetura, deploy

## Contexto

O QualyTeam (referência funcional) usa microsserviços com APIs separadas por subdomínio (`doc-api-web`, `nonconformities-api-web`, etc.). Precisamos decidir se replicamos esse padrão ou começamos com monólito modular.

Opções:

1. **Microsserviços puros** — 1 API por módulo, deploy independente, DBs próprios.
2. **Monólito modular** — 1 API com pastas por módulo, deploy único, DB compartilhado.
3. **Modular monolith → microservices** — começar como (2), extrair quando dor aparecer.

## Comparação

| Critério | Microsserviços | Monólito modular |
|---|---|---|
| Tempo até primeiro deploy | Alto | Baixo |
| Custo de infra inicial | Alto (N services + ferramentas) | Baixo (1 service) |
| Deploy independente | Sim | Não |
| Transações cross-módulo | Difícil (sagas, 2PC) | Trivial |
| Onboarding de dev | Difícil | Fácil |
| Time pequeno (< 5) | Sofre | Confortável |
| Time grande (10+) | Escala | Vira gargalo |
| Observabilidade necessária | Distribuída (traces) | Simples |
| Refatoração de schema | Coordenação | Direta |

## Decisão

**Opção 3: Monólito modular agora, com **fronteiras claras** entre módulos preparando para extração futura.**

## Justificativa

- Time inicial pequeno (1-2 devs) — microsserviços custam caro.
- Domínio ainda em descoberta — fronteiras certas vão se revelar com uso real.
- Time-to-market do MVP é prioridade.
- Frontend **já nasce dividido** por subdomínio (vantagens de bundle isolado sem custo de microsserviço no back).
- Quando precisarmos extrair (ex: Documentos virar serviço dedicado por escala), as fronteiras já estarão prontas.

## Como manter "fronteiras"

Estrutura no monólito:

```
apps/api/src/
├── modules/
│   ├── admin/
│   │   ├── routes.ts
│   │   ├── service.ts
│   │   ├── repo.ts
│   │   └── schema.ts
│   ├── docs/
│   ├── nc/
│   ├── opp/
│   └── risk/
├── shared/
│   ├── auth/
│   ├── audit/
│   ├── notifications/
│   └── storage/
└── server.ts
```

Regras de boundary:

1. **Imports proibidos**: `modules/X/...` **NUNCA** importa de `modules/Y/...`. Linter custom enforça.
2. **Comunicação cross-module**: apenas via interface pública (`modules/X/api.ts`) ou via eventos.
3. **DB**: cada módulo tem seu prefixo de tabelas (`document_*`, `nc_*`, `risk_*`). Nada de FK cross-module direta — usar IDs e validar em service.
4. **Auth e audit** são compartilhados (vivem em `shared/`).

## Consequências

### Positivas
- Deploy simples, 1 imagem Docker.
- Latência baixa (sem network overhead inter-service).
- DX boa para onboarding.
- Custo de infra mínimo.

### Negativas / Mitigações
- **Acoplamento acidental** — mitigado pelas regras de boundary + linter.
- **Deploy bloqueia tudo** — aceitável no início; quando incomodar, é sinal de extrair módulo.
- **Schema único pode crescer** — mitigado por organização clara e migrations por módulo.

## Quando extrair um módulo para microsserviço

Critérios:
- Módulo tem performance/escala muito diferente do resto.
- Time de 5+ devs trabalhando no mesmo módulo.
- Requisito de deploy independente real (compliance, SLA).

Sinais para **NÃO** extrair:
- "É moderno fazer microsserviços".
- "Vai escalar muito" (sem evidência).
- "Cada um quer sua linguagem".

## Revisão

Reavaliar em **12 meses** ou quando algum critério acima for atingido.
