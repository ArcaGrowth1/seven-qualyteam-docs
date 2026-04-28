# Contribuindo

## Princípio

Esta doc é **viva**. PRs de código que mudem comportamento devem atualizar a doc relevante. Doc desatualizada é pior que sem doc.

## Setup local

```bash
git clone https://github.com/<org>/seven-qualyteam-docs.git
cd seven-qualyteam-docs
```

Não há build — Markdown puro + Mermaid. Recomendado abrir no VS Code com extensão **Markdown Preview Mermaid Support**.

## Como escrever

### Estrutura

Siga a árvore em `docs/`:
- `00-overview.md` — visão de produto
- `01-architecture/` — arquitetura
- `02-domain/` — domínio compartilhado
- `03-modules/<modulo>/` — 1 pasta por módulo
- `04-cross-cutting/` — temas transversais
- `05-design-system/` — UI
- `06-roadmap/` — roadmap e ADRs

### Diagramas

Usamos **Mermaid**. Renderiza no GitHub e na maioria dos editores.

```markdown
\`\`\`mermaid
flowchart LR
    A --> B
\`\`\`
```

Testar no [Mermaid Live](https://mermaid.live) antes de commitar.

### Tom

- **Direto**. Frases curtas. Listas onde fizer sentido.
- **Visual primeiro**. Diagrama antes da prosa.
- **Linkar** entre arquivos sempre que mencionar conceito de outro.
- **Datado**. ADRs e decisões importantes têm data.

## Workflow de PR

1. Cria branch `doc/<assunto>` ou `adr/NNNN-<titulo>`.
2. Faz mudanças.
3. Abre PR usando o template.
4. Revisão de pelo menos 1 owner (CODEOWNERS).
5. Merge → main.

## Padrões de commit

Convenções (não obrigatório, ajuda):

- `docs: <descrição>` — mudança de doc.
- `adr: NNNN <título>` — novo ADR.
- `fix: link quebrado em ...`
- `chore: ...` — coisas de repo (CI, templates).

## Convenções

- Arquivos em `kebab-case.md`.
- Headings começam em `#` (h1) — 1 por arquivo.
- Listas com `-` (não `*`).
- Code blocks com linguagem (` ```ts `, ` ```sql `, etc.).

## Dúvidas

Abra issue com label `question` ou pergunte no canal do time.
