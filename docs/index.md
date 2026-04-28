# Seven Qualyteam Docs

Documentação técnica do **Sistema de Gestão da Qualidade / Ambiental** da Seven Resíduos.

Este site é a fonte da verdade para arquitetura, domínio, módulos e decisões. PRs de código que afetem comportamento devem atualizar a doc relevante.

## Como navegar

Use a barra superior para alternar entre seções principais ou o menu lateral para detalhes.

| Seção | Quando ler |
|---|---|
| [Visão geral](00-overview.md) | Primeiro contato — entenda o produto |
| [Arquitetura](01-architecture/system-context.md) | Antes de codar / antes de mudar infra |
| [Domínio](02-domain/glossary.md) | Quando aparecer termo desconhecido |
| [Módulos](03-modules/admin/overview.md) | Trabalhando em uma feature do módulo |
| [Cross-cutting](04-cross-cutting/auth.md) | Tópicos que atravessam módulos |
| [Design System](05-design-system/components.md) | Construindo UI |
| [Roadmap](06-roadmap/mvp.md) | Planejamento e decisões registradas |

## Stack (resumo)

- **Frontend**: Next.js + TypeScript + shadcn/ui + Tailwind
- **Backend**: APIs REST (monólito modular) em Node + Fastify/NestJS
- **DB**: PostgreSQL multi-tenant com Row-Level Security
- **Storage**: S3 / R2
- **Auth**: cookie httpOnly cross-subdomain

Detalhes em [Stack técnica](01-architecture/tech-stack.md).

## Quem mantém

Time da Seven + ArcaGrowth. Owner inicial: [@ArcaGrowth1](https://github.com/ArcaGrowth1).

## Inspiração funcional

A análise inicial veio do **QualyTeam** como referência funcional — não de código. Documentamos o que aprendemos para construir nossa própria implementação, mais aderente ao segmento de **gestão de resíduos**.

---

!!! tip "Dica de navegação"
    Aperte `s` para abrir busca · `n`/`p` para próxima/anterior página · `f` para foco no menu.
