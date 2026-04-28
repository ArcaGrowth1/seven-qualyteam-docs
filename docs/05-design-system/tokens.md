# Design Tokens

Tokens centralizados em `packages/ui/tokens.ts`. Tailwind config consome.

## Cores

### Brand

```ts
brand: {
  50:  '#f0fdf4',
  100: '#dcfce7',
  500: '#22c55e',  // verde Seven (a confirmar com brand)
  700: '#15803d',
  900: '#14532d',
}
```

⚠️ Confirmar paleta com identidade visual da Seven Resíduos.

### Semânticas

| Token | Light | Dark | Uso |
|---|---|---|---|
| `background` | `#ffffff` | `#0a0a0a` | fundo geral |
| `foreground` | `#0a0a0a` | `#fafafa` | texto principal |
| `muted` | `#f4f4f5` | `#27272a` | fundos secundários |
| `muted-foreground` | `#71717a` | `#a1a1aa` | texto secundário |
| `border` | `#e4e4e7` | `#27272a` | bordas |
| `success` | `#22c55e` | `#16a34a` | toasts, badges |
| `warning` | `#f59e0b` | `#d97706` | alertas |
| `destructive` | `#ef4444` | `#dc2626` | ações destrutivas |
| `info` | `#3b82f6` | `#2563eb` | informativos |

### Matriz P × I (Riscos / Oportunidades)

| Ação | Cor de fundo | Cor de texto |
|---|---|---|
| Implementar / Reter | `#86efac` (green-300) | `#14532d` |
| Ponderar / Mitigar | `#fde68a` (amber-200) | `#78350f` |
| Arquivar / Evitar | `#fca5a5` (red-300) | `#7f1d1d` |

## Tipografia

| Token | Valor | Uso |
|---|---|---|
| `font-sans` | `Inter, system-ui, sans-serif` | UI |
| `font-mono` | `JetBrains Mono, monospace` | código, IDs |
| `text-xs` | `12px / 16px` | labels secundárias |
| `text-sm` | `14px / 20px` | corpo padrão |
| `text-base` | `16px / 24px` | inputs, parágrafos |
| `text-lg` | `18px / 28px` | subtítulos |
| `text-xl` | `20px / 28px` | títulos de página |
| `text-2xl` | `24px / 32px` | título principal |
| `text-3xl` | `30px / 36px` | hero |

## Espaçamento

Sistema base 4px (Tailwind padrão).

Espaços comuns:
- `gap-2` (8px) entre itens em chips
- `gap-4` (16px) entre campos de form
- `gap-6` (24px) entre cards de dashboard
- `p-6` (24px) padding interno de card
- `py-12` (48px) padding vertical de seção

## Border radius

| Token | Valor | Uso |
|---|---|---|
| `rounded-sm` | `2px` | inputs |
| `rounded-md` | `6px` | botões, badges |
| `rounded-lg` | `8px` | cards |
| `rounded-xl` | `12px` | modais |

## Shadows

| Token | Uso |
|---|---|
| `shadow-sm` | Cards leves |
| `shadow` | Botões hover |
| `shadow-md` | Dropdowns, popovers |
| `shadow-lg` | Modais |

## Z-index

| Camada | Valor |
|---|---|
| `dropdown` | 50 |
| `sticky-header` | 40 |
| `popover` | 60 |
| `modal` | 70 |
| `toast` | 80 |
| `tooltip` | 90 |

## Animations

- `transition-colors duration-150` — hover de botões
- `transition-transform duration-200` — chevrons que giram
- `animate-pulse` — skeletons
- `animate-spin` — spinners

## Ícones

- Stack: **lucide-react** (consistente, tree-shakeable).
- Tamanho default: `16px` (inline), `20px` (botões), `24px` (headers).
- Ícones críticos em ações: `Plus`, `Edit`, `Trash`, `Eye`, `Filter`, `Download`, `Share`, `Settings`, `Bell`, `User`, `Check`, `X`.

## Dark mode

Suportado via `next-themes`. Classe `dark` no `<html>`. Switch no menu do user.

## Densidade

Default: comfortable. Opção compacta no futuro (linhas mais finas em listas mestras).
