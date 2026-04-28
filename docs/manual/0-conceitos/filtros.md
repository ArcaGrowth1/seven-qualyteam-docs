# Como funcionam os filtros

Toda lista tem botão **Filtros** no canto superior direito. Clicar abre um painel lateral à direita com os filtros disponíveis para aquela tela.

## Tipos de filtros

### Dropdown simples
```
Status
[ Selecione  ▾ ]
```
Escolhe um valor. Já filtra ao selecionar.

### Multi-select (com chips)
```
Unidades organizacionais
[ Matriz ✕ ] [ Fábrica de Berlim ✕ ] [+]
```
Vários valores. Filtra por **qualquer um deles** (lógica OU dentro do mesmo filtro).

### Faixa de data
```
Período
De: [ 01/01/2026 ]    Até: [ 31/01/2026 ]
```
Filtra registros com data **dentro do intervalo** (inclusivo).

### Texto livre / busca
```
Buscar por descrição
[ ............................. ]
```
Filtra por trecho parcial de qualquer palavra (case-insensitive). Filtra ao parar de digitar (debounce ~500ms).

### Pessoa (responsável, solicitante)
```
Responsável
[ Selecione pessoa  ▾ ]
```
Dropdown com busca. Mostra usuários ativos da sua empresa.

## Lógica entre filtros

- Filtros **diferentes** = lógica E (precisa atender todos).
- Filtros do **mesmo campo** com vários valores = lógica OU.

**Exemplo**: filtrei "Unidades = [Matriz, Fábrica de Berlim]" + "Status = Aberta"
→ Mostra registros que estão (em Matriz **OU** em Fábrica de Berlim) **E** com status Aberta.

## Filtros mais comuns por módulo

### Documentos — Consulta
| Filtro | O que filtra |
|---|---|
| **Categoria** | Tipo de documento (POP, Política, Manual…). Vem dos cadastros feitos pelo Admin |
| **Unidades organizacionais** | Filtra docs vinculados àquelas unidades |
| **Processo** | Vendas, RH, Produção, Gestão da Qualidade, Financeiro (vem do Admin) |
| **Status** | Em elaboração / Em aprovação / Publicado / Vencido / Inativo |
| **Validade** | "Vencendo em N dias" / "Vencido" / "Sem validade" |
| **Responsável (Elaborador / Aprovador)** | Pessoa designada nesses papéis |
| **Período de cadastro** | Data em que o doc foi criado |

### Documentos — Tarefas
| Filtro | O que filtra |
|---|---|
| **Responsável** | Default = você (clicar `✕` no chip mostra de todos, se tem permissão `docs.tasks.read_all`) |
| **Aba ativa** (Elaboração / Solicitação / Validade) | Filtra automaticamente pelo tipo da tarefa |

### Não Conformidades — Dashboard
Filtros globais que afetam **todos os widgets** ao mesmo tempo:
| Filtro | O que filtra |
|---|---|
| **Unidades organizacionais** | NCs/Ocorrências daquelas unidades |
| **Processos** | Daqueles processos |
| **Origem** | Auditoria, Reclamação cliente, Inspeção, Indicador… (configurável no módulo) |

### Riscos — Tarefas / Consulta
| Filtro | O que filtra |
|---|---|
| **Unidade organizacional** | Riscos da unidade selecionada (cada unidade tem matriz própria) |
| **Categoria** | Tipo de risco (configurável) |
| **Tratamento** | Reter / Mitigar / Evitar |
| **Status** | Em tratamento / Monitorado / Para reavaliação |

## Filtros persistem na URL

Quando você aplica filtros, a URL muda automaticamente, exemplo:
```
/search/internal-documents?status=published&unitIds=abc,def&page=2
```

Implicações práticas:
- ✅ **Bookmark** uma visão filtrada útil ("Meus docs vencendo este mês").
- ✅ **Compartilhar URL** com colega = ele vê a mesma visão.
- ✅ **Voltar do navegador** restaura os filtros que estavam.
- ⚠️ Se o sistema for atualizado e os parâmetros mudarem, bookmarks antigos podem não funcionar.

## Limpar filtros

Botão **Limpar filtros** no painel lateral remove todos.
Cada chip individual tem `✕` para remover só ele.

## Filtro vs Busca global

- **Filtro**: refina a lista atual (escopo: aquela tela).
- **Busca global** (atalho `S`): procura em todo o sistema, em qualquer módulo. Veja [Busca](busca.md).
