# Busca global

Você pode buscar **qualquer coisa** no sistema sem precisar lembrar em qual módulo está.

## Atalho

Aperte `S` ou `/` em qualquer tela (sem estar dentro de um campo de texto). Abre a busca global.

## O que ela procura

| Tipo | Exemplo de match |
|---|---|
| **Documentos** | Título, código (POP0001), categoria, conteúdo de campos personalizados |
| **Ocorrências / RNCs** | Descrição, código, origem |
| **Oportunidades** | Descrição, código |
| **Riscos** | Descrição, categoria |
| **Pessoas** | Nome, e-mail (se você tem permissão de ver usuários) |
| **Unidades** | Nome, sigla |
| **Processos** | Nome, sigla |

## Como aparecem os resultados

```
┌─────────────────────────────────────────────────┐
│ 🔍 [ derramamento óleo                       ✕ ]│
├─────────────────────────────────────────────────┤
│ NÃO CONFORMIDADES                                │
│   📋 Ocorrência #042 — Derramamento óleo na...  │
│   📋 RNC #018 — Derramamento de óleo lubrifi... │
│                                                  │
│ DOCUMENTOS                                       │
│   📄 POP0007 — Procedimento para derramamento... │
│                                                  │
│ RISCOS                                           │
│   🎯 Risco — Derramamento de óleo (Fáb. Berlim) │
└─────────────────────────────────────────────────┘
```

- Resultados agrupados por módulo.
- Clicar em um resultado leva direto ao detalhe do registro.
- Mostra os **15 mais relevantes** — para mais resultados, ir na Consulta do módulo correspondente.

## Operadores de busca

| Operador | Exemplo | O que faz |
|---|---|---|
| Termo simples | `vazamento` | Busca em qualquer campo |
| Várias palavras | `vazamento bombona` | Busca registros que contenham **ambas** (ordem não importa) |
| Aspas | `"derramamento de óleo"` | Busca a frase exata |
| Negação | `vazamento -teste` | Inclui "vazamento", **exclui** "teste" |
| Por módulo | `modulo:nc derramamento` | Restringe ao módulo NC |
| Por status | `status:aberto` | Filtra por status |

## Permissão

Você só vê resultados de registros que **tem permissão para ler**. Documentos confidenciais que seu grupo não tem acesso simplesmente não aparecem na busca.

## Histórico de buscas

A busca lembra suas últimas 10 pesquisas. Aparece uma lista de "Buscas recentes" quando você abre a busca sem digitar nada.

## Atalhos dentro da busca

| Tecla | Ação |
|---|---|
| `↑` `↓` | Navegar resultados |
| `Enter` | Abrir resultado selecionado |
| `Esc` | Fechar busca |
| `Cmd/Ctrl + Enter` | Abrir resultado em nova aba |
