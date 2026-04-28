# Documentos — Cadastrar documento interno

## Onde fica

`Documentos → Documento interno` (URL: `/new/internal-document`)

> Tem mais de uma forma de chegar:
> - Aba "Documento interno" no top-bar.
> - Botão "+ Novo documento" em qualquer lista de Consulta de internos.
> - Pelo aceite de uma Solicitação do tipo "Novo documento".

## Quem acessa

`docs.internal.create`. Geralmente: Coord. Qualidade.

## Quando usar

- Criar um POP / Política / Manual / Procedimento / Instrução de trabalho.
- Documento que **vai ser elaborado dentro da empresa** e tem ciclo de aprovação interno.

Para licenças, certificados, normas externas: use **Documento externo** em vez disso.

## O wizard (4 passos)

```
            ┌── 1. Informações ──┬── 2. Responsável ──┬── 3. Documentos associados (opcional) ──┬── 4. Configuração (opcional) ──┐
```

### Passo 1: Informações

```
Cadastrar documento interno
[1] Informações ●  [2] Responsável  [3] Doc. assoc. (opc)  [4] Config. (opc)

Informações do documento

Título *
[ ........................................................... ]
                                                       0/500

Categoria *
[ Selecione                                                   ▾]

Unidades organizacionais *
[ Selecione                                                   ▾]

Processo *
[ Selecione                                                   ▾]

Número sequencial          Revisão *
[                ]          [ 0       ]

[ Próximo ]
```

#### Campos do passo 1

| Campo | O que é | Obrigatório? | De onde vem |
|---|---|---|---|
| **Título** | Nome do documento (ex: "Procedimento de derramamento de óleo") | ✅ | Você digita. Max 500 caracteres |
| **Categoria** | Tipo (POP, Política, Manual, Norma, Instrução de trabalho…) | ✅ | Dropdown — categorias cadastradas pela sua empresa |
| **Unidades organizacionais** | Para quais unidades este doc se aplica | ✅ | Multi-select — vem do Admin → Unidades |
| **Processo** | Área funcional (Produção, Operações, Ambiental…) | ✅ | Vem do Admin → Processos |
| **Número sequencial** | Numeração interna (ex: 001, 002…) | Opcional | Você define ou sistema sugere baseado em sigla + último número |
| **Revisão** | Versão. Default 0 | ✅ | Sistema preenche 0 no novo, incrementa em revisões |

> 📝 **Como cadastrar Categorias?**
> Categorias não são criadas pela Admin direto, são cadastradas dentro do módulo Documentos por usuários com permissão. Em geral, o Coord. Qualidade cria as categorias antes do primeiro uso. Categorias típicas: **POP** (Procedimento Operacional Padrão), **Manual**, **Política**, **Instrução de Trabalho (IT)**, **Formulário (FOR)**, **Plano**, **Procedimento (PR)**.

### Passo 2: Responsável

```
[2] Responsável ●

Quem elabora?
Elaborador *
[ Selecione pessoa                                            ▾]

Quem aprova?
Aprovador *
[ Selecione pessoa                                            ▾]

[ Voltar ]   [ Próximo ]
```

#### Campos do passo 2

| Campo | O que é | Obrigatório? |
|---|---|---|
| **Elaborador** | Quem vai produzir / anexar o arquivo do documento | ✅ |
| **Aprovador** | Quem vai aprovar antes de publicar | ✅ |

> Os dois podem ser a mesma pessoa? **Não** — o sistema bloqueia. ISO exige separação de funções: quem elabora não aprova.

> Quem aparece nos dropdowns? Todos os usuários ativos da sua empresa.

### Passo 3: Documentos associados (opcional)

```
[3] Documentos associados (opcional)

Vincule documentos relacionados (referências, formulários filhos):
[ + Adicionar documento ]

Sem documentos associados.

[ Voltar ]   [ Próximo ]   [ Pular ]
```

Use para vincular outros documentos relacionados:
- POP que referencia um Manual.
- Procedimento que tem formulário filho.
- Documento que substitui outro.

Não é obrigatório. Pode pular ou adicionar depois.

### Passo 4: Configuração (opcional)

```
[4] Configuração (opcional)

Validade
☐ Tem data de validade
   Validade até: [ DD/MM/AAAA ]
   Notificar X dias antes do vencimento: [ 30 ]

Periodicidade de revisão
☐ Revisar periodicamente
   A cada: [ 12 ] meses

Leitura obrigatória
☐ Definir leitura obrigatória após publicação
   Para quem? [ Selecione usuários ou grupos ]

Cópia controlada
☐ Distribuir como cópia controlada após publicação
   Para quem? [ Selecione ]

[ Voltar ]   [ Gravar ]
```

#### Cada toggle

