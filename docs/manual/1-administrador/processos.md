# Administrador — Processos

## Onde fica

`Administrador → Processos` (URL: `/processes`)

## Quem acessa

- Ver: qualquer um com permissão básica de Admin.
- Cadastrar: `admin.processes.create`.
- Editar: `admin.processes.update`.
- Ativar / Inativar: `admin.processes.toggle`.

## O que é "Processo"

Uma **área funcional** da sua empresa (não confundir com fluxo de trabalho). Exemplos típicos:
- Vendas
- Recursos Humanos
- Produção
- Gestão da Qualidade
- Financeiro
- Logística
- Ambiental (relevante para Seven)

Tudo que é cadastrado nos outros módulos (documentos, NCs, riscos, oportunidades) é **vinculado a um Processo**. Permite filtrar e fazer relatórios por área.

## O que aparece na tela

```
┌─────────────────────────────────────────────────────────────────┐
│ Processos                                [+ Adicionar processo] │
│                                                  [Filtros]      │
│ Total: 5                                                        │
├─────────────────────────────────────────────────────────────────┤
│ Sigla ↓ │ Nome ↓        │ Data registro ↓ │ Editar │ Ativar    │
│ ─       │ Vendas        │ 27/04/2026      │   ✏    │   👁      │
│ ─       │ Recursos H... │ 27/04/2026      │   ✏    │   👁      │
│ ─       │ Produção      │ 27/04/2026      │   ✏    │   👁      │
│ ─       │ Gestão da...  │ 27/04/2026      │   ✏    │   👁      │
│ ─       │ Financeiro    │ 27/04/2026      │   ✏    │   👁      │
└─────────────────────────────────────────────────────────────────┘
```

## Cada coluna

| Coluna | O que mostra |
|---|---|
| **Sigla** | Abreviação curta (ex: PRD, GQ, FIN). Opcional na criação |
| **Nome** | Nome completo do processo. Obrigatório |
| **Data de registro** | Quando foi criado |
| **Editar** ✏ | Clique abre o form para editar |
| **Ativar e Inativar** 👁 | Toggle. Olho aberto = ativo, olho fechado = inativo |

## Filtros disponíveis

Botão **Filtros** abre painel:
- **Status**: Ativos / Inativos / Todos
- **Busca por nome**: texto livre

## Como criar um Processo novo

1. Clica em **+ Adicionar processo** (canto superior direito).
2. Abre form simples:
   - **Sigla** (opcional, max ~10 caracteres)
   - **Nome** *(obrigatório)*
3. Clica **Gravar**.
4. Toast verde de sucesso.
5. Volta para a lista, novo processo aparece.

## Como editar

- Clica no ícone **✏** na linha do processo.
- Abre mesmo form, com valores preenchidos.
- Pode editar Sigla e Nome.
- Gravar.

## Ativar / Inativar

- Clica no ícone **👁**.
- Confirma no AlertDialog: "Inativar processo Y?".
- Processo fica indisponível para **novos cadastros** em outros módulos.
- **Cadastros já existentes** que usam esse processo continuam funcionando — apenas não pode mais vincular novos.
- Para reativar: mesma ação.

## Excluir um Processo

⚠️ **Não tem botão de exclusão na lista**. Apenas inativar.

Por quê? Excluir um processo apagaria registros históricos vinculados (documentos, NCs antigas que usaram esse processo). O sistema **prefere preservar histórico**. Se realmente precisar excluir (ex: cadastrou errado e ainda não usou em lugar nenhum), tem que entrar via API ou suporte.

## Onde "Processo" aparece em outros módulos

- **Documentos**: campo obrigatório no cadastro de documento interno e externo.
- **Não Conformidades**: campo obrigatório no cadastro de ocorrência e RNC.
- **Oportunidades**: campo obrigatório no cadastro.
- **Riscos**: campo obrigatório no cadastro do risco.
- **Filtros e Dashboards** de todos os módulos: você pode filtrar por processo.

## Notificações

Cadastro de processo **não dispara notificações** — é configuração silenciosa.

Mas é gravado no audit log:
- `admin.process.created`
- `admin.process.updated`
- `admin.process.toggled` (ativado/inativado)

## Estados especiais

### Lista vazia
Não acontece em conta nova — sistema vem com 5 processos exemplo (Vendas, RH, Produção, GQ, Financeiro). Você pode editar/inativar e cadastrar os seus.

### Processo em uso x inativação
Inativar um processo usado por 0 cadastros: silencioso.
Inativar um processo usado por documentos / NCs: aviso "Este processo está em uso em N documentos. Inativar mesmo assim?".

## Exemplo Seven

A Seven Resíduos provavelmente vai querer estes processos:
- **Operações** (coleta, transporte de resíduos)
- **Tratamento e Destinação** (operação no aterro / ETE)
- **Comercial**
- **Ambiental** (gestão de licenças, MTRs)
- **Manutenção** (frota, equipamentos)
- **Administrativo / Financeiro**
- **RH e Saúde Ocupacional**
- **Qualidade** (gestão do SGI)

Provavelmente você vai querer **inativar** os 5 padrão e cadastrar esses customizados.
