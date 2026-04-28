# Riscos — Configurar unidade

## Onde fica

`Riscos → Unidades → botão "Configurar unidade"` (URL: `/units/configure`)

> ⚠️ **Obrigatório antes de identificar qualquer risco**. Se você acessar Tarefas / Consulta / Identificar risco sem ter configurado a primeira unidade, o sistema te leva para o setup.

## Quem acessa

`risk.unit_config.create` (criar nova) / `risk.unit_config.update` (editar existente).

## Por que configurar por unidade

Cada unidade tem **apetite ao risco diferente**. Não faz sentido aplicar a mesma matriz para a Matriz administrativa (poucos riscos, baixo impacto) e para uma fábrica de tratamento de resíduos perigosos (muitos riscos, alto impacto).

## A tela: 3 blocos lado a lado

```
┌──────────────────────────────────────────────────────────────────────┐
│ Configuração da unidade                                               │
│                                                                       │
│ ┌── (1) Informações Básicas ──────┐  ┌── (3) Apetite ao risco ────┐ │
│ │                                  │  │                              │ │
│ │ Unidade organizacional *         │  │ [3x3] [4x4] [5x5]            │ │
│ │ [ Selecione                  ▾ ] │  │ | [Editar rótulos eixos]     │ │
│ │                                  │  │                              │ │
│ │ Responsável *                    │  │ ⓘ Ao finalizar a configura-  │ │
│ │ [ Selecione                  ▾ ] │  │   ção da unidade não será    │ │
│ │                                  │  │   possível alterar o modelo  │ │
│ ├── (2) Reavaliação dos riscos ──┤  │   do apetite ao risco.    ✕  │ │
│ │                                  │  │                              │ │
│ │ Período para reavaliação *       │  │  Probab.↑                    │ │
│ │ [ Selecione                  ▾ ] │  │  ┌────┬────┬────┐            │ │
│ │   ⚙ 12 meses                    │  │  │Mit.│Evit│Evit│  Alta       │ │
│ │   ⚙ 9 meses                     │  │  ├────┼────┼────┤            │ │
│ │   ⚙ 6 meses                     │  │  │Ret.│Mit.│Evit│  Mod        │ │
│ │   ⚙ 3 meses                     │  │  ├────┼────┼────┤            │ │
│ │   ⚙ Personalizado                │  │  │Ret.│Ret.│Mit.│  Baixa      │ │
│ │                                  │  │  └────┴────┴────┘            │ │
│ │                                  │  │   Baixo Mod  Alto            │ │
│ │                                  │  │       Impacto →              │ │
│ │                                  │  │                              │ │
│ └──────────────────────────────────┘  └──────────────────────────────┘ │
│                                                                       │
│                                          [ Cancelar ]   [ Próximo ]   │
└──────────────────────────────────────────────────────────────────────┘
```

## Bloco 1: Informações Básicas

### Unidade organizacional *
Dropdown com todas as unidades cadastradas no Admin.

> Cada unidade só pode ter **uma configuração ativa**. Se você seleciona uma já configurada, o sistema te leva para edição em vez de duplicar.

### Responsável *
Dropdown com usuários ativos. Geralmente: gerente da unidade, ou Coord. Qualidade da unidade.

Esta pessoa:
- É notificada de novos riscos identificados na unidade.
- É notificada de reavaliações.
- Vê tudo da unidade na sua aba Tarefas.

## Bloco 2: Reavaliação dos riscos

### Período para reavaliação *

Dropdown com:
- 12 meses (mais comum)
- 9 meses
- 6 meses
- 3 meses
- **Personalizado** (abre input de dias)

### O que isso controla

A cada N meses, **todos os riscos ativos da unidade** são automaticamente colocados na aba **Tarefas → Para reavaliação** do Responsável da unidade.

A pessoa precisa abrir cada risco e:
- Reavaliar P × I (pode ter mudado).
- Confirmar se o tratamento ainda é o mesmo.
- Adicionar comentários do período.

## Bloco 3: Apetite ao risco

### Tamanho da matriz
3 botões: 3x3 / 4x4 / 5x5.

### Editar rótulos dos eixos e quadrantes
Abre painel para customizar (mesmo padrão de Oportunidades — ver [Configuração de Oportunidades](../4-oportunidades/configuracao-inicial.md)).

### Atribuir ações aos quadrantes
Clicar numa célula abre popover:

```
Selecione tratamento:
⊙ Reter (verde)
⊙ Mitigar (amarelo)
⊙ Evitar (vermelho)
[ adicional 5x5: Transferir, Compartilhar ]
```

