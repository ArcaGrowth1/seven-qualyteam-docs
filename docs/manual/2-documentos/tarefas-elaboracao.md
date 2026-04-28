# Documentos — Tarefas → Elaboração

## Onde fica

`Documentos → Tarefas → aba Elaboração` (URL: `/tasks/elaboration`)

É a aba que abre por padrão quando você clica em **Tarefas** no top-bar de Documentos.

## Quem acessa

Qualquer usuário do módulo Documentos (não exige permissão extra).

> Por padrão, mostra **só as suas tarefas** (filtro de Responsável = você). Para ver de outras pessoas, precisa de `docs.tasks.read_all` e remover o chip de filtro.

## O que aparece aqui

Lista de **documentos onde você é o Elaborador designado** e que ainda não foram concluídos.

Resumindo: **"o que eu preciso elaborar"**.

```
┌──────────────────────────────────────────────────────────────────────┐
│ Tarefas                                                               │
│ ┌─Elaboração ●─┬─Solicitação─┬─Validade─┐                           │
│                                                                       │
│ [chip: Beatriz Brito ✕]                              [Filtro] [Exportar]│
├──────────────────────────────────────────────────────────────────────┤
│ # │ Código │ Documento     │ Categoria │ Etapa │ Atribuído em │ Prazo │
│ 1 │ POP-007│ Procedimento..│ POP       │ Em el.│ 22/04/26     │ 5 dias │
│ 2 │ MAN-002│ Manual de seg.│ Manual    │ Em el.│ 25/04/26     │ 12 dias│
└──────────────────────────────────────────────────────────────────────┘
```

## Como uma tarefa **cai** aqui

Uma tarefa de elaboração aparece nesta lista quando:

1. Alguém cadastra um documento interno (via **+ Novo documento interno**).
2. No passo 2 do wizard ("Responsável"), define **Elaborador = você**.
3. Após gravar, o documento entra com status **"Em elaboração"**.
4. Você passa a ver na sua aba Elaboração.
5. Você recebe **e-mail e notificação in-app**: "Você foi designado elaborador de POP-007".

## Cada coluna explicada

| Coluna | O que mostra | De onde vem |
|---|---|---|
| **#** | Número sequencial | Da tabela |
| **Código** | Código do documento (ex: POP-007:00) | Gerado automaticamente baseado em sigla + sequência + revisão |
| **Documento** | Título do documento | Campo "Título" do passo 1 do cadastro |
| **Categoria** | Tipo de documento | Campo "Categoria" do passo 1 |
| **Etapa** | Onde está no fluxo (Em elaboração / Em aprovação / Publicado…) | Status do documento |
| **Atribuído em** | Data em que você foi colocado como Elaborador | Campo `assigned_at` |
| **Prazo** | Tempo restante até o prazo de elaboração (se configurado) | Cálculo: prazo − hoje |

> Algumas colunas podem ser ocultadas/mostradas via menu três-pontos.

## Filtros disponíveis

| Filtro | O que filtra |
|---|---|
| **Responsável** | Default = você. Limpa o chip e seleciona outros (precisa de `docs.tasks.read_all`) |
| **Status / Etapa** | Em elaboração / Aprovação / Publicado |
| **Unidade organizacional** | Documentos vinculados àquela unidade |
| **Processo** | Documentos do processo |
| **Categoria** | POP / Manual / Política / etc. |
| **Atribuído em** | Faixa de data |
| **Prazo** | "Atrasado" / "Vencendo em até N dias" / "Sem prazo" |

## Cada botão / ação

### Clicar na linha
Abre o **detalhe do documento** (ver [Detalhe do documento](detalhe-documento.md)).
Lá você vê o painel de "Editar cadastro / Revisar / Inativar" + seções colapsáveis.

### + Novo documento interno (canto superior direito)
Atalho: leva para `/new/internal-document`.

### Filtros
Botão direita: abre painel de filtros.

### Exportar
Gera CSV com a lista atual (precisa de `docs.export`).

## Como **trabalhar** uma tarefa de elaboração (passo a passo)

1. Clica na linha do documento.
2. Abre detalhe. À direita o painel mostra botão **Editar cadastro** (se faltar info) ou direto a interface de elaboração.
3. **Anexa o arquivo PDF** do documento (campo Anexos do passo 1).
4. Preenche dados restantes se faltar.
5. Clica **"Enviar para aprovação"** (botão na ação principal do detalhe).
6. Documento muda de status → **"Em aprovação"**.
7. Sai da sua aba Elaboração e cai na aba do **Aprovador designado**.
8. Aprovador recebe e-mail e notificação.

## Para onde vai depois

Depois que você envia para aprovação:
- A tarefa **some da sua lista de Elaboração**.
- Aparece na aba **Aprovação** do aprovador (não confundir: hoje a aba se chama "Solicitação" no wizard, mas "Aprovação" no fluxo interno).
- Se **reprovado**, volta para **sua aba Elaboração** com status "Em elaboração" + comentário do aprovador.
- Se **aprovado**, vira **Publicado** e some de qualquer aba de Tarefas (vai para Consulta).

## Notificações relacionadas

| Quando | Quem é notificado | Canal |
|---|---|---|
| Você foi designado elaborador (cadastro inicial) | Você | E-mail + in-app |
| Documento reprovado pelo aprovador | Você | E-mail + in-app, com motivo |
| Você reatribui a outra pessoa | Outra pessoa | E-mail + in-app |
| Documento abandonado por > N dias | Você + admin | E-mail (lembrete) |

## Estados especiais

### Lista vazia
```
            [ 📁 ilustração ]

      Nenhuma tarefa pendente

           [ + Novo documento ]
```
Significa: você não tem nada para elaborar agora. 🎉

### Lista vazia mas eu sei que tenho tarefa
- Verifica se o filtro de Responsável está em você (chip "Beatriz Brito").
- Verifica se outro filtro não está restringindo demais (ex: filtro de unidade).
- "Limpar filtros" mostra tudo.

### Tarefa que não consigo concluir
Se o documento está bugado ou você não é mais o responsável:
- Botão "Reatribuir" no detalhe (se você tem permissão).
- Ou pedir para Admin reatribuir via edição do documento.

## Permissões necessárias para "fazer" o trabalho

| Ação | Permissão |
|---|---|
| Ver a aba Tarefas | (default) |
| Ver tarefas de outras pessoas | `docs.tasks.read_all` |
| Editar o documento (anexar arquivo, etc.) | `docs.update` |
| Enviar para aprovação | `docs.update` (ou ser o elaborador designado) |
| Reatribuir | `docs.update` |

## Exemplo Seven

**Cenário**: Beatriz cadastra POP novo "Procedimento de derramamento" e designa João como elaborador.

1. João recebe e-mail "Você foi designado elaborador de POP-007".
2. João abre Documentos → Tarefas → Elaboração.
3. Vê POP-007 na lista.
4. Clica, abre detalhe. Anexa o PDF que ele criou.
5. Clica "Enviar para aprovação". Beatriz é a aprovadora.
6. POP-007 sai da aba de João.
7. Beatriz vê POP-007 na aba **Solicitação** dela (que também serve de "aprovação").
8. Se Beatriz aprovar → POP-007 fica **Publicado** e some das tarefas. Aparece na **Consulta** para todos lerem.
