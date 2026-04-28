# Manual Funcional

Esta seção descreve **como o produto funciona** — tela por tela, do ponto de vista de quem vai **usar** o sistema (não construir).

Para cada tela você vai encontrar:

| Item | O que é |
|---|---|
| **Onde fica** | Caminho do menu para chegar na tela |
| **Quem acessa** | Permissões necessárias |
| **O que aparece** | Descrição do que o usuário vê |
| **Cada elemento** | O que cada bloco/coluna significa, de onde vem |
| **Filtros disponíveis** | Cada filtro e o que exatamente ele filtra |
| **Cada botão** | O que faz, para onde leva |
| **De onde vêm os dados** | Qual tela ou cadastro alimenta esta |
| **Para onde levam as ações** | Qual tela é o destino do que foi feito aqui |
| **Notificações** | O que dispara e-mail / sino, quando, pra quem |
| **Estados especiais** | Como fica quando vazia, com erro, sem permissão |
| **Exemplo Seven** | Caso prático da Seven Resíduos |

## Como ler

1. Comece por **[Conceitos básicos](0-conceitos/navegacao-geral.md)** — explica padrões que se repetem em todo o app (lista mestra, filtros, anexos, notificações). Lendo isso, todo o resto fica fácil.
2. Depois siga pelo módulo do seu interesse.
3. **Fluxos completos** no final mostram jornadas do começo ao fim (ex: "do registro de uma ocorrência até a verificação de eficácia").

## Atalho por papel

| Sou… | Comece por… |
|---|---|
| Admin do tenant | Conceitos → Administrador |
| Coord. ambiental | Conceitos → Documentos → NC → Riscos |
| Operacional de campo | Conceitos → Documentos (consulta) → NC (registrar ocorrência) |
| Auditor | Conceitos → Consulta de cada módulo |

## Convenções

- **Negrito** = botão ou label da interface.
- `Código` = caminho de URL ou nome técnico.
- 🔵 = ação principal · 🟡 = ação secundária · 🔴 = ação destrutiva.
- Permissões aparecem como `modulo.acao` (ex: `docs.internal.create`).
