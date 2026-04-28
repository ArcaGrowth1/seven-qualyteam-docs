# Riscos — Dashboard, Tarefas e Consulta

## Dashboard

### Onde fica
`Riscos → Dashboard` (URL: `/dashboard`)

### Quem acessa
`risk.dashboard.read`. Geralmente: Coord. Qualidade, Responsáveis de unidade, Gerência.

### Anatomia

Filtros globais no topo:
- **Unidade organizacional** (multi)
- **Categoria** (Ambiental, Operacional, Saúde…)
- **Status** (Em tratamento, Monitorado, Para reavaliação, Inativo)

### Widgets

#### 1. Visão geral do tratamento
Resumo: quantos riscos em cada estado.

```
Em tratamento: 12
Monitorados: 18
Para reavaliação: 5
Inativos: 3
─────
Total ativo: 35
```

#### 2. ⭐ Heatmap (matriz de riscos)
Visual mais importante.

```
Probabilidade ↑
       ┌──────────┬──────────┬──────────┐
  Alta │   2 🟡   │   1 🔴   │   3 🔴   │
       ├──────────┼──────────┼──────────┤
   Mod │   4 🟢   │   8 🟡   │   2 🔴   │
       ├──────────┼──────────┼──────────┤
 Baixa │  10 🟢   │   5 🟢   │   0 🟡   │
       └──────────┴──────────┴──────────┘
        Baixo    Moderado    Alto
                  Impacto →

Total: 35 riscos
```

Cada célula mostra **quantos riscos caem nela** + cor do tratamento.

> Cuidado se aparecer muitos vermelhos no canto Alto/Alto: sua empresa está em estado crítico.

#### 3. Riscos por unidade
Quantos riscos cada unidade tem (cada uma com sua matriz).

#### 4. Riscos por categoria
Distribuição: Ambiental / Operacional / Saúde / Legal / etc.

#### 5. Riscos por tratamento
Quantos Reter / Mitigar / Evitar.

#### 6. Riscos pendentes de reavaliação
Quantos chegaram na data e ainda não foram reavaliados (atraso).

#### 7. Evolução do risco médio (no tempo)
Linha mostrando o "risco médio" (média de P × I) ao longo dos meses. Útil para ver se a empresa está reduzindo o risco geral.

#### 8. Top 10 riscos mais críticos
Lista dos riscos com maior P × I score.

### Interagir
- Hover mostra valor exato.
- Clicar em uma célula do heatmap leva à Consulta filtrada por aquela combinação P × I.

---

## Tarefas

### Onde fica
`Riscos → Tarefas` (URL: `/tasks/<aba>`)

### Quem acessa
Default: você. Outros: depende.

### As 2 abas

#### Aba 1: Riscos em tratamento
URL: `/tasks/risks-in-treatment`

**O que aparece**: ações de tratamento (do plano de mitigação) atribuídas a você.

```
│ Vinculado a │ Ação                  │ Prazo     │ Status     │
│ RSK-007     │ Instalar bandeja...   │ 30/06/26  │ Pendente   │
│ RSK-005     │ Treinar operadores... │ 15/05/26  │ Concluída  │
│ RSK-009     │ Substituir EPIs       │ 10/06/26  │ Em curso   │
```

**Como cai aqui**: alguém criou um risco e te atribuiu numa AC do plano.

**Ações**:
- Clicar abre detalhe da ação.
- Marcar como "Concluída" + evidência.
- Notifica responsável da unidade.

#### Aba 2: Riscos para reavaliação
URL: `/tasks/risks-for-reassessment`

**O que aparece**: riscos da sua unidade que **chegaram na data de reavaliação**.

```
│ Código │ Descrição          │ Quadrante atual │ Última avaliação │ Vence em │
│ RSK-001│ Contaminação solo  │ Mitigar         │ 28/10/2025       │ 0 dias   │
│ RSK-003│ Atraso entrega...  │ Reter           │ 15/01/2026       │ atrasado │
│ RSK-007│ Vazamento Cl.I     │ Mitigar         │ 28/01/2026       │ 5 dias   │
```

**Como cai aqui**: cron diário verifica `next_reassessment_at <= hoje + 30d`.

**Ações**:
- Clicar abre detalhe → fazer **nova avaliação P × I**.
- Adicionar comentários do período observado.
- Sistema atualiza tratamento se quadrante mudou.
- Próxima reavaliação = hoje + período da unidade.

---

## Consulta

### Onde fica
`Riscos → Consulta` (URL: `/search`)

### Quem acessa
(default).

### Lista mestra completa