| Toggle | O que faz |
|---|---|
| **Tem data de validade** | Define `valid_until`. Sistema vai notificar X dias antes do vencimento |
| **Revisar periodicamente** | Após cada publicação, recalcula próxima validade automaticamente (ex: 12 meses depois da publicação) |
| **Leitura obrigatória** | Após publicar, lista de usuários selecionados precisa confirmar leitura. Aparece para eles na **aba Tarefas → Solicitação** ou via notificação |
| **Cópia controlada** | Após publicar, sistema cria registros de cópia controlada distribuída para os usuários/locais selecionados |

> Você pode pular este passo e configurar depois no detalhe do documento.

## Botão "Gravar"

Ao clicar:
1. Documento é criado com status **"Em elaboração"** (porque tem Elaborador definido).
2. Toast verde: "Documento POP-007 criado com sucesso".
3. Sistema redireciona para o **detalhe do documento** (não para a lista).
4. **Notifica o Elaborador** por e-mail e in-app: "Você foi designado elaborador de POP-007. Acesse para anexar o arquivo."
5. Aparece na aba **Tarefas → Elaboração** do Elaborador.

## Como o Código (POP0007:00) é gerado

- **Sigla**: vem da **Categoria** (ex: POP, MAN, POL).
- **Número**: sequencial dentro da categoria. Sistema procura o último número usado e incrementa.
- **Revisão**: começa em 00.

Exemplo: se a categoria "POP" já tem 6 documentos, o novo será POP-007.

## Ações posteriores no documento (após criar)

Vão para a tela de [Detalhe do documento](detalhe-documento.md):
- Editar cadastro
- Anexar arquivo (Elaborador faz)
- Enviar para aprovação
- Aprovar / Reprovar (Aprovador faz)
- Publicar
- Inativar

## Notificações disparadas pelo cadastro

| Quando | Quem | Canal |
|---|---|---|
| Cadastro criado | Elaborador | E-mail + in-app: "Você foi designado elaborador" |
| Após arquivo anexado e enviado | Aprovador | E-mail + in-app: "Doc esperando sua aprovação" |
| Aprovado | Lista de leitura obrigatória (se configurada) | E-mail + in-app: "Novo POP publicado, leitura obrigatória" |
| Aprovado | Solicitante (se veio de Solicitação) | E-mail: "Sua solicitação resultou no documento X" |

## Estados especiais durante o cadastro

### Categoria não cadastrada
Aparece "Sem opções" ou lista vazia. Solução: cadastrar a categoria primeiro (admin de Documentos faz isso).

### Sigla + número duplicado
Bloqueio: "Já existe documento POP-007 ativo. Próximo número disponível: POP-008".

### Validade no passado
Bloqueio: "A data de validade precisa ser futura".

### Elaborador = Aprovador
Bloqueio: "Elaborador e Aprovador não podem ser a mesma pessoa".

## Permissões necessárias

| Ação | Permissão |
|---|---|
| Acessar este wizard | `docs.internal.create` |
| Definir leitura obrigatória | `docs.read_required.manage` |

## Audit log

- `docs.document.created`
- `docs.elaboration.assigned`
- (Outras ações ficam em outras telas — anexar, aprovar, etc.)

## Exemplo Seven — passo a passo real

**Cenário**: criar POP "Procedimento de coleta de resíduos perigosos" para o CT Caieiras.

**Passo 1 — Informações**:
- Título: "Procedimento de coleta de resíduos Classe I"
- Categoria: POP
- Unidades: CT Caieiras
- Processo: Operações
- Número sequencial: deixa em branco (sistema sugere POP-007)
- Revisão: 0

**Passo 2 — Responsável**:
- Elaborador: Carlos Ferreira (encarregado operações)
- Aprovador: Beatriz Brito (Coord. Qualidade)

**Passo 3 — Documentos associados**:
- Pular.

**Passo 4 — Configuração**:
- ☑ Tem data de validade → 28/04/2027 (revalidar em 1 ano)
- ☑ Revisar periodicamente → a cada 12 meses
- ☑ Leitura obrigatória → toda equipe de operações do CT Caieiras
- ☐ Cópia controlada (não vamos imprimir)

**Gravar**:
- POP-007 criado, em elaboração.
- Carlos recebe e-mail e ve em Tarefas → Elaboração.
- Carlos abre, anexa o PDF do procedimento, envia para aprovação.
- Beatriz vê em Tarefas → Solicitação (que aqui também serve como "aprovação").
- Beatriz aprova.
- POP-007 vira Publicado.
- Toda equipe de operações do CT Caieiras recebe e-mail "Novo POP publicado, leitura obrigatória".
- 28/03/2027 (30 dias antes): Beatriz recebe alerta de validade.
