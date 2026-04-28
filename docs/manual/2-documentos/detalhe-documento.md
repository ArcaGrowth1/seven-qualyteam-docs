# Documentos — Detalhe do documento

## Onde fica

Ao **clicar em qualquer documento** na Consulta ou em uma Tarefa.

URL típica: `/search/<id>/revision/<revision_id>?showDetails=true`

## Quem acessa

`docs.read`. Anexos só são baixáveis se o usuário tem permissão de leitura do documento.

## Layout — duas colunas

```
┌────────────────────────────────────┬─────────────────────────────────┐
│ ← Voltar  ⛶ ↻ ⊟ ⊞ │ < 1 / 2 > │ ⎙ │ Título do documento          ✕  │
│                                    │ POP-007: 03 (Em vigor)           │
│                                    │ ─────────────                    │
│ [ visualizador PDF embutido ]      │  [ Editar cadastro ]             │
│                                    │  [ Revisar ]                     │
│                                    │     Inativar                     │
│                                    │ ─────────────                    │
│                                    │ ▾ Informações Básicas            │
│                                    │ ▾ Anexos                         │
│                                    │ ▾ Revisões                       │
│                                    │ ▾ Responsáveis                   │
│                                    │ ▾ Visualizações                  │
│                                    │ ▾ Controle                       │
│                                    │                                  │
│                                    │       [ Compartilhar ]           │
└────────────────────────────────────┴─────────────────────────────────┘
        Coluna esquerda                    Coluna direita
        (visualizador PDF)                 (painel de informações + ações)
```

## Coluna esquerda — Visualizador PDF

Toolbar do topo:

| Ícone | O que faz |
|---|---|
| **← Voltar** | Volta para a Consulta ou Tarefa |
| ⛶ Fullscreen | Maximiza o PDF |
| ↻ Rotação | Gira em 90° |
| ⊟ Reduzir / ⊞ Ampliar | Zoom out / in |
| `< 1 / 2 >` | Paginação |
| ⎙ Imprimir | Imprime (se permitido) |
| ⬇ Download | Baixa o PDF (se permitido) |
| 📋 Detalhes | Mostra/esconde a coluna direita |

> Documentos sensíveis podem ter download/print desabilitados — depende da configuração.

## Coluna direita — Painel de informações

### Cabeçalho do painel

```
Título do documento
POP-007: 03 (Em vigor)
─────
[ Editar cadastro ]    ← ação primária
[ Revisar ]            ← ação secundária
   Inativar            ← ação destrutiva (link, sem botão)
```

### Ações principais

| Botão | O que faz | Permissão |
|---|---|---|
| **Editar cadastro** | Abre form para editar metadados (título, categoria, unidades, processo, responsáveis, validade). Não troca o arquivo | `docs.update` |
| **Revisar** | Inicia uma nova revisão do documento. Cria revisão N+1 em "Em elaboração". Original continua "Publicado" até a nova ser aprovada | `docs.update` ou ser elaborador designado |
| **Inativar** | Tira de circulação. Pede justificativa. Documento fica como "Inativo". Cópias controladas são revogadas | `docs.update` |

### Seções colapsáveis

Cada seção abre/fecha clicando no nome.

#### ▾ Informações Básicas

```
Processo: Operações
Unidades organizacionais: CT Caieiras
Categoria: POP - Procedimentos Operacionais Padrão
Validade: 28/04/2027
Periodicidade de revisão: a cada 12 meses
Cadastrado em: 28/04/2026
```

Mostra os metadados que vieram do passo 1 do cadastro. Clicar em "Editar cadastro" abre o form para mudar.

#### ▾ Anexos

```
📄 procedimento-coleta-classe-i-rev03.pdf  (3.2 MB)  ⬇ Baixar
📄 anexo-fluxograma.pdf                    (450 KB)  ⬇ Baixar
   ✏ Substituir   🗑 Remover
[ + Adicionar anexo ]
```

| Ação | O que faz |
|---|---|
| ⬇ Baixar | Faz download do arquivo |
| ✏ Substituir | Troca o arquivo por uma nova versão (mantém o mesmo registro de anexo). Audit log registra |
| 🗑 Remover | Remove o anexo |
| + Adicionar | Anexa novo arquivo |

> O **arquivo principal** do documento (o PDF que aparece no visualizador) está aqui também. Substituir ele requer `docs.published_file.replace`.

#### ▾ Revisões

```
[ Rev 03 (atual) ]   28/04/2026   Beatriz Brito (aprovou)
[ Rev 02 ]           15/03/2025   Beatriz Brito (aprovou)
[ Rev 01 ]           10/06/2024   Beatriz Brito (aprovou)
[ Rev 00 ]           28/04/2024   Beatriz Brito (aprovou)
```

Histórico de versões. Clicar em uma revisão antiga abre o **detalhe daquela revisão** (PDF antigo + metadados antigos).

> Revisões antigas ficam **read-only** — você não consegue editar dados nem trocar o arquivo. É preservação de histórico (auditoria).

#### ▾ Responsáveis

