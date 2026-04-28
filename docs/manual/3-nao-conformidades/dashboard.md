# Não Conformidades — Dashboard

## Onde fica

`Não conformidades → Dashboard` (URL: `/dashboard`)

## Quem acessa

`nc.dashboard.read`. Geralmente: Coord. Qualidade, Auditor, Gerência.

## Anatomia

Tela com **3 filtros globais** + **vários widgets** divididos em duas seções: **RNCs** e **Ocorrências**.

```
┌──────────────────────────────────────────────────────────────────────┐
│ Dashboard          Unidades       Processos        Origem            │
│                    [ Selecione ▾] [ Selecione ▾]   [ Selecione ▾]  🔍│
├──────────────────────────────────────────────────────────────────────┤
│  Status das tarefas das não conformidades                            │
│  Total: 0                                                             │
│       [ 📊 ]  Ainda não contém dados                                 │
└──────────────────────────────────────────────────────────────────────┘
```

## Filtros globais (afetam todos os widgets)

| Filtro | O que filtra |
|---|---|
| **Unidades organizacionais** | Multi-select. Mostra dados das unidades selecionadas |
| **Processos** | Multi-select. Mostra dados dos processos |
| **Origem** | Multi-select. Mostra dados da origem (Auditoria / Reclamação / Inspeção…) |

Aplica-se ao clicar na **lupa 🔍** ao lado.

## Widgets — Seção RNCs

### 1. Status das tarefas das NCs (por etapa)
**O que mostra**: quantas RNCs estão em cada etapa do fluxo:
- Causa e planejamento
- Aprovação
- Implementação
- Eficácia
- Encerradas

**Útil para**: identificar gargalos. Se "Aprovação" tem 20 itens há semanas, gerência está atrasada.

### 2. NCs por unidade organizacional
Quantidade de RNCs (ativas + encerradas) por unidade. Pizza ou barras.

### 3. NCs por processo
Idem, agrupado por processo (Produção, Operações, Ambiental…).

### 4. NCs por origem
Idem, por origem (Auditoria, Reclamação, Inspeção, Indicador…).
**Útil para**: ver de onde mais vêm não-conformidades. Se 70% vêm de "Inspeção interna", você está identificando bem internamente. Se 70% vêm de "Reclamação cliente", você está tarde demais.

### 5. NCs por período
Linha temporal (mês a mês). Tendência.

### 6. ⭐ Eficácia das ações por período
**O KPI mais importante do módulo**.

Mostra: % de RNCs verificadas como **eficazes** vs **ineficazes**, mês a mês.

```
Eficácia (%)
100 ─────────────────────
 90 ─       ─ ─ ─ ─ ─ ─ ─
 80 ─ ──────              ← meta
 70 ─
 60 ─
        ▓▓▓▓▓ ▓▓▓▓ ▓▓▓▓▓
        Jan   Fev  Mar
```

**Útil para**: provar para auditoria que as ações corretivas funcionam. Se está caindo, processo de análise de causa precisa melhorar.

## Widgets — Seção Ocorrências

### 7. Tarefas de ocorrências por tipo
Ocorrências em aberto, divididas pelo seu estado (Aberta, Com ações imediatas, Escalada).

### 8. Ocorrências por unidade
### 9. Ocorrências por processo
### 10. Ocorrências por origem
### 11. Ocorrências por período
### 12. Ocorrências encerradas por período

Análogos aos de RNC, mas para ocorrências.

## Diferenças importantes entre os widgets de NC e Ocorrência

| | RNC | Ocorrência |
|---|---|---|
| Volume | Menor (só casos sérios) | Maior (todo evento registrado) |
| Tempo de tratamento | Semanas a meses | Horas a dias |
| Eficácia mensurada? | ✅ | ❌ |
| Aparecem em auditoria? | ✅ Sim, sempre | Geralmente sim, como amostragem |

## Interagir com widgets

- **Hover**: mostra valor exato.
- **Clicar em uma fatia**: leva para Consulta filtrada por aquele valor.
- **Menu três-pontos** no widget: exportar como imagem, ver dados em tabela.

## De onde vêm os dados (técnico)

Cada widget faz uma chamada de API:
- `/api/dashboard/non-conformance-reports-tasks-by-step`
- `/api/dashboard/non-conformance-reports-by-organizational-unit`
- `/api/dashboard/non-conformance-reports-by-process`
- `/api/dashboard/non-conformance-reports-by-origin`
- `/api/dashboard/non-conformance-reports-by-period`
- `/api/dashboard/non-conformance-reports-effectiveness-by-period`
- `/api/dashboard/occurrences-tasks-by-type`
- `/api/dashboard/occurrences-by-organizational-unit`
- `/api/dashboard/occurrences-by-process`
- `/api/dashboard/occurrences-by-origin`
- `/api/dashboard/occurrences-by-period`
- `/api/dashboard/occurrences-closed-by-period`

(Detalhes em [03-modules/nonconformities/overview.md](../../03-modules/nonconformities/overview.md).)

## Estados especiais

### Sem dados ainda
"Ainda não contém dados" em cada widget. Some quando registrar a primeira ocorrência/RNC.

### Filtros sem retorno
"Sem resultados para os filtros" + sugestão de limpar.

## Boas práticas

- Olhar **semanalmente** se você é Coord. Qualidade.
- Apresentar o widget de **Eficácia** em reuniões mensais com gerência.
- Filtrar por **sua unidade** se você é gerente de filial.
- **Comparar processos**: se Operações tem 10x mais NC que Comercial, normal (Operações tem mais risco). Mas se Comercial tem mais que Operações, algo estranho.

## Exemplo Seven

Cenário: depois de 6 meses operando o sistema, Beatriz vai para reunião gerencial com:
- 47 RNCs no semestre, 62% encerradas com eficácia.
- 80% das origens vêm de "Inspeção interna" (bom — Seven está identificando).
- 15% vêm de "Reclamação de cliente" (precisa reduzir).
- Pico de NCs em julho/agosto (verão, mais derramamentos).
- Unidade Caieiras tem 2x mais que outras (porque opera o aterro).

Ela exporta os widgets como imagens, monta apresentação, define metas para o próximo semestre.
