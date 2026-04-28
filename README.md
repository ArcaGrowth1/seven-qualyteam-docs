# Seven SGQ — Documentação

Documentação técnica do **Sistema de Gestão da Qualidade / Ambiental** da Seven Resíduos.

Este repositório é a **fonte da verdade** para arquitetura, domínio, módulos e decisões. PRs de código que afetem comportamento do sistema **devem** atualizar a doc relevante.

## Como navegar

| Pasta | Para quem | Quando ler |
|---|---|---|
| [`00-overview.md`](docs/00-overview.md) | Todos | Primeiro contato |
| [`01-architecture/`](docs/01-architecture/) | Devs, SRE | Antes de codar / antes de mudar infra |
| [`02-domain/`](docs/02-domain/) | Todos | Quando aparecer termo desconhecido |
| [`03-modules/`](docs/03-modules/) | Devs do módulo | Trabalhando em uma feature do módulo |
| [`04-cross-cutting/`](docs/04-cross-cutting/) | Devs | Tópicos que atravessam módulos (auth, audit log, etc.) |
| [`05-design-system/`](docs/05-design-system/) | Front, design | Construindo UI |
| [`06-roadmap/`](docs/06-roadmap/) | Todos | Planejamento, decisões registradas |

## Stack técnica (resumo)

- **Frontend**: Next.js + TypeScript
- **Backend**: APIs REST por módulo (microsserviços ou monólito modular — ver ADR-0002)
- **DB**: PostgreSQL multi-tenant
- **Storage**: S3 / R2
- **Auth**: cookie httpOnly cross-subdomain

Detalhes em [`01-architecture/tech-stack.md`](docs/01-architecture/tech-stack.md).

## Diagramas

Usamos **Mermaid** — renderiza nativamente no GitHub. Se for editar, use o [Mermaid Live Editor](https://mermaid.live) para previsualizar.

## Contribuindo

1. Para cada PR de código que mude comportamento, atualize a doc do módulo afetado.
2. Para decisões arquiteturais relevantes, abra um ADR em [`06-roadmap/adrs/`](docs/06-roadmap/adrs/) seguindo o template.
3. Para novos módulos, copie [`03-modules/_template.md`](docs/03-modules/_template.md).

## Inspiração funcional

A análise inicial deste produto foi feita a partir do **QualyTeam**. Documentamos o que aprendemos como referência funcional, mas a implementação Seven é independente. Detalhes em [`00-overview.md`](docs/00-overview.md).
