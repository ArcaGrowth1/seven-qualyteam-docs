# Administrador — Grupos de permissões

## Onde fica

`Administrador → Grupos de permissões` (URL: `/roles`)

## Quem acessa

- Ver: `admin.roles.read`.
- Criar: `admin.roles.create`.
- Editar: `admin.roles.update`.
- Excluir: `admin.roles.delete`.

## O que é um "Grupo de permissões"

Um **conjunto de capacidades** que você atribui a usuários. Em vez de configurar permissões pessoa por pessoa, você configura por grupo e coloca pessoas no grupo.

Exemplos típicos:
- **Admin** (acesso total)
- **Coord. Qualidade** (gerencia documentos, NCs, riscos)
- **Auditor** (lê tudo, não edita)
- **Operacional** (registra ocorrências, lê documentos)

## O que aparece na lista

```
┌─────────────────────────────────────────────────────────────────┐
│ Grupos de permissões              [Novo grupo de permissões]    │
│ Total: 02                                                        │
├─────────────────────────────────────────────────────────────────┤
│ Nome ↑    │ Tipo                │ Usuários ativos               │
│ Admin *   │ Padrão do sistema   │ 1                              │
│ Padrão    │ Criado pelo usuário │ 0                              │
└─────────────────────────────────────────────────────────────────┘
```

## Cada coluna

| Coluna | O que mostra |
|---|---|
| **Nome** | Nome do grupo. "Admin *" tem asterisco indicando ser fixo do sistema |
| **Tipo** | "Padrão do sistema" (não excluível) ou "Criado pelo usuário" (excluível) |
| **Usuários ativos** | Quantas pessoas pertencem a esse grupo no momento |

## Como criar um grupo novo

1. Clica em **Novo grupo de permissões**.
2. Abre tela "Editar grupo de permissões" com 2 abas:
   - **Permissões** (escolher o que o grupo pode fazer)
   - **Usuários** (vazia por enquanto)
3. Em **Permissões**:
   - Preenche **Nome do grupo** *(obrigatório, ex: "Coord. Ambiental")*
   - Banner explicativo: "Permissões essenciais ao sistema não podem ser desmarcadas."
   - Marca as permissões desejadas (organizadas por blocos: Administrador, Documentos, Não conformidades, Oportunidades, Riscos).
4. Clica **Gravar**.
5. Toast verde, volta para lista.

## Anatomia da tela de edição

```
┌──────────────────────────────────────────────────────────────────┐
│ Editar grupo de permissões                                        │
│ ┌─────────┬─────────┐                                             │
│ │Permissões│Usuários│                                             │
│ └─────────┴─────────┘                                             │
│                                                                   │
│ Nome do grupo de permissões *                                     │
│ [ Admin *                                              ] 7/100    │
│                                                                   │
│ ⓘ Permissões essenciais ao sistema não podem ser desmarcadas. ✕  │
│                                                                   │
│ ─────  Administrador  ──────────────────────────────────────     │
│ ☑ Ver lista de usuários    ☑ Adicionar usuários    ...           │
│ ☑ Editar usuários          ☑ Inativar usuários                   │
│ ☑ Cadastrar processos      ☑ Editar processos                    │
│ ☑ Cadastrar campos personalizados                                 │
│ ☑ Editar campos personalizados                                    │
│                                                                   │
│ ─────  Documentos  ─────────────────────────────────────────     │
│ ☑ Cadastrar documento interno                                     │
│ ☑ Cadastrar documento externo                                     │
│ ☑ Deletar documentos       ☑ Editar documentos                    │
│ ☑ Consultar documentos     ☑ Exportar consulta                    │
│ ☑ Ver dashboard            ☑ Alterar arquivo publicado            │
│ ☑ Excluir cópia controlada distribuída                            │
│ ☑ Controle de leitura obrigatória                                 │
│                                                                   │
│ ─────  Não conformidades  ──────────────────────────────────     │
│ ☑ Registrar RNC            ☑ Excluir RNC                          │
│ ☑ Editar RNC aberta        ☑ Encerrar ocorrência                  │
│ ☑ Adicionar origem         ☑ Editar configuração do módulo        │
│  ...                                                              │
│                                                                   │
│ ─────  Oportunidades  ──────────────────────────────────────     │
│ ☑ Criar / Editar / Consultar oportunidades                        │
│ ☑ Validar oportunidades                                           │
│  ...                                                              │
│                                                                   │
│ ─────  Riscos  ─────────────────────────────────────────────     │
│ ☑ Configurar unidades organizacionais                             │
│ ☑ Inativar e reativar riscos                                      │
│ ☐ Excluir riscos     ← perigoso, raramente liberado              │
│                                                                   │
│                              [ Gravar ]   [ Cancelar ]            │
└──────────────────────────────────────────────────────────────────┘
```

