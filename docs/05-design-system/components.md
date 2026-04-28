# Design System — Componentes

Catálogo de componentes reutilizáveis. Stack: **shadcn/ui + Tailwind**. Tokens em [`tokens.md`](tokens.md).

## TopBar

```
┌──────────────────────────────────────────────────────────────────────┐
│ [Logo] │ [Módulo▾] │ [Nav1] [Nav2] [Nav3] │              [🔔10][⚙][👤] │
└──────────────────────────────────────────────────────────────────────┘
```

**Props**:
- `module: 'admin' | 'docs' | 'nc' | 'opp' | 'risk'`
- `nav: NavItem[]`
- `user: { name, email, avatarUrl }`

**Comportamento**:
- Switcher de módulo abre dropdown com **todos os módulos contratados pelo tenant** (não os fixos).
- Bell mostra contador de notificações in-app não-lidas (badge azul). Clicar abre painel lateral.
- Engrenagem mostra dropdown com atalhos do Admin (apenas se user tem permissão).
- Avatar mostra dropdown com nome, e-mail e "Sair".

## Wizard (multi-step)

```
            ┌── 1. Etapa A ──┬── 2. Etapa B ──┬── 3. Opcional ──┐
            │      ●         │       2        │       3         │
            └────────────────┴────────────────┴─────────────────┘

[ campos do step ativo... ]

                    [Voltar]   [Próximo / Gravar]
```

**Props**:
- `steps: { id, label, optional?: boolean, isValid?: () => boolean }[]`
- `current: number`
- `onNext`, `onBack`, `onSubmit`

**Comportamento**:
- Botão "Próximo" só habilita se `isValid(current)` retorna true.
- Steps com `optional: true` mostram "(opcional)" no label.
- Persistência local: salva rascunho em `localStorage` com TTL de 24h.

## Lista mestra (DataTable)

```
┌──────────────────────────────────────────────────────────────────┐
│ Total: 42                            [+ Novo] [Filtros] [Exportar][⋮]│
├──────────────────────────────────────────────────────────────────┤
│ Col 1 ↓  │ Col 2 ↓  │ Col 3 ↓  │ Col 4 ↓  │ Ações                │
│ ──────── │ ──────── │ ──────── │ ──────── │                       │
│ valor    │ valor    │ valor    │ valor    │ ✏ 👁 🗑                │
└──────────────────────────────────────────────────────────────────┘
                                                  [< 1 2 3 ... >]
```

**Props**:
- `columns: { key, label, sortable, render?: (row) => ReactNode }[]`
- `data: Row[]`
- `pagination: { page, pageSize, total }`
- `actions: Action[]` (Novo, Filtros, Exportar, custom)
- `rowActions: (row) => Action[]`
- `onRowClick: (row) => void`

**Comportamento**:
- Sort visual com chevron na coluna ativa.
- Filtros abrem drawer lateral com chips do filtro aplicado em cima da tabela.
- Estado vazio mostra ilustração + CTA.
- Loading: skeleton rows.

## Filtros (chips + drawer)

```
┌──────────────────────────────────────────────────────────────────┐
│ [chip: Beatriz Brito ✕] [chip: Status: Aberta ✕]   [+ Filtros]    │
└──────────────────────────────────────────────────────────────────┘
```

**Comportamento**:
- Cada filtro aplicado vira um chip com `✕` para remover.
- "+ Filtros" abre drawer com formulário de filtros disponíveis.

## Form fields

| Componente | shadcn base | Notas |
|---|---|---|
| `<TextField>` | Input | Obrigatório com `*`, contador `0/N`, helper text |
| `<TextArea>` | Textarea | Igual TextField, multi-line |
| `<Select>` | Select | Com busca quando >10 opções |
| `<MultiSelect>` | Combobox | Chips de seleção, contagem "3 selecionados" |
| `<DatePicker>` | DayPicker | Format `DD/MM/AA`, validação de range |
| `<Toggle>` | Switch | Para flags binárias |
| `<RadioGroup>` | RadioGroup | Para 2-4 opções mutuamente exclusivas |
| `<FileDrop>` | (custom) | Drag-drop + click. Mostra progress por file |

## Empty state

```
                          [📁]

              Você ainda não tem <recurso>.
                Clique abaixo para criar.

                  [ Criar <recurso> ]
```

**Props**:
- `illustration: 'folder' | 'chart' | 'inbox'`
- `title: string`
- `description?: string`
- `cta?: { label, onClick }`

## PDF Viewer

**Stack**: PDF.js wrapper.

Toolbar:
- Voltar (ao listing)
- Fullscreen
- Toggle thumbnails
- Rotação
- Zoom in/out
- Paginação (input "1 / 42")
- Print
- Download (se permitido)
- Toggle painel de detalhes

## Detail panel (lateral colapsável)

```
┌─────────────────────────┐
│ <Título do registro>    │
│ <Subtítulo>             │
│                         │
│ [ Ação primária ]       │
│ [ Ação secundária ]     │
│   Ação destrutiva       │
│                         │
│ ▾ Seção 1               │
│ ▾ Seção 2               │
│ ▾ Seção 3               │
│                         │
│        [Compartilhar]   │
└─────────────────────────┘
```

## Matrix selector (Probabilidade × Impacto)

Componente customizado para Oportunidades e Riscos.

```
                Probabilidade ↑
             ┌───────┬───────┬───────┐
        Alta │   ?   │   ?   │   ?   │
             ├───────┼───────┼───────┤
    Moderada │   ?   │   ?   │   ?   │
             ├───────┼───────┼───────┤
       Baixa │   ?   │   ?   │   ?   │
             └───────┴───────┴───────┘
              Baixo Moderado  Alto
                  Impacto →
```

**Props**:
- `size: 3 | 4 | 5`
- `xLabels: string[]`, `yLabels: string[]`
- `quadrants: Action[][]` (matriz NxN)
- `editable: boolean`
- `onCellChange?: (x, y, action) => void`
- `selectedCell?: { x, y }` (para visualização do score)

**Comportamento**:
- Se `editable`, clique numa célula abre popover para escolher ação.
- Drag-to-paint: arrasta com mouse para pintar várias células iguais.
- Cores: Implementar/Reter (verde), Ponderar/Mitigar (amarelo), Arquivar/Evitar (vermelho).

## Notification badge / panel

```
🔔 (10)        ←  badge azul com contagem

Painel:
┌─────────────────────────┐
│ Notificações            │
├─────────────────────────┤
│ 🔵 Você foi designado   │
│    elaborador do POP-42 │
│    há 2 minutos         │
├─────────────────────────┤
│ ⚪ RNC #123 aprovada    │
│    há 1 hora            │
└─────────────────────────┘
```

## Tarefa card / list item

```
┌──────────────────────────────────────────────────┐
│ #DOC-42 · POP Procedimento de Tratamento     [⋮] │
│ Categoria: POP · Unidade: Matriz                  │
│ Atribuída a você · prazo em 5 dias                │
└──────────────────────────────────────────────────┘
```

## Dashboard widget

```
┌──────────────────────────────────────────┐
│ <Título do widget>                  [⋮]  │
│                                          │
│        [gráfico ou KPI]                  │
│                                          │
│ filtro / detalhe                         │
└──────────────────────────────────────────┘
```

**Tipos**:
- KPI (número grande + delta vs período anterior)
- Bar chart
- Pie / donut
- Line (séries temporais)
- Heatmap (para matriz de risco)
- Tabela compacta (top N)

**Stack**: `recharts` ou `tremor` (preferido por integração shadcn).
