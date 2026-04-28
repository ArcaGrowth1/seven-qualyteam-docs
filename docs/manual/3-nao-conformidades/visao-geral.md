# Não Conformidades — visão geral

Onde você captura e trata **eventos não conformes** no SGQ/SGA: vazamentos, derramamentos, falhas operacionais, reclamações, desvios de processo, incidentes ambientais.

## URL do módulo

`nc.qualyteam.com` (ou `nonconformities.qualyteam.com`)

## Top-bar interno

```
[Logo] [Não conformidades ▾] [Dashboard] [Tarefas] [Registrar ocorrência] [Registrar não conformidade] [Consulta ▾]
```

| Aba | O que tem |
|---|---|
| **Dashboard** | Visão executiva: NCs/Ocorrências por status, unidade, processo, origem, eficácia |
| **Tarefas** | Suas pendências em **6 etapas do fluxo**: Ocorrências → Ações imediatas → Causa e planejamento → Aprovação → Implementação → Eficácia |
| **Registrar ocorrência** | Cadastrar uma **ocorrência leve** (registro inicial rápido) |
| **Registrar não conformidade** | Cadastrar diretamente uma **RNC** (tratamento formal completo) |
| **Consulta ▾** | Lista mestra com 4 visões: Não conformidades, Ações imediatas, Ações corretivas, Ocorrências |

## ⭐ Conceito-chave: Ocorrência ≠ RNC

**Ocorrência**: registro **leve** de um evento. Pode ou não escalar para RNC.
**RNC** (Relatório de Não Conformidade): **tratamento formal completo** com 6 etapas.

```mermaid
flowchart LR
    O[📋 Ocorrência<br/>cadastro leve em 2-5 min] -->|encerrar simples| C1[✅ Encerrada]
    O -->|escalar / virar RNC| RNC[📑 RNC<br/>tratamento formal<br/>6 etapas]
    RNC --> C2[✅ Encerrada<br/>com eficácia verificada]
```

### Quando registrar Ocorrência (sem virar RNC)

- Evento pequeno, contornável no momento.
- Reclamação simples já resolvida.
- Quase-acidente sem impacto.
- "Para registro" de eventos do dia-a-dia.

### Quando virar RNC

- Requisito de norma exige tratamento formal (ISO 9001 / 14001).
- Houve impacto real (ambiental, financeiro, segurança).
- Repetição de evento (recorrente = ação corretiva sistêmica).
- Cliente exige análise formal.
- Auditoria identificou.

## Os 6 estados do fluxo de RNC

```mermaid
flowchart TD
    A[1. Ocorrência] -->|escalar| B[2. Ações Imediatas<br/>contenção rápida]
    B --> C[3. Causa e Planejamento<br/>5 Porquês ou Ishikawa<br/>+ Plano de ações corretivas]
    C --> D[4. Aprovação<br/>do plano]
    D -->|reprovado| C
    D -->|aprovado| E[5. Implementação<br/>executar ações]
    E --> F[6. Eficácia<br/>verificar após N dias]
    F -->|eficaz| G[✅ Encerrada]
    F -->|ineficaz| C
```

Cada etapa tem **uma aba dedicada de Tarefas**, mostrando o que está naquele estágio sob sua responsabilidade.

## Mapa das telas

```mermaid
flowchart LR
    NC[Não Conformidades] --> D[Dashboard]
    NC --> T[Tarefas]
    T --> Toc[Ocorrências]
    T --> Tai[Ações imediatas]
    T --> Tcp[Causa e planejamento]
    T --> Tap[Aprovação]
    T --> Tim[Implementação]
    T --> Tef[Eficácia]
    NC --> Roc[Registrar ocorrência]
    NC --> Rnc[Registrar RNC]
    NC --> C[Consulta]
    C --> Cnc[Não conformidades]
    C --> Cai[Ações imediatas]
    C --> Cac[Ações corretivas]
    C --> Coc[Ocorrências]
    NC --> Cfg[Configuração do módulo]
```

## Permissões deste módulo

| Permissão | Para quê |
|---|---|
| `nc.occurrence.create` | Registrar ocorrência |
| `nc.occurrence.update_delete` | Editar/excluir ocorrência |
| `nc.occurrence.close` | Encerrar ocorrência |
| `nc.rnc.create` | Registrar RNC (direto ou escalar de ocorrência) |
| `nc.rnc.update_open` | Editar RNC enquanto está em fluxo |
| `nc.rnc.delete` | Excluir RNC (raríssimo) |
| `nc.rnc.read` | Ler RNCs (Consulta) |
| `nc.rnc.export` | Exportar lista |
| `nc.tasks.read_all` | Ver tarefas de outros |
| `nc.dashboard.read` | Ver Dashboard |
| `nc.config.update` | Editar configuração do módulo (origens, métodos) |
| `nc.origin.create` | Adicionar origem nova |
