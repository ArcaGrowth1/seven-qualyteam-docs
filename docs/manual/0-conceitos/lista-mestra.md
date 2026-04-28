# Como funcionam as listas (Consulta, Tarefas, etc.)

Todas as listas do sistema (Consulta de Documentos, lista de RNCs, lista de Oportunidades, lista de Tarefas, etc.) **funcionam do mesmo jeito**. Aprendendo uma, você sabe operar todas.

## Anatomia padrão

```
┌─────────────────────────────────────────────────────────────────┐
│ Título da lista                  [+ Novo] [Filtros] [Exportar][⋮]│
│ Total: 42                                                        │
├─────────────────────────────────────────────────────────────────┤
│ [chip: Filtro 1 ✕]  [chip: Filtro 2 ✕]   ← filtros aplicados   │
├─────────────────────────────────────────────────────────────────┤
│ Coluna A ↓ │ Coluna B ↓ │ Coluna C ↓ │ Coluna D │ Ações          │
│ ────────── │ ────────── │ ────────── │ ──────── │                │
│ valor      │ valor      │ valor      │ valor    │ ✏ 👁 🗑          │
│ valor      │ valor      │ valor      │ valor    │ ✏ 👁 🗑          │
└─────────────────────────────────────────────────────────────────┘
                                                  [< 1 2 3 ... 12 >]
```

## Cabeçalho da lista

| Elemento | O que faz |
|---|---|
| **Título** | Nome da lista (ex: "Documentos internos", "Tarefas") |
| **Total: N** | Quantidade total **considerando os filtros aplicados** |
| Botão **+ Novo** (ou similar) | Abre o formulário de cadastro daquele item (só aparece se você tem permissão de criar) |
| Botão **Filtros** | Abre o painel de filtros à direita |
| Botão **Exportar** | Gera CSV/Excel do que está sendo mostrado (só se você tem permissão de exportar daquele módulo) |
| Botão **⋮** (três pontos) | Ações secundárias: imprimir, configurar colunas visíveis, etc. |

## Filtros aplicados (chips)

Quando você aplica um filtro, ele aparece como um **chip** acima da tabela:
```
[chip: Beatriz Brito ✕]  [chip: Status: Aberta ✕]
```
- Cada chip mostra o filtro ativo.
- O `✕` no chip remove **só aquele filtro**.
- Botão "Limpar filtros" (no painel de filtros) remove todos.

## Cabeçalho das colunas (sort)

Cada coluna tem uma seta `↓` ou `↑`:
- Clicar **alterna** entre 3 estados: `↑` (crescente) → `↓` (decrescente) → sem ordenação.
- Apenas uma coluna por vez fica ordenada.
- A ordenação **persiste na URL** — copiar a URL preserva a ordenação.

## Linhas

- **Clique simples** na linha: abre o detalhe do item em uma nova página.
- **Hover**: linha fica cinza claro.
- Ações por linha (✏ editar, 👁 visualizar, 🗑 excluir) aparecem **se você tem permissão** para aquela ação naquele item.

## Paginação

```
                              [< 1 2 3 ... 12 >]   [25 ▾ por página]
```
- Default: 25 itens por página.
- Opções: 10, 25, 50, 100.
- Setas `<` e `>` ficam desabilitadas nas extremidades.
- Mudar a quantidade volta para a página 1.

## Estados especiais

### 1. Vazio (nenhum item cadastrado ainda)
```
              [📁 ilustração]

   Você ainda não tem [recurso].
   Clique abaixo para adicionar.

       [ + Adicionar [recurso] ]
```
Aparece **só** quando o módulo está completamente vazio (sem dados nenhum). Se tem itens mas filtros não retornam, vai pro estado abaixo.

### 2. Vazio por filtro
```
              [📁 ilustração]

      Sem resultados para os filtros aplicados.

           [ Limpar filtros ]
```

### 3. Carregando
Linhas em "esqueleto" (placeholder cinza pulsando) enquanto busca os dados.

### 4. Erro
```
   Não foi possível carregar a lista.

         [ Tentar novamente ]
```
Aparece se a API falhou. Geralmente é problema de rede; "Tentar novamente" geralmente resolve.

## Ações em massa

Em algumas listas (como Documentos), você pode selecionar várias linhas via checkbox e aplicar uma ação a todas:
```
[ ☑ ] Coluna A │ Coluna B │ ...
[ ☐ ] valor    │ valor    │ ...
[ ☑ ] valor    │ valor    │ ...

3 selecionados   [ Distribuir cópia controlada ▾ ] [ Exportar selecionados ]
```

## Onde os dados vêm

Cada lista mestra é alimentada por seus respectivos **cadastros**:
- Lista de Documentos ← preenchida quando alguém usa **Documento interno** ou **Documento externo**.
- Lista de RNCs ← preenchida quando alguém usa **Registrar não conformidade**.
- Lista de Tarefas (Documentos) ← preenchida automaticamente quando você é definido como **Elaborador** ou **Aprovador** de um documento.

Em cada tela específica do manual está descrito **exatamente** quais cadastros alimentam a lista.

## Permissão para ver/exportar

Você só vê uma lista se tem a permissão `modulo.read` (ex: `docs.read`, `nc.rnc.read`).
Você só exporta se tem `modulo.export` (ex: `docs.export`, `nc.rnc.export`).
Você só vê **tarefas dos outros** se tem `modulo.tasks.read_all` — caso contrário só vê as suas.
