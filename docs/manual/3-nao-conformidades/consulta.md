# Não Conformidades — Consulta

## Onde fica

`Não conformidades → Consulta ▾`. Dropdown com **4 visões**:
- **Não conformidades** — lista de RNCs.
- **Ações imediatas** — lista de AIs.
- **Ações corretivas** — lista de ACs.
- **Ocorrências** — lista de ocorrências.

URLs:
- `/search/non-conformities`
- `/search/immediate-actions`
- `/search/corrective-actions`
- `/search/occurrences`

## Quem acessa

`nc.rnc.read` (e implicitamente para Ocorrências). Geralmente: Coord. Qualidade, Auditor, Gerência.

## Por que 4 visões

Cada visão atende uma pergunta diferente:

| Visão | Pergunta que ela responde |
|---|---|
| **Não conformidades** | "Quantas RNCs temos? Em que estado? Onde acontecem?" |
| **Ações imediatas** | "Quem está fazendo o quê AGORA, em contenção rápida?" |
| **Ações corretivas** | "Quais são as ações de longo prazo em andamento?" |
| **Ocorrências** | "Histórico completo de eventos, mesmo os pequenos" |

## Visão 1: Não conformidades (RNCs)

```
┌──────────────────────────────────────────────────────────────────────┐
│ Não conformidades                                                     │
│                                       [+ Nova RNC] [Filtros] [Exportar]│
│ Total: 18                                                             │
├──────────────────────────────────────────────────────────────────────┤
│ Código │ Descrição    │ Etapa atual │ Severidade │ Unidade  │ Origem │
│ RNC-018│ Vazamento... │ Implementação│ Alta      │ Caieiras │ Insp.  │
│ RNC-017│ Atraso ent...│ Eficácia     │ Moderada  │ Matriz   │ Reclam.│
│ RNC-016│ Não conform..│ Encerrada ✓  │ Baixa     │ Berlim   │ Audit. │
└──────────────────────────────────────────────────────────────────────┘
```

### Colunas

| Coluna | O que mostra |
|---|---|
| Código | RNC-XXX |
| Descrição | Texto curto da NC |
| Etapa atual | Causa e Planejamento / Aprovação / Implementação / Eficácia / Encerrada |
| Severidade | Baixa / Moderada / Alta / Crítica |
| Unidade | Onde ocorreu |
| Origem | Auditoria / Reclamação / etc. |
| Tipo | Real / Potencial |
| Data identificação | Quando foi aberta |
| Data encerramento | Vazio se aberta |
| Eficaz | ✅ / ❌ / — (se ainda não verificada) |

### Filtros

| Filtro | O que filtra |
|---|---|
| Etapa | Filtra por estágio do fluxo |
| Severidade | Baixa / Moderada / Alta / Crítica |
| Tipo | Real / Potencial |
| Unidade | Multi |
| Processo | Multi |
| Origem | Multi |
| Período de identificação | Faixa |
| Período de encerramento | Faixa |
| Eficaz | Sim / Não / Ainda não verificada |
| Responsável (atual) | Pessoa |
| Aprovador | Pessoa |

## Visão 2: Ações imediatas

Lista todas as AIs (de todas as ocorrências e RNCs):

```
│ Vinculado a │ Descrição          │ Responsável │ Prazo     │ Status     │
│ OC-042      │ Conter vazamento.. │ Carlos F.   │ 28/04/26  │ Concluída  │
│ RNC-018     │ Inspecionar lote.. │ Pedro Alm.  │ 02/05/26  │ Pendente   │
│ OC-040      │ Notificar cliente..│ Beatriz B.  │ 25/04/26  │ Concluída  │
```

### Filtros específicos

- **Status**: Pendente / Em execução / Concluída / Atrasada
- **Tipo de pai**: Ocorrência / RNC
- **Responsável**

## Visão 3: Ações corretivas

Lista todas as ACs:

```
│ Vinculado a │ Descrição          │ Responsável │ Prazo     │ % concluído │
│ RNC-018     │ Atualizar procedi..│ Pedro Alm.  │ 15/05/26  │ 60%         │
│ RNC-018     │ Treinar equipe...  │ RH          │ 30/05/26  │ 0%          │
│ RNC-016     │ Substituir software│ TI          │ 30/04/26  │ 100%        │
```