```
Elaboração: Carlos Ferreira     (27/04/2026)
Aprovação:  Beatriz Brito       (28/04/2026)
```

Mostra quem foi designado em cada papel para a revisão atual.

Para mudar: "Editar cadastro" → seção Responsáveis.

#### ▾ Visualizações

```
Total de leituras: 18
Última leitura: João Silva, hoje às 14:32

[ Ver lista completa ]
```

Lista de quem **abriu** o documento (qualquer abertura conta — não confunda com Leitura Obrigatória que precisa ser confirmada).

Útil para saber: "esse POP novo está sendo realmente lido?".

#### ▾ Controle

```
Cópias controladas distribuídas: 3
- João Silva (CT Caieiras)         distribuída em 28/04/2026
- Maria Souza (Matriz)             distribuída em 28/04/2026  [ 🗑 Revogar ]
- Mural CT Caieiras (físico)       distribuída em 28/04/2026  [ 🗑 Revogar ]

[ + Distribuir nova cópia controlada ]

Leitura obrigatória:
- Configurada para: equipe Operações CT Caieiras (15 pessoas)
- 12 confirmaram leitura
- 3 pendentes: Pedro X, Ana Y, Bruno Z
[ Cobrar pendentes ] [ Editar lista ]
```

Aqui ficam:
- **Cópias controladas distribuídas** (registros de quem recebeu cópia oficial).
- **Leitura obrigatória** (lista de pessoas que precisam confirmar leitura).

Ações:

| Ação | O que faz |
|---|---|
| Distribuir nova cópia | Abre form: para quem (usuário ou local físico), data |
| Revogar cópia | Marca como obsoleta, registra no audit |
| Cobrar pendentes | Reenvia e-mail para quem não leu |
| Editar lista | Adiciona/remove pessoas da leitura obrigatória |

### Botão Compartilhar (rodapé do painel)

```
[ Compartilhar ]
```

Abre dialog:
```
Compartilhar documento

  ⊙ Link interno (apenas usuários da empresa logados)
  ⊙ Link público temporário (qualquer pessoa com o link)
       Expira em: [ 7 dias ▾ ]
       ☐ Permitir download
       ☐ Marca d'água com dados da empresa

[ Gerar link ]
[ Cancelar ]
```

Gera URL única. Acessos via link público são auditados (cada acesso grava no audit log: IP + data).

Pode ser revogado a qualquer momento na seção **Controle**.

## Marcar leitura como confirmada (usuário comum)

Se você está na lista de leitura obrigatória de um documento, ao abrir o detalhe, aparece destacado no topo do painel:

```
🔵 Você precisa confirmar a leitura deste documento.
   Atribuído em: 28/04/2026
   [ Confirmo que li ]
```

Clicando, sua confirmação é registrada e some do alerta. Aparece em "Visualizações" e em "Controle → Leitura obrigatória".

## Detalhe de Documento Externo (diferenças)

Para documentos externos, o painel direito tem variações:

```
Título: Licença CETESB nº 32145/2026
─────
[ Editar cadastro ]
[ Substituir versão ]      ← em vez de "Revisar"
   Inativar
─────
▾ Informações Básicas
   Órgão emissor: CETESB
   Número origem: 32145/2026
   Validade: 11/05/2027
   Responsável renovação: Beatriz Brito

▾ Anexos
▾ Histórico de versões      ← em vez de "Revisões"
▾ Visualizações
   (sem seção Controle, normalmente)
```

## Notificações associadas

| Quando | Quem é notificado |
|---|---|
| Você confirmou leitura | Não notifica ninguém — só registra |
| Cópia controlada distribuída pra você | Você (e-mail + in-app) |
| Cópia revogada | Você (e-mail) |
| Documento revisado e republicado | Lista de leitura obrigatória + quem tem cópia controlada |
| Compartilhamento público acessado | Auditável no log (sem notificação) |

## Estados especiais

### Documento ainda em fluxo (não publicado)
- Visualizador pode estar vazio (Elaborador não anexou ainda).
- Ações disponíveis variam conforme estado: Em Elaboração mostra "Enviar para aprovação", Em Aprovação mostra "Aprovar / Reprovar".

### Documento inativo
- Banner amarelo no topo: "Este documento está inativo. Não use."
- Anexos ainda baixáveis (para histórico) mas não recomendado.

### Documento vencido
- Banner vermelho: "Este documento está vencido em DD/MM/AAAA. Iniciar revisão."
- Botão "Iniciar revisão" destacado.

### Você não tem permissão
Página "endereço errado".

## Permissões necessárias

| Ação | Permissão |
|---|---|
| Ver detalhe | `docs.read` |
| Editar cadastro | `docs.update` |
| Revisar (criar nova revisão) | `docs.update` |
| Substituir arquivo publicado | `docs.published_file.replace` |
| Inativar | `docs.update` |
| Distribuir cópia controlada | `docs.read_required.manage` |
| Revogar cópia | `docs.controlled_copy.delete` |
| Definir leitura obrigatória | `docs.read_required.manage` |
| Gerar link público | `docs.read_required.manage` (provisório) |
