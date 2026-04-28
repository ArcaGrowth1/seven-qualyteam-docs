# Documentos — Tarefas → Solicitação

## Onde fica

`Documentos → Tarefas → aba Solicitação` (URL: `/tasks/request`)

## Quem acessa

Default: você. Para ver de outros, `docs.tasks.read_all`.

## O que é "Solicitação"

São **pedidos de novos documentos** ou **pedidos de revisão de documentos existentes**, abertos por usuários comuns para serem analisados pelo Coord. Qualidade ou pelos aprovadores.

Exemplos:
- Operacional pede: "Precisamos de um POP novo para o procedimento de descarte da bombona Y".
- Auditor identifica falha: "Manual MAN-002 precisa ser revisado urgente".
- Cliente exige: "Política de privacidade precisa ser atualizada para LGPD".

## Diferença entre Solicitação e Elaboração

| | Elaboração | Solicitação |
|---|---|---|
| O que faz a pessoa? | **Produz** o documento | **Pede** que se produza ou revise |
| Quando vira tarefa? | Documento já existe e está em fluxo | Pedido foi aberto, aguardando análise |
| Responsável? | Elaborador designado | Coord. Qualidade / aprovador |
| Resultado | Documento publicado | Decisão: aceitar (vira documento real) ou rejeitar |

## O que aparece na lista

```
┌──────────────────────────────────────────────────────────────────────┐
│ Tarefas                                                               │
│ ┌─Elaboração─┬─Solicitação ●─┬─Validade─┐                            │
│                                                                       │
│ [chip: Beatriz Brito ✕]                              [Filtro] [Exportar]│
├──────────────────────────────────────────────────────────────────────┤
│ # │ Tipo de pedido    │ Documento alvo │ Solicitante │ Data │ Status │
│ 1 │ Novo doc          │ —              │ João Silva  │ 22/04│ Aberto │
│ 2 │ Revisão urgente   │ MAN-002        │ Beatriz B.  │ 25/04│ Aberto │
└──────────────────────────────────────────────────────────────────────┘
```

## Tipos de solicitação

| Tipo | O que é | O que gerar |
|---|---|---|
| **Novo documento** | Pedido de criar um doc inexistente | Criar doc interno do zero (Cadastrar) |
| **Revisão** | Pedido de atualizar um doc publicado | Iniciar nova revisão no doc-alvo |
| **Inativação** | Pedido de tirar um doc de uso | Inativar com justificativa |
| **Cópia controlada** | Pedido de receber cópia de um doc para uso local | Distribuir cópia controlada para o solicitante |

## Como a tarefa **cai** aqui

1. Usuário comum (Operacional, Auditor, etc.) abre a tela de **"Solicitar documento"** (atalho ou botão na Consulta).
2. Preenche: tipo de pedido + documento alvo (se for revisão) + descrição do motivo.
3. Sistema cria a solicitação e atribui ao **Coord. Qualidade** (configurável) ou ao **Aprovador designado** do documento.
4. Você (responsável) vê na sua aba Solicitação.
5. Recebe e-mail.

## Cada coluna explicada

| Coluna | O que mostra |
|---|---|
| **Tipo de pedido** | Novo / Revisão / Inativação / Cópia controlada |
| **Documento alvo** | Se for revisão / inativação / cópia, qual doc. Em "Novo", fica vazio |
| **Solicitante** | Quem abriu o pedido |
| **Data** | Quando o pedido foi aberto |
| **Status** | Aberto / Em análise / Aprovado / Rejeitado |

## Filtros disponíveis

| Filtro | O que filtra |
|---|---|
| **Tipo de pedido** | Novo / Revisão / etc. |
| **Status** | Aberto / Em análise / Aprovado / Rejeitado |
| **Solicitante** | Pessoa que abriu |
| **Documento alvo** | Filtro por doc específico |
| **Período** | Data do pedido |

## Ações na tarefa (clicar na linha → abre detalhe)

### Aceitar pedido (criar documento ou iniciar revisão)
- **Pedido de novo doc**: abre o wizard de cadastro pré-preenchido com info do pedido.
- **Pedido de revisão**: abre a tela de revisão do doc alvo, com você como Elaborador.
- **Pedido de cópia**: abre tela de distribuição de cópia controlada.

### Rejeitar pedido
- AlertDialog pedindo justificativa obrigatória.
- Pedido fica como "Rejeitado".
- Solicitante recebe e-mail com motivo.

### Comentar
- Adicionar comentário sem aceitar nem rejeitar (ex: "Estou avaliando, dá uma semana").
- Solicitante recebe.

### Reatribuir
- Mandar para outro responsável.

## Notificações

| Quando | Quem | Canal |
|---|---|---|
| Pedido criado | Você (responsável) | E-mail + in-app |
| Você aceitou e iniciou ação | Solicitante | E-mail |
| Você rejeitou | Solicitante | E-mail com motivo |
| Comentou | Solicitante | E-mail |

## Estados especiais

### Lista vazia
```
   Nenhuma solicitação pendente
```
Significa: ninguém pediu nada ainda. Normal em SGQs maduros.

### Pedido sem documento alvo (Novo doc) que expira
Após aceitar e gerar o documento real, a solicitação fica **Vinculada** ao novo documento.

## Permissões necessárias

| Ação | Permissão |
|---|---|
| Ver Solicitações | (default) |
| Aceitar pedido (criar doc) | `docs.internal.create` |
| Aceitar pedido de revisão | `docs.update` |
| Rejeitar | (default) |
| Reatribuir | `docs.update` |

## Exemplo Seven

**Cenário**: a Coord. Operacional do CT Caieiras percebe que precisa de um POP para um novo tipo de resíduo Classe I.

1. Ela acessa Documentos → botão "Solicitar documento" → tipo "Novo documento".
2. Preenche descrição: "Necessário POP para tratamento do resíduo XYZ que vamos receber a partir de junho".
3. Sistema cria a solicitação, atribui à Beatriz (Coord. Qualidade).
4. Beatriz vê em Tarefas → Solicitação.
5. Beatriz aceita. Abre o wizard de cadastrar documento interno, **com a descrição copiada do pedido**.
6. Beatriz preenche os campos (Categoria=POP, Unidade=CT Caieiras, Processo=Tratamento, Elaborador=Carlos Operações, Aprovador=Beatriz).
7. Grava → POP novo aparece em "Em elaboração" → cai na aba **Elaboração** de Carlos.
8. Solicitante (Coord. Operacional) recebe e-mail "Sua solicitação foi aceita. Documento POP-009 criado."
