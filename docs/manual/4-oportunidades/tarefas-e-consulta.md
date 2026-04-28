# Oportunidades — Tarefas e Consulta

## Tarefas

### Onde fica

`Oportunidades → Tarefas` (URL: `/tasks/validation`)

### Quem acessa

Default: você. Para ver de outros: `opp.tasks.read_all`.

### O que aparece

Suas pendências em cada **etapa do fluxo de oportunidade**:

| Aba | O que tem |
|---|---|
| **Validação** | Oportunidades aguardando você atribuir P × I (apenas para quem tem `opp.validate`) |
| **Plano de ação** | Oportunidades onde você é responsável de criar o plano |
| **Aprovação** | Aguardando sua aprovação (se habilitado) |
| **Implementação** | Ações que você precisa executar |
| **Validação final** | Verificar se funcionou após N dias |

> Quantidade exata de abas depende da configuração do módulo (se tem aprovação habilitada ou não).

### Como uma oportunidade cai aqui

| Cai em | Quando |
|---|---|
| Validação | Alguém criou uma oportunidade e você é validador (`opp.validate`) |
| Plano de ação | A oportunidade foi avaliada como "Implementar" e você é responsável designado |
| Aprovação | Plano foi enviado para sua aprovação |
| Implementação | AC dentro de um plano foi atribuída a você |
| Validação final | Implementação concluída + N dias e você é validador |

### Cada coluna típica

| Coluna | O que mostra |
|---|---|
| Código | OPP-XXX |
| Descrição | Texto curto |
| Avaliação P × I | Ex: "Alta × Alto" + ícone do quadrante |
| Ação recomendada | Implementar / Ponderar / Arquivar |
| Solicitante | Quem propôs |
| Status | Etapa atual do fluxo |
| Prazo | Se aplicável |

### Ações por linha

- Clicar abre detalhe.
- No detalhe: avaliar (validação), criar plano, aprovar, marcar concluída, etc.

### Notificações

Mesmas regras dos outros módulos:
- Atribuição → e-mail + in-app.
- Prazo se aproximando → lembrete.
- Atrasou → escalação para chefe.

---

## Consulta

### Onde fica

`Oportunidades → Consulta` (URL: `/search`)

### Quem acessa

`opp.read`. Geralmente todos.

### O que aparece

Lista mestra de **todas as oportunidades** (de todos os status):

```
┌──────────────────────────────────────────────────────────────────────┐
│ Oportunidades                                  [+ Nova] [Filtros] [↓]│
│ Total: 42                                                             │
├──────────────────────────────────────────────────────────────────────┤
│ Cód. │ Descrição     │ P × I    │ Ação        │ Status     │ Solic.  │
│OPP-12│ Subst. bomb...│ Alta×Alto│ Implementar │ Implem.    │ Carlos  │
│OPP-11│ Trocar fluxo..│ Mod×Alto │ Implementar │ Aguard.val.│ João    │
│OPP-10│ Otimizar rota.│ Baixa×B. │ Arquivar    │ Arquivada  │ Maria   │
│OPP-09│ Reciclar plás.│ Alta×Alto│ Implementar │ Encerrada✓ │ Pedro   │
└──────────────────────────────────────────────────────────────────────┘
```

### Colunas

| Coluna | O que mostra |
|---|---|
| Código | OPP-XXX |
| Descrição | Texto |
| P × I | Avaliação |
| Ação | Implementar / Ponderar / Arquivar |
| Status | Aguardando val. / Implementar / Aprovação / Implementação / Encerrada / Arquivada |
| Solicitante | Quem propôs |
| Unidade / Processo | |
| Categoria | (se tiver) |
| Custo estimado | (se preenchido) |
| Data criação | |
| Data encerramento | |

### Filtros

- **Ação** (Implementar / Ponderar / Arquivar)
- **Status** (cada estado do fluxo)
- **Solicitante** (pessoa)
- **Unidade**, **Processo**, **Categoria**
- **Período** (criação ou encerramento)
- **P × I** (filtrar por quadrante específico)

### Detalhe da oportunidade (clicar na linha)

```
┌──────────────────────────────────────────────────────────────────────┐
│ OPP-007 · Substituir bombonas inox · Implementação 60%                │
│                                                                       │
│ ┌─Resumo─┬─Avaliação─┬─Plano─┬─Implementação─┬─Histórico─┐            │
│                                                                       │
│ Resumo                                                                │
│ Descrição: ...                                                        │
│ Solicitante: Carlos Ferreira (28/04/2026)                             │
│ Categoria: Redução de resíduo                                         │
│ Custo estimado: R$ 25.000                                             │
│                                                                       │
│ Avaliação                                                             │
│  Probabilidade: Alta (3/3)                                            │
│  Impacto: Alto (3/3)                                                  │
│  Quadrante: Implementar 🟢                                             │
│                                                                       │
│ Plano                                                                 │
│  AC-1: Cotar 3 fornecedores — Pedro, prazo 10/05 ✓                    │
│  AC-2: Comprar 50 unidades — Pedro, prazo 30/05 (em curso)            │
│  AC-3: Treinar equipe — RH, prazo 15/06                               │
│                                                                       │
│ Validação final                                                       │
│  Prevista para: 30/08/2026                                            │
│  Verificador: Beatriz Brito                                           │
│  Critério: redução > 70% no descarte de bombonas plásticas             │
└──────────────────────────────────────────────────────────────────────┘
```

### Ações no detalhe

| Ação | Quem pode |
|---|---|
| Editar descrição | `opp.update` ou solicitante (se ainda não validada) |
| Avaliar P × I | `opp.validate` |
| Pedir reavaliação | `opp.validate` (se está em "Ponderar" e quer mudar) |
| Aprovar / Reprovar plano | aprovador designado |
| Marcar AC como concluída | responsável da AC |
| Verificar eficácia final | verificador |
| Arquivar | `opp.update` |

## Permissões

| Ação | Permissão |
|---|---|
| Ver Tarefas / Consulta | (default) |
| Ver de outros | `opp.tasks.read_all` |
| Criar oportunidade | `opp.create` |
| Validar (atribuir P×I) | `opp.validate` |
| Editar configuração | `opp.config.update` |

## Estados especiais

### Tarefas vazias
"Nenhuma oportunidade pendente". 🎉

### Consulta vazia
"Nenhuma oportunidade cadastrada. [+ Nova oportunidade]"

### Filtros sem retorno
"Sem resultados para os filtros aplicados."

## Exemplo Seven — uso típico mensal

Beatriz dedica meia hora toda sexta:

1. Vai em **Tarefas → Validação**.
2. Vê 5 novas oportunidades cadastradas durante a semana.
3. Avalia cada uma com a equipe (P × I).
4. 2 caem em "Implementar" → atribui responsáveis e prazos.
5. 2 caem em "Ponderar" → marca para discutir na reunião mensal.
6. 1 cai em "Arquivar" → escreve justificativa, fecha. Solicitante recebe e-mail explicando por que não vai pra frente.

Em **Tarefas → Implementação**:
- Acompanha o que está sendo executado pelos responsáveis.
- Cobra atrasados.

Em **Consulta**:
- Filtra "Encerradas com sucesso" no último trimestre.
- Apresenta na reunião mensal: "Implementamos 8 melhorias, economia estimada R$ 130k."