#### Cores
- 🟢 Reter — verde
- 🟡 Mitigar — amarelo
- 🔴 Evitar — vermelho
- 🔵 Transferir / Compartilhar — azul (apenas em 5x5)

### Drag-to-paint
Arrasta com mouse para pintar várias células iguais.

## ⚠️ Restrição importante após primeiro risco

Quando o **primeiro risco** for identificado nessa unidade, o **modelo do apetite trava**:

```
ⓘ Ao finalizar a configuração da unidade não será possível alterar
   o modelo do apetite ao risco.
```

**Permanece editável**:
- ✅ Rótulos de eixos.
- ✅ Rótulos de níveis.
- ✅ Ação em cada quadrante (mas mexer aqui muda tratamento de **todos** os riscos existentes — cuidado).
- ✅ Período de reavaliação.
- ✅ Responsável.

**Trava**:
- ❌ Tamanho da matriz (3x3 → 5x5).
- ❌ Adicionar/remover níveis (eixo de 3 → 4 níveis).

## Botão Próximo / Gravar

Após preencher os 3 blocos:
1. Clica **Próximo** (ou Gravar dependendo do passo).
2. Configuração é salva.
3. Toast verde: "Unidade Matriz configurada com sucesso".
4. Volta para a lista de Unidades.
5. A unidade agora aparece como "Ativa" na lista.

## Lista de Unidades configuradas

```
Unidades                           [+ Configurar unidade]    [Filtros]

│ Unidade           │ Responsável     │ Período │ Status │ Riscos │ ✏ │
│ Matriz            │ Beatriz Brito   │ 12m     │ Ativa  │ 7      │ ✏ │
│ CT Caieiras       │ João Operações  │ 6m      │ Ativa  │ 23     │ ✏ │
│ Base Norte        │ Maria Logística │ 9m      │ Ativa  │ 5      │ ✏ │
```

## Editar config existente

Clicar em ✏ → vai para `/units/edit/<id>` com mesmo layout, valores preenchidos.

## Excluir config

Botão "Excluir" no detalhe da configuração:
- Bloqueado se há **riscos ativos** vinculados à unidade.
- Para excluir: inativar todos os riscos primeiro.

## Notificações

| Quando | Quem |
|---|---|
| Configuração criada | Responsável da unidade (e-mail boas-vindas) |
| Configuração editada | Responsável + admin |
| Trocou Responsável | Antigo + novo |
| Tamanho/quadrantes mudou (afeta riscos) | Responsável + lista interessada |

## Audit log

- `risk.unit_config.created`
- `risk.unit_config.updated`
- `risk.unit_config.deactivated`
- `risk.unit_config.locked` (quando trava após 1º risco)

## Permissões

| Ação | Permissão |
|---|---|
| Configurar nova | `risk.unit_config.create` |
| Editar | `risk.unit_config.update` |
| Excluir | `risk.unit_config.delete` |

## Estados especiais

### Tentar configurar unidade já configurada
Sistema redireciona para edição.

### Tentar mudar tamanho com riscos existentes
Bloqueado. Mensagem: "Há N riscos cadastrados. Inative todos para mudar tamanho."

### Quadrante sem ação
Bloqueado: "Defina tratamento para todas as células antes de gravar."

## Exemplo Seven — configurar 2 unidades

### Unidade 1: Matriz administrativa

**Bloco 1**:
- Unidade: Matriz
- Responsável: Beatriz Brito

**Bloco 2**:
- Período: 12 meses (riscos administrativos mudam pouco)

**Bloco 3**:
- Tamanho: 3x3
- Eixos: Probabilidade × Impacto (default)
- Quadrantes: matriz padrão (Mitigar/Evitar/Reter conforme imagem)

Riscos típicos: Riscos legais, riscos de reputação, riscos administrativos.

### Unidade 2: CT Caieiras (centro de tratamento)

**Bloco 1**:
- Unidade: CT Caieiras
- Responsável: João Operações

**Bloco 2**:
- Período: **6 meses** (operação tem mudança rápida)

**Bloco 3**:
- Tamanho: **4x4** (quer mais granularidade)
- Eixos:
  - Probabilidade (4 níveis: Rara, Possível, Provável, Quase certa)
  - Impacto (4 níveis: Insignificante, Menor, Moderado, Crítico)
- Quadrantes: matriz mais agressiva — qualquer coisa em "Provável + Crítico" ou superior é **Evitar**.

Riscos típicos: Vazamento de resíduo Classe I, contaminação de solo, acidente de trabalho com resíduo perigoso, autuação CETESB, falha de equipamento crítico.
