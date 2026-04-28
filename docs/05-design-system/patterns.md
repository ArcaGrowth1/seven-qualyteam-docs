# Design System — Patterns

Padrões de interação que se repetem em todo o app.

## Layout base

- **Container**: max-width `1280px` com padding lateral `24px` (mobile) / `48px` (desktop).
- **Grid**: 12 colunas com gap `24px`.
- **Breakpoints**: `sm 640 / md 768 / lg 1024 / xl 1280 / 2xl 1536`.

## Estados de tela

Toda lista mestra deve cobrir 4 estados:

| Estado | Quando | UI |
|---|---|---|
| **Loading** | Primeiro fetch | Skeleton rows (não spinner) |
| **Vazio (zero)** | Sem dados ainda | Ilustração + CTA |
| **Vazio (filtro)** | Filtros não retornam nada | Mensagem "Sem resultados" + botão "Limpar filtros" |
| **Preenchido** | Caso normal | Tabela + paginação |
| **Erro** | API falha | Mensagem + botão "Tentar novamente" |

## Confirmação de ações destrutivas

Toda ação irreversível (deletar, inativar) abre **AlertDialog** com:
- Título descritivo ("Excluir documento POP-001?")
- Descrição explicando consequência
- Botão "Cancelar" (default focus)
- Botão "Excluir" (destrutivo, vermelho)

```tsx
<AlertDialog>
  <AlertDialogTitle>Excluir POP-001 permanentemente?</AlertDialogTitle>
  <AlertDialogDescription>
    Esta ação não pode ser desfeita. As cópias controladas distribuídas
    serão revogadas automaticamente.
  </AlertDialogDescription>
  <AlertDialogCancel>Cancelar</AlertDialogCancel>
  <AlertDialogAction variant="destructive">Excluir</AlertDialogAction>
</AlertDialog>
```

## Toasts

- **Sucesso**: top-right, auto-dismiss 4s, ícone verde.
- **Erro**: top-right, dismiss manual, ícone vermelho.
- **Info**: top-right, auto-dismiss 6s, ícone azul.

Stack: `sonner` (preferido) ou `react-hot-toast`.

## Validação de formulários

- **Inline**: erros aparecem abaixo do campo após blur ou submit.
- **Resumo no topo do form**: lista todos os erros se o form for grande.
- **Submit desabilitado** até campos obrigatórios estarem preenchidos.
- **Loading do submit**: botão vira spinner com label "Salvando…".

## Drag and drop

Padrão usado em:
- Upload de anexos
- Ordenação de itens (matriz, listas configuráveis)

UI esperada:
- Cursor muda para `grab` ao hover, `grabbing` ao arrastar.
- Drop zone destacada com borda tracejada e cor de fundo levemente alterada.
- Mensagem "Solte aqui" durante drag-over.

## Paginação

```
                                    [< 1 2 3 ... 12 >]   [10 ▾ por página]
```

- Default 25 por página, opções: 10, 25, 50, 100.
- Botões "Anterior" / "Próxima" desabilitados nas extremidades.
- Truncar com `...` quando há muitas páginas.

## Sort

- Coluna sortável tem chevron pequeno ao lado do label.
- Clique alterna `asc` / `desc` / `none` (3 estados).
- Sort persiste na URL (`?sort=name&dir=asc`).

## Filtros

- Filtros aplicados aparecem como **chips** acima da tabela.
- Cada chip tem `✕` para remover individualmente.
- Botão "Limpar filtros" remove todos.
- Filtros persistem na URL (queryparam) — copiar URL = recriar visão.

## Breadcrumbs

Quando profundidade > 2 níveis:
```
Documentos / Consulta / POP-001 / Revisão 02
```
Cada nível clicável (exceto o atual).

## Saved views (futuro)

Listas mestras com filtros frequentes podem virar "views salvas" — chip especial com nome ("Minhas pendências", "Vencendo este mês").

## Mobile responsivo

- Top bar vira hambúrguer < `md`.
- Listas mestras viram cards empilhados.
- Wizard mantém steps mas com layout vertical.
- PDF viewer tem toolbar minimalista.

## Acessibilidade

- Contraste mínimo WCAG AA (4.5:1).
- Foco visível em todo elemento interativo.
- Navegação por teclado (Tab, Enter, Esc).
- Labels em todo input (não apenas placeholder).
- Aria-labels em ícones-only buttons.
- Error messages associados via `aria-describedby`.

## Performance

- Lazy load de listas com paginação server-side.
- Imagens com `next/image` + lazy.
- PDFs renderizam sob demanda (não pré-carrega o arquivo todo se não foi clicado).
- Skeleton em vez de spinner para reduzir CLS.
- React Server Components quando possível para reduzir JS no cliente.

## Optimistic UI

Em ações simples (toggle status, criar tarefa quick-add):
1. Atualizar UI imediatamente.
2. Disparar request.
3. Reverter + toast de erro se falhar.

Em ações complexas (criar documento, aprovar), preferir loading state explícito.

## Auto-save vs save explícito

- **Wizard de cadastro**: rascunho em `localStorage` (24h TTL) automaticamente. Persistir no servidor só ao "Gravar".
- **Edição inline em listas**: auto-save com debounce 1s + toast "Salvo".
- **Edição de detail panel**: save explícito com botão "Gravar".

## Dirty state guard

Se user tem mudanças não salvas e tenta navegar/fechar:
```
"Você tem alterações não salvas. Sair mesmo assim?"
[Cancelar]  [Sair sem salvar]
```
