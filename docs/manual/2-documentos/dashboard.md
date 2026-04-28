# Documentos — Dashboard

## Onde fica

`Documentos → Dashboard` (URL: `/dashboard`)

## Quem acessa

`docs.dashboard.read`. Geralmente: Coord. Qualidade, Auditor, Gerência.

## O que aparece

Tela com **9 widgets** + **2 filtros globais** no topo.

```
┌──────────────────────────────────────────────────────────────────────┐
│ Dashboard                Unidades organizacionais   Processos        │
│                          [ Selecione        ▾ ]    [ Selecione   ▾]  │
├──────────────────────────────────────────────────────────────────────┤
│  Status das tarefas de elaboração                                    │
│  ────────────────────────────────────────────                        │
│  [gráfico de barras por etapa]                                       │
├──────────────────────────────────────────────────────────────────────┤
│  Tarefas de solicitação por tipo                                     │
│  ...                                                                  │
└──────────────────────────────────────────────────────────────────────┘
```

## Filtros globais

Os 2 filtros do topo afetam **todos os widgets ao mesmo tempo**:

### Unidades organizacionais (multi-select)
Filtra para mostrar apenas dados de documentos vinculados às unidades selecionadas.
- Vazio = todas.
- Selecionou Matriz + Berlim = só dados desses dois.

### Processos (multi-select)
Mesma lógica, mas por processo (Vendas, Produção, GQ, etc.).

> Filtros se aplicam clicando na **lupa** ao lado deles.

## Os 9 widgets

### 1. Status das tarefas de elaboração

**Gráfico**: barras horizontais (ou pizza).
**O que mostra**: quantidade de documentos em cada etapa do fluxo de elaboração:
- Aguardando elaborador
- Em elaboração
- Aguardando aprovação
- Aprovados (no período)

**De onde vêm os dados**: documentos cujo status está em fluxo (não publicados ainda).

**Útil para**: ver gargalos. Se "Aguardando aprovação" tem 30 itens, provavelmente os aprovadores estão sobrecarregados.

### 2. Tarefas de solicitação por tipo

**Gráfico**: barras.
**O que mostra**: solicitações de novos documentos abertas pelos usuários, agrupadas por tipo (POP, Política, Manual…).

**De onde vêm**: usuários podem solicitar a criação de um documento novo (workflow alternativo, nem todos os clientes usam).

### 3. Tarefas de validade por status

**Gráfico**: pizza ou barras.
**O que mostra**: documentos divididos por:
- Vencendo em até 30 dias
- Vencendo em até 90 dias
- Já vencidos
- Sem validade

**Útil para**: priorizar revisões.

### 4. Documentos por tipo

Categoria do documento (POP, Manual, Política, Norma, Lei…) — quantos existem de cada.

### 5. Documentos por status

Quantos estão em Rascunho / Em elaboração / Em aprovação / Publicados / Vencidos / Inativos.

### 6. Documentos por unidade organizacional

Quantos por unidade (Matriz, Berlim, Amsterdã, Chennai…). Útil para ver desbalanceamento (uma unidade com muito mais docs que outra).

### 7. Documentos por processo

Quantos por área funcional (Vendas, Produção, etc.).

### 8. Documentos por categoria

Similar ao "por tipo" mas usa as **categorias** que você cadastrou (mais granular).

### 9. Documentos por período

**Gráfico**: linha temporal.
**O que mostra**: documentos cadastrados ao longo do tempo (mês a mês).
**Útil para**: ver evolução do SGQ — empresa mais "madura" tem documentos cadastrados de forma constante, não em picos.

## Interagir com os widgets

- **Hover** em uma barra/fatia: mostra valor exato.
- **Clicar** em uma barra: leva para a Consulta filtrada por aquele valor (ex: clicar em "Vencidos" leva para `/search/internal-documents?status=expired`).
- **Menu três-pontos** no widget: opções de exportar widget em imagem, ver dados em tabela.

## De onde os dados vêm

Os 9 widgets fazem chamadas para a API:
- `/api/dashboards/elaboration-tasks-by-stage`
- `/api/dashboards/request-tasks-by-type`
- `/api/dashboards/expiration-tasks-by-status`
- `/api/dashboards/documents-by-type`
- `/api/dashboards/documents-by-status`
- `/api/dashboards/documents-by-organizational-unit`
- `/api/dashboards/documents-by-process`
- `/api/dashboards/documents-by-category`
- `/api/dashboards/documents-by-period`

(Detalhes técnicos em [`docs/03-modules/documents/overview.md`](../../03-modules/documents/overview.md).)

## Atualização

- **Em tempo real?**: não. Os dados são consultados quando você abre a página e quando muda os filtros.
- Para forçar atualização: F5 ou voltar para a aba Dashboard.

## Estados especiais

### "Ainda não contém dados"
Se o módulo está vazio (nenhum documento cadastrado), aparece em cada widget:
```
[ 📊 ilustração de gráfico vazio ]
   Ainda não contém dados
```
Some assim que você cadastrar o primeiro documento.

### Filtros não retornam nada
Mesmo estado vazio, mas com mensagem "Sem resultados para os filtros aplicados" + sugestão de limpar filtros.

## Boas práticas de uso

- Olhar **toda manhã** se você é Coord. Qualidade — identifica gargalos.
- Filtrar por **sua unidade** se você é gerente de filial — visão local.
- **Apresentação para auditoria**: filtra Status = Publicado + Período = ano corrente. Mostra para auditor o tamanho do SGQ.

## Exemplo Seven

Se a Seven tiver:
- 80 documentos publicados
- 5 em elaboração
- 3 em aprovação
- 12 com validade próxima (30 dias)

→ O widget #1 mostra a fila de aprovação ainda saudável.
→ O widget #3 alerta para revisar os 12 que estão vencendo.
→ Você clica em "12 vencendo" → vai para Consulta filtrada → resolve um a um.
