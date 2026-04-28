# Oportunidades — Configuração inicial (matriz)

## Onde fica

`Oportunidades → Configuração do módulo` (URL: `/module-configuration`)

> ⚠️ **Obrigatório antes de usar o módulo**. Se você acessar Tarefas / Consulta / Nova oportunidade sem ter configurado, o sistema te redireciona para cá com banner: "Você ainda não realizou a configuração geral. Clique no botão abaixo para realizá-la."

## Quem acessa

`opp.config.update`. Geralmente: Coord. Qualidade.

## A tela

```
┌──────────────────────────────────────────────────────────────────────┐
│ Configuração do módulo                                                │
│                                                                       │
│            Critérios para oportunidades  ⓘ                            │
│            [ 3x3 ] [ 4x4 ] [ 5x5 ]   |   [ Editar rótulos eixos ]    │
│                                                                       │
│ ⓘ Quando houver oportunidade de melhorias cadastradas, as opções 3x3, │
│   4x4 e 5x5 não estarão disponíveis. Apenas a edição dos quadrantes   │
│   e eixos.                                            ✕               │
│                                                                       │
│  Probab.↑                                                             │
│         ┌──────────┬──────────┬──────────┐                            │
│    Alta │ Ponderar │ Ponderar │Implement.│                            │
│         ├──────────┼──────────┼──────────┤                            │
│   Mod   │ Arquivar │ Ponderar │ Ponderar │  ← clicar célula muda ação │
│         ├──────────┼──────────┼──────────┤                            │
│   Baixa │ Arquivar │ Arquivar │ Ponderar │                            │
│         └──────────┴──────────┴──────────┘                            │
│           Baixo    Moderado   Alto                                    │
│                       Impacto →                                       │
│                                                                       │
│ Validação da oportunidade de melhoria  ⓘ                              │
│ Prazo (dias)                                                          │
│ [ 30 ]                                                                │
│                                                                       │
│ Aprovação do plano de ação  ⓘ                                          │
│ ◯ Adicionar etapa de aprovação do plano de ação                        │
│                                                                       │
│ [ Gravar ]                                                            │
│ [ Cancelar ]                                                          │
└──────────────────────────────────────────────────────────────────────┘
```

## Configurações disponíveis

### 1. Tamanho da matriz

3 botões: `3x3`, `4x4`, `5x5`.

- Default: 3x3.
- Clicar troca o tamanho. **Atenção**: muda só enquanto não há oportunidades cadastradas. Após a primeira oportunidade, **trava o tamanho**.

### 2. Editar rótulos dos eixos e quadrantes

Clicar em **"Editar rótulos dos eixos e quadrantes"** abre painel:

```
Eixo Y (Probabilidade)
   3 níveis (3x3):
   - Nível 3 (topo): [ Alta            ]
   - Nível 2:         [ Moderada        ]
   - Nível 1 (base):  [ Baixa           ]

Eixo X (Impacto)
   - Nível 1 (esq):   [ Baixo           ]
   - Nível 2:         [ Moderado        ]
   - Nível 3 (dir):   [ Alto            ]

Nome do eixo Y: [ Probabilidade        ]
Nome do eixo X: [ Impacto              ]

[ Aplicar ]
```

Você pode renomear tudo. Por exemplo, em vez de "Probabilidade × Impacto", usar "Facilidade × Benefício" ou "Custo × Retorno".

### 3. Atribuir ação a cada quadrante

Clicar numa célula da matriz abre popover:

```
Selecione a ação:
⊙ Implementar (verde)
⊙ Ponderar (amarelo)
⊙ Arquivar (vermelho)
```

Você marca cada uma das 9 (ou 16, ou 25) células conforme sua estratégia.

#### Drag-to-paint

Pode arrastar com o mouse para pintar várias células com a mesma ação rapidamente.

#### Cores das ações

São fixas:
- 🟢 Implementar = verde
- 🟡 Ponderar = amarelo
- 🔴 Arquivar = vermelho

(Cores não são editáveis para manter consistência visual em todo o sistema.)

### 4. Prazo de validação

```
Prazo (dias)
[ 30 ]
```

- Default: 30 dias.
- O que controla: quantos dias após criar a oportunidade ela precisa ser **validada** (avaliada com P × I) por alguém com permissão `opp.validate`.
- Se passar do prazo sem validação, sistema notifica.

### 5. Aprovação do plano de ação (opcional)

```
◯ Adicionar etapa de aprovação do plano de ação
```

Toggle. Se marcar:
- Após a oportunidade ser validada e cair em "Implementar", o plano de ação precisa ser aprovado por alguém antes de ir para implementação.
- Adiciona mais um passo no fluxo.

Se desmarcar (default):
- Após validação "Implementar", vai direto para criar e executar plano de ação. Sem aprovação intermediária.

> Recomendado **manter desmarcado** se sua empresa quer agilidade. **Marcar** se a empresa exige governança formal (gerência precisa aprovar tudo).

## ⚠️ Restrição importante

Após cadastrar a **primeira oportunidade**, o tamanho da matriz **trava**. A mensagem aparece:

```
ⓘ Quando houver oportunidade de melhorias cadastradas, as opções 3x3,
   4x4 e 5x5 não estarão disponíveis. Apenas a edição dos quadrantes
   e eixos.
```

**O que continua editável** após primeira oportunidade:
- ✅ Rótulos de eixos.
- ✅ Rótulos de níveis (Alta / Moderada / Baixa).
- ✅ Ação atribuída a cada quadrante.
- ✅ Prazo de validação.
- ✅ Toggle de aprovação.

**O que trava**:
- ❌ Tamanho da matriz (3x3 → 4x4).

Por que? Porque oportunidades já têm `probability_score` e `impact_score` numéricos. Mudar de 3x3 para 5x5 quebraria os dados (uma nota "3" significaria coisas diferentes).

## Botão Gravar

Persiste a configuração. Toda oportunidade futura usa esta matriz.

## Estados especiais

### Configuração nunca feita
Acesso a Tarefas / Consulta / Nova oportunidade redireciona para cá com banner.

### Tentar mudar tamanho com oportunidades existentes
Bloqueado. Mensagem.

### Quadrante sem ação atribuída
Bloqueado. "Defina ação para todas as células antes de gravar."

## Permissões

| Ação | Permissão |
|---|---|
| Acessar configuração | `opp.config.update` |

## Exemplo Seven — configuração para gestão de resíduos

Cenário: Beatriz quer usar Oportunidades para capturar ideias de redução de resíduo / reciclagem.

**Tamanho**: 3x3 (simples).

**Rótulos personalizados**:
- Eixo Y: "Viabilidade técnica" (em vez de Probabilidade)
  - Alta: Tecnicamente fácil
  - Moderada: Possível com algum investimento
  - Baixa: Tecnicamente complexo / arriscado
- Eixo X: "Benefício ambiental" (em vez de Impacto)
  - Alto: Reduz > 20% de resíduo
  - Moderado: Reduz 5-20%
  - Baixo: Reduz < 5%

**Quadrantes**:
| Viabilidade ↓ \ Benefício → | Baixo | Moderado | Alto |
|---|---|---|---|
| Alta | Ponderar | Implementar | **Implementar** |
| Moderada | Arquivar | Ponderar | Implementar |
| Baixa | Arquivar | Arquivar | Ponderar |

**Prazo de validação**: 60 dias (Seven é meticulosa, prefere análise mais demorada).

**Aprovação do plano**: ☐ desativada (Beatriz tem autonomia para decidir).

**Gravar**. Sistema pronto para receber oportunidades.