```
┌──────────────────────────────────────────────────────────────────────┐
│ Riscos                                              [+ Novo] [Filtros]│
│ Total: 35 ativos / 3 inativos                                         │
├──────────────────────────────────────────────────────────────────────┤
│ Cód.  │ Descrição        │ Unidade   │ Categ │ P×I │ Tratam.│ Status │
│RSK-007│ Vazamento Cl.I   │ Caieiras  │ Amb.  │ 3×3 │Mitigar │ Em tr. │
│RSK-005│ Falha equipto    │ Caieiras  │ Op.   │ 2×3 │Mitigar │ Monit. │
│RSK-003│ Atraso coleta    │ Norte     │ Op.   │ 1×2 │ Reter  │ Reaval.│
│RSK-001│ Multa CETESB     │ Caieiras  │ Legal │ 2×4 │ Evitar │ Em tr. │
└──────────────────────────────────────────────────────────────────────┘
```

### Colunas

| Coluna | O que mostra |
|---|---|
| Código | RSK-XXX |
| Descrição | Texto curto |
| Unidade | Onde está |
| Categoria | Tipo |
| P×I | Avaliação atual (com cor do tratamento) |
| Tratamento | Reter / Mitigar / Evitar |
| Status | Em tratamento / Monitorado / Para reavaliação / Inativo |
| Próx. reavaliação | Data prevista |
| Responsável | Da unidade |
| Última atualização | |

### Filtros

| Filtro | O que filtra |
|---|---|
| Unidade | (multi) |
| Categoria | |
| Tratamento | Reter / Mitigar / Evitar |
| Status | Em tratamento / Monitorado / Reavaliação / Inativo |
| P × I exato | Filtrar por quadrante |
| Período (criação ou reavaliação) | Faixa de datas |
| Responsável | Pessoa |

### Ações por linha

- Clicar abre detalhe completo.
- Menu três-pontos: Editar, Reavaliar, Inativar, Reativar, Imprimir, Copiar link.

### Detalhe do risco

```
┌──────────────────────────────────────────────────────────────────────┐
│ RSK-007 · Vazamento Classe I · Em tratamento (60%)                    │
│                                                                       │
│ ┌─Resumo─┬─Avaliação─┬─Tratamento─┬─Reavaliações─┬─Histórico─┐       │
│                                                                       │
│ Resumo                                                                │
│  Unidade: CT Caieiras                                                 │
│  Categoria: Ambiental                                                 │
│  Descrição: ...                                                       │
│  Causa potencial: ...                                                 │
│  Consequência: ...                                                    │
│                                                                       │
│ Avaliação atual                                                       │
│  Probabilidade: Provável (3/4)                                        │
│  Impacto: Moderado (3/4)                                              │
│  Quadrante: Mitigar 🟡                                                 │
│                                                                       │
│ Tratamento                                                            │
│  AC-1: Bandeja contenção — Pedro, prazo 30/06 (60%)                   │
│  AC-2: Treinamento — RH, prazo 15/05 ✓                                │
│  AC-3: Checklist — Carlos, prazo 07/05 ✓                              │
│  Indicador: quase-acidentes/mês ≤ 1                                   │
│                                                                       │
│ Reavaliações                                                          │
│  Atual: cadastrado em 28/04/2026                                      │
│  Próxima: 28/10/2026                                                  │
│  Histórico: (vazio, ainda não foi reavaliado)                         │
│                                                                       │
│ Histórico (timeline completa)                                         │
│  - 28/04/2026: Cadastrado por Beatriz                                  │
│  - 30/04/2026: AC-3 concluída por Carlos                              │
│  - 15/05/2026: AC-2 concluída por Maria                               │
│  - ...                                                                │
└──────────────────────────────────────────────────────────────────────┘
```

### Aba Reavaliações
Histórico de **todas as reavaliações** que esse risco já teve. Cada uma:
- Quando foi feita.
- Quem fez.
- P × I antes e depois.
- Tratamento antes e depois.
- Comentários.

Útil para ver evolução do risco no tempo.

### Permissões

| Ação | Permissão |
|---|---|
| Ver | (default) |
| Editar enquanto em tratamento | responsável da unidade |
| Reavaliar | responsável da unidade ou `risk.unit_config.update` |
| Editar risco encerrado | `risk.update_completed` |
| Inativar risco | `risk.toggle` |
| Excluir risco | `risk.delete` (raríssimo) |

## Estados especiais

### Sem unidade configurada
Tarefas e Consulta vazias. CTA "Configurar primeira unidade".

### Sem riscos cadastrados
"Nenhum risco identificado. [+ Identificar risco]"

### Reavaliação atrasada
Linha aparece com **borda vermelha** e flag "Atrasado".

## Exemplo Seven — fluxo de uso

**Coord. Ambiental — rotina semanal**:
1. Vai em Dashboard. Vê heatmap. Confere se algum risco subiu de quadrante.
2. Vai em **Tarefas → Em tratamento**. Cobra ações atrasadas.
3. Ao final do mês: Tarefas → Para reavaliação. Reavalia 5 riscos do mês.

**Auditoria interna anual**:
1. Vai em Consulta → exporta lista de riscos ativos.
2. Filtra por categoria Ambiental: 12 riscos.
3. Verifica se todos têm plano de tratamento.
4. Verifica histórico de reavaliações.
5. Apresenta ao auditor com evidências.
