# Matriz de permissões

Cada permissão tem uma `key` (`<modulo>.<acao>`) e pode ser **essencial** (não removível do grupo Admin).

## Administrador

| Key | Descrição | Essencial |
|---|---|---|
| `admin.users.read` | Ver lista de usuários | sim |
| `admin.users.create` | Adicionar usuários | sim |
| `admin.users.update` | Editar usuários | sim |
| `admin.users.deactivate` | Inativar usuários | sim |
| `admin.roles.read` | Ver lista de grupos de permissões | sim |
| `admin.roles.create` | Adicionar grupos | sim |
| `admin.roles.update` | Editar grupos | sim |
| `admin.roles.delete` | Excluir grupos | sim |
| `admin.org_units.create` | Cadastrar unidades organizacionais | sim |
| `admin.org_units.update` | Editar unidades | sim |
| `admin.org_units.delete` | Excluir unidades | sim |
| `admin.processes.create` | Cadastrar processos | sim |
| `admin.processes.update` | Editar processos | sim |
| `admin.processes.toggle` | Ativar/inativar processos | sim |
| `admin.audit.read` | Ver registros de atividades | sim |
| `admin.custom_fields.create` | Cadastrar campos personalizados | sim |
| `admin.custom_fields.update` | Editar campos personalizados | sim |

## Documentos

| Key | Descrição |
|---|---|
| `docs.internal.create` | Cadastrar documento interno |
| `docs.external.create` | Cadastrar documento externo |
| `docs.delete` | Deletar documentos |
| `docs.update` | Editar documentos |
| `docs.read` | Consultar documentos |
| `docs.export` | Exportar consulta de documentos |
| `docs.tasks.read_all` | Ver tarefa de todos (não só do próprio user) |
| `docs.controlled_copy.delete` | Excluir cópia controlada distribuída |
| `docs.read_required.manage` | Controle de leitura obrigatória |
| `docs.audit.read` | Ver registros de atividades |
| `docs.dashboard.read` | Ver dashboard |
| `docs.published_file.replace` | Alterar arquivo publicado |

## Não Conformidades

| Key | Descrição |
|---|---|
| `nc.rnc.create` | Registrar RNC |
| `nc.rnc.delete` | Excluir RNC |
| `nc.tasks.read_all` | Ver tarefa de todos |
| `nc.rnc.read` | Consultar RNC |
| `nc.rnc.export` | Exportar lista de RNC |
| `nc.rnc.update_open` | Editar RNC aberta |
| `nc.occurrence.create` | Registrar ocorrência |
| `nc.occurrence.update_delete` | Editar e excluir ocorrência |
| `nc.occurrence.close` | Encerrar ocorrência |
| `nc.config.update` | Editar configuração do módulo |
| `nc.dashboard.read` | Ver dashboard |
| `nc.origin.create` | Adicionar origem |

## Oportunidades

| Key | Descrição |
|---|---|
| `opp.create` | Criar oportunidades de melhoria |
| `opp.update` | Editar oportunidades |
| `opp.read` | Consultar oportunidades |
| `opp.tasks.read_all` | Ver tarefa de todos |
| `opp.validate` | Validar oportunidades |
| `opp.config.update` | Editar configuração do módulo |

## Riscos

| Key | Descrição |
|---|---|
| `risk.unit_config.create` | Configurar unidade organizacional |
| `risk.unit_config.update` | Editar configuração |
| `risk.unit_config.delete` | Excluir configuração |
| `risk.dashboard.read` | Ver dashboard |
| `risk.toggle` | Inativar/reativar riscos |
| `risk.update_completed` | Editar riscos concluídos |
| `risk.delete` | Excluir riscos |

## Como o backend valida

Pseudocódigo:

```ts
// middleware
async function authorize(req, requiredKey: string) {
  const user = await loadUser(req.cookies.session);
  const role = await loadRole(user.role_id);
  if (!role.permissions.includes(requiredKey)) {
    throw new ForbiddenError(requiredKey);
  }
  req.user = user;
}

// uso
app.post('/api/docs',
  authorize('docs.internal.create'),
  handler
);
```

## Visão por persona (sugerida)

| Permissão | Admin | Coord. Qual. | Auditor | Operacional |
|---|:---:|:---:|:---:|:---:|
| Cadastrar documento | ✅ | ✅ | ❌ | ❌ |
| Aprovar documento | ✅ | ✅ | ❌ | ❌ |
| Registrar ocorrência | ✅ | ✅ | ✅ | ✅ |
| Tratar RNC | ✅ | ✅ | ❌ | ❌ |
| Ver dashboards | ✅ | ✅ | ✅ | ❌ |
| Configurar módulos | ✅ | ❌ | ❌ | ❌ |
| Ver audit log | ✅ | ❌ | ✅ | ❌ |
