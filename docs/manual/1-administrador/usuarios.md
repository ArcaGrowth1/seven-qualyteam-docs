# Administrador — Usuários

## Onde fica

`Administrador → Usuários` (URL: `/user`)

## Quem acessa

- Ver lista: `admin.users.read`.
- Adicionar: `admin.users.create`.
- Editar: `admin.users.update`.
- Inativar: `admin.users.deactivate`.

## A lista

```
┌─────────────────────────────────────────────────────────────────┐
│ Usuários                       [+ Novo usuário] [Filtros] [↓Exportar] │
│ Total: 01                                                        │
├─────────────────────────────────────────────────────────────────┤
│ Nome           │ E-mail                          │ Grupo │Status │
│ Beatriz Brito  │ meioambiente1@sevenresiduos...  │ Admin*│ Ativo │
└─────────────────────────────────────────────────────────────────┘
```

## Cada coluna

| Coluna | O que mostra |
|---|---|
| **Nome** | Nome completo |
| **E-mail** | Login (também usado para notificações) |
| **Grupos de permissões** | Único grupo a que pertence |
| **Status** | Ativo / Inativo |

## Filtros disponíveis

- **Status**: Ativos / Inativos / Todos.
- **Grupo de permissões**: filtra usuários daquele grupo.
- **Busca por nome ou e-mail**: texto livre.

## Como cadastrar um usuário novo

1. Clica **+ Novo usuário** (canto superior direito).
2. URL muda para `/user/new`. Form:

```
Cadastrar usuário

Nome *
[ Escrever                                              ] 0/100

E-mail *
[ Escrever                                              ]

Senha *
[ Escrever                                              ]

Confirmar a senha *
[ Escrever                                              ]

Grupos de permissões *
[ Selecione                                            ▾]

Idioma *
[ Selecione                                            ▾]

[ Gravar ]
[ Cancelar ]
```

3. Preenche todos os campos obrigatórios:
   - **Nome**: até 100 caracteres.
   - **E-mail**: precisa ser único na empresa. Será o login.
   - **Senha**: você define inicialmente. O usuário pode trocar depois.
   - **Confirmar senha**: igual à senha.
   - **Grupo**: escolha o grupo apropriado (Admin / Coord. Qualidade / Auditor / Operacional / outro).
   - **Idioma**: pt-BR / en-US / es-ES (depende dos suportados).
4. Clica **Gravar**.
5. Toast verde, volta para a lista.

> **Decisão Seven**: usar e-mail corporativo (`@sevenresiduos.com.br`). Não usar e-mails pessoais.

## Como editar um usuário

- Clica na linha → abre detalhe.
- Pode editar: Nome, E-mail, Grupo, Idioma.
- Pode **resetar senha**: gera nova senha temporária + envia por e-mail.
- **Não pode** ver a senha atual (é só hash).

## Como inativar um usuário

⚠️ **Não excluímos** usuários, apenas inativamos. Por que? Para preservar audit log: "fulano fez X em janeiro/2026" continua sendo verdade mesmo depois dele sair da empresa.

- No detalhe do usuário, botão **Inativar**.
- AlertDialog: "Inativar Beatriz Brito? Ela não conseguirá mais fazer login. Tarefas em aberto serão mantidas."
- Confirma → status vira "Inativo".

### Tarefas pendentes

Se o usuário tinha tarefas atribuídas (elaborar documento, aprovar, executar ação corretiva), elas **continuam atribuídas** ao inativo.
- Sistema avisa o admin: "5 tarefas estão atribuídas a Beatriz. Reatribuir antes?".
- Você pode reatribuir agora ou deixar para depois (mas alguém vai precisar reatribuir antes que estoure prazo).

### Reativar

Mesmo botão, agora **Reativar**. Volta como Ativo, com o mesmo grupo e tarefas.

## Como exportar lista de usuários

Botão **Exportar** gera CSV/Excel com:
- Nome, E-mail, Grupo, Status, Data de cadastro, Último login.

Útil para auditoria interna ou backup.

## Permissão x grupo

Cada usuário tem **1 grupo**. Isso define o que ele pode fazer.

Para mudar o que ele pode fazer:
- Mudá-lo de grupo (rápido, afeta só ele).
- Editar o grupo dele (afeta todo mundo do grupo).

## Reset de senha (opcional, futuro)

O usuário também pode resetar a própria senha pelo link "Esqueci minha senha" na tela de login. O sistema envia um e-mail com link de redefinição.

## Notificações

| Evento | Quem é notificado |
|---|---|
| Usuário criado | Próprio usuário recebe e-mail de boas-vindas com link de login |
| Senha resetada pelo Admin | Próprio usuário recebe e-mail com nova senha temporária |
| Usuário inativado | Próprio usuário (caso esteja logado, é forçado o logout) |
| Mudança de grupo | Não notifica |

Audit log:
- `admin.user.created`
- `admin.user.updated`
- `admin.user.deactivated`
- `admin.user.reactivated`
- `admin.user.role_changed`
- `auth.password.reset_by_admin`

## Estados especiais

### E-mail duplicado
Bloqueado. Erro: "E-mail já em uso na sua empresa".

### Não tem grupo nenhum cadastrado ainda
Sistema vem com pelo menos "Admin *" e "Padrão". Se você apagou tudo, o cadastro de usuário fica bloqueado até criar um grupo.

### Senha fraca
Bloqueada. Mínimo: 8 caracteres com letras + números + símbolos.

## Exemplo Seven — primeiros usuários

Sequência sugerida:
1. **Beatriz Brito** (você) → grupo "Admin Seven".
2. **Backup admin** → outra pessoa com grupo "Admin Seven" (importante ter 2 admins, nunca só 1).
3. **Coord. Ambiental** → grupo "Coord. Ambiental Seven".
4. **Coord. Operacional** → grupo "Coord. Operacional".
5. **Encarregados de campo** → grupo "Encarregado Seven".
6. **Auditor externo** (se houver) → grupo "Auditor Externo".