## Permissões essenciais (cinza claro)

Algumas permissões aparecem com checkbox **desabilitado e marcado em cinza**. Significam: "essa permissão é parte do mínimo para o sistema funcionar — você não pode desmarcar".

Exemplo: "Ver lista de usuários" no grupo Admin é essencial. Mesmo desmarcando, ela volta.

> Em grupos não-Admin (criados pelo usuário), a maioria das permissões é desmarcável. As "essenciais" só aparecem no grupo Admin original.

## Aba Usuários do grupo

Lista todos os usuários que pertencem àquele grupo.
- Coluna: Nome, E-mail, Status (Ativo/Inativo).
- Permite **remover** usuário do grupo (move para outro grupo).
- Não permite cadastrar usuário daqui — vá em **Usuários** para isso.

## Como editar um grupo existente

- Clica na linha do grupo na lista.
- Mesma tela de criação, com permissões já marcadas.
- Marca/desmarca o que precisa.
- **Gravar**.
- Mudança vale **imediatamente** para todos os usuários do grupo.

## Como excluir um grupo

⚠️ Só funciona se:
1. Não é "Padrão do sistema" (Admin *).
2. **Não tem usuários ativos vinculados** (a coluna "Usuários ativos" precisa estar em 0).

Se tem usuários, faça antes:
1. Vai em **Usuários**.
2. Edita cada usuário do grupo, troca para outro grupo.
3. Volta aqui e exclui.

## Como pessoas ficam num grupo

Não é nesta tela.
Vá em **Administrador → Usuários**, edite o usuário e troque o grupo dele.

## Ordem de blocos na lista de permissões

A organização visual é sempre: **Administrador → Documentos → Não conformidades → Oportunidades → Riscos**. Outros módulos contratados aparecem nessa ordem.

## Notificações

- Mudança em grupo **não notifica** os usuários afetados.
- Vai pro audit log: `admin.role.updated` com diff de permissões antes/depois.

## Estados especiais

### Grupo sem nenhuma permissão marcada
Permitido salvar (vira grupo "leitor zero"). Mas usuários nele não vão ver nada.

### Grupo com nome duplicado
Bloqueado: erro "Já existe grupo com esse nome".

### Tentar excluir Admin *
Bloqueado: erro "Grupo padrão do sistema não pode ser excluído".

## Exemplo Seven — proposta de grupos

**Grupo: Admin Seven**
- Tudo marcado.
- Apenas 2-3 pessoas no máximo (você + 1 backup).

**Grupo: Coord. Ambiental Seven**
- Admin: ler usuários, processos, unidades, audit log. Não criar/excluir usuário.
- Documentos: tudo.
- NC: tudo, exceto Excluir RNC.
- Oportunidades: tudo.
- Riscos: tudo, exceto Excluir riscos.

**Grupo: Encarregado de campo Seven**
- Documentos: só Consultar.
- NC: Registrar ocorrência, Editar e excluir ocorrência (suas), Encerrar ocorrência.
- Sem acesso a Riscos, Oportunidades, Admin.

**Grupo: Auditor Externo / Cliente Seven**
- Admin: ler audit log.
- Cada módulo: só Consultar e Exportar.
- Sem criar / editar / excluir nada.