### Filtros específicos

- **Status**: Planejada / Em execução / Concluída / Cancelada
- **Vencimento**: Atrasada / Vencendo em 7d / Vencendo em 30d / OK
- **RNC vinculada**: filtrar por RNC específica

## Visão 4: Ocorrências

Lista todas as ocorrências:

```
│ Código │ Descrição     │ Status     │ Data ocorr.│ Unidade   │ Origem  │
│ OC-047 │ Falha bombo...│ Aberta     │ 30/04/2026 │ Caieiras  │ Insp.   │
│ OC-046 │ Atraso col... │ Encerrada  │ 28/04/2026 │ Norte     │ Reclam. │
│ OC-045 │ Vazamento ...│ Escalada   │ 27/04/2026 │ Caieiras  │ Insp.   │
```

### Filtros específicos

- **Status**: Aberta / Encerrada / Escalada (virou RNC)
- **Vinculada a RNC**: Sim / Não
- **Severidade** (se cadastrada): igual RNC

## Ações por linha (todas as visões)

### Clicar
Abre detalhe completo do registro.

### Menu três-pontos
- Editar (se permitido pelo estado)
- Reatribuir
- Adicionar comentário
- Imprimir
- Copiar link

## Ações em massa

- Reatribuir múltiplas tarefas para outro responsável.
- Exportar selecionadas.
- Encerrar múltiplas ocorrências (raro, mas existe).

## Botões do topo

| Botão | O que faz |
|---|---|
| **+ Nova RNC** | Atalho para `/nonconformance-reports/new` |
| **Filtros** | Painel lateral |
| **Exportar** | CSV/Excel da lista filtrada |

## Detalhe de uma RNC (clicando na linha)

Abre página completa da RNC com layout que evolui conforme a etapa:

```
┌──────────────────────────────────────────────────────────────────────┐
│ RNC-018  · Vazamento bombonas lote 2025-12  · Etapa: Implementação    │
│                                                                       │
│ ┌─Detalhes─┬─Causa─┬─Plano─┬─Eficácia─┬─Histórico─┐                  │
│                                                                       │
│ Detalhes (passo 1 do cadastro)                                        │
│ Descrição: ...                                                        │
│ Severidade: Alta                                                      │
│ ...                                                                   │
│                                                                       │
│ Anexos:                                                               │
│ 📄 relatorio.pdf  📷 foto-vazamento.jpg                              │
│                                                                       │
│ [ Editar ] (se aberta)                                                │
└──────────────────────────────────────────────────────────────────────┘
```

Cada aba dentro mostra os dados de uma etapa do fluxo.

### Aba Histórico
Mostra timeline completa de tudo que aconteceu na RNC:
- Criada em DD/MM por X
- Plano enviado em DD/MM por Y
- Plano aprovado em DD/MM por Z
- AC-1 concluída em DD/MM por W
- Verificação Eficaz em DD/MM por V

Útil para auditoria.

## Exportar

CSV/Excel inclui:
- Todas as colunas visíveis.
- Plus: campos personalizados configurados.
- Para RNCs: status atual + histórico completo de etapas.

Limite: 5.000 linhas. Acima: refinar filtros.

## Permissões

| Ação | Permissão |
|---|---|
| Ver | `nc.rnc.read` |
| Exportar | `nc.rnc.export` |
| Editar enquanto aberta | `nc.rnc.update_open` |
| Excluir | `nc.rnc.delete` (raríssimo) |

## Estados especiais

### Lista vazia
"Nenhuma não conformidade registrada ainda".

### Filtros não retornam
"Sem resultados. [ Limpar filtros ]".

## Exemplo Seven — uso típico

**Reunião de análise crítica trimestral**:
1. Beatriz vai em Consulta → Não conformidades.
2. Filtra: período = trimestre passado, severidade = Alta + Crítica.
3. Lista 8 RNCs. Exporta para Excel.
4. Apresenta na reunião: 6 já encerradas com eficácia, 2 ainda em implementação.
5. Discute padrões: 4 das 8 vieram do CT Caieiras → ação preventiva sistêmica para Caieiras.
