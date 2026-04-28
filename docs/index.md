# Seven Qualyteam Docs

Documentação completa do sistema. Duas frentes:

## 📘 Manual Funcional — para entender como usar o produto

**[Acesse o Manual Funcional →](manual/index.md)**

Tela por tela, página por página: o que aparece, como criar, como filtrar, como tudo se conecta. Comece por aqui se você quer **operar o sistema**.

Inclui:

- **[Conceitos básicos](manual/0-conceitos/navegacao-geral.md)** — padrões que se repetem em todo app (listas, filtros, anexos, notificações, busca, permissões).
- **[Administrador](manual/1-administrador/visao-geral.md)** — usuários, grupos, processos, unidades, campos personalizados, audit log.
- **[Documentos](manual/2-documentos/visao-geral.md)** — POPs, licenças, ciclo de elaboração, validade, leitura obrigatória.
- **[Não Conformidades](manual/3-nao-conformidades/visao-geral.md)** — ocorrências e RNCs com fluxo de 6 etapas.
- **[Oportunidades](manual/4-oportunidades/visao-geral.md)** — captação de melhorias com matriz P × I.
- **[Riscos](manual/5-riscos/visao-geral.md)** — gestão por unidade, ISO 31000.
- **[Implantação Seven do dia 1](manual/6-fluxos-completos/implantacao-seven.md)** — passo a passo para começar do zero.

## 🛠 Visão técnica — para o time que vai construir o clone

**[Acesse a Visão técnica →](00-overview.md)**

Arquitetura, modelo de dados, padrões de código, decisões registradas. Comece por aqui se você é **dev**.

Inclui:

- **[Arquitetura](01-architecture/system-context.md)** — containers, stack, infraestrutura.
- **[Domínio](02-domain/glossary.md)** — glossário, ERD, permissões, multi-tenancy.
- **[Módulos (técnico)](03-modules/admin/overview.md)** — endpoints, state machines, eventos.
- **[Cross-cutting](04-cross-cutting/auth.md)** — auth, audit, notificações, storage.
- **[Design System](05-design-system/components.md)** — componentes, patterns, tokens.
- **[Roadmap e ADRs](06-roadmap/mvp.md)** — MVP, fase 2, decisões.

---

## Atalhos do site

| Tecla | Ação |
|---|---|
| `s` ou `/` | Buscar |
| `n` / `p` | Próxima / anterior |
| `f` | Foco no menu |

## Quem mantém

Time da Seven + ArcaGrowth. Owner inicial: [@ArcaGrowth1](https://github.com/ArcaGrowth1).
