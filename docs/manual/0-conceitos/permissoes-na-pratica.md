# Permissões na prática

## Como sei o que posso fazer

3 sinais visuais simples:

1. **Item de menu não aparece**: você não tem nem permissão de leitura → o módulo/aba/atalho fica invisível.
2. **Botão "+ Novo" / "Editar" / etc. não aparece**: você lê mas não cria/edita.
3. **Página "endereço errado"**: você tentou acessar via URL direta uma área que não pode → mostra como se fosse 404 (por segurança, não diz "acesso negado" para não vazar a existência da página).

## Onde as permissões são definidas

No **Administrador → Grupos de permissões**.
- Você pertence a **um grupo**.
- O grupo é uma **lista de permissões marcadas**.

Apenas quem tem permissão de Admin pode mudar grupos ou trocar você de grupo.

## Permissões essenciais (não desmarcáveis)

Algumas permissões são **obrigatórias** para o sistema funcionar. Aparecem com checkbox cinza (disabled). Exemplo: ver lista de usuários, processos, unidades.

> Mesmo o grupo "Admin *" tem permissões essenciais — elas existem para que ninguém crie um grupo "Admin sem permissão de ver nada" por engano.

## Permissões organizadas por módulo

Os grupos são lidos como árvore:

```
[Administrador]
  ☑ Ver lista de usuários
  ☑ Adicionar usuários
  ☑ ...

[Documentos]
  ☑ Cadastrar documento interno
  ☑ Editar documentos
  ☑ Ver dashboard
  ☐ Alterar arquivo publicado     ← se desmarcado, não pode trocar PDF de doc já publicado
  ...

[Não conformidades]
  ☑ Registrar ocorrência
  ☐ Excluir RNC                    ← perigoso, raramente liberado
  ...
```

## Permissões por persona — sugestão

Modelo recomendado de grupos (você adapta na sua empresa):

### Admin do tenant
- Tudo marcado.
- 1 ou 2 pessoas no máximo.

### Coord. Qualidade / Ambiental
- **Admin**: Ver tudo. Não pode criar usuários (deixa pro Admin).
- **Documentos**: Tudo marcado.
- **NC**: Tudo marcado, exceto "Excluir RNC".
- **Oportunidades**: Tudo marcado.
- **Riscos**: Tudo marcado, exceto "Excluir riscos".

### Auditor interno
- **Admin**: Apenas "Ver registros de atividades".
- Cada módulo: apenas as permissões "Consultar" e "Exportar". Sem criar/editar/excluir.

### Operacional de campo
- **Documentos**: Apenas Consultar (precisa ver POPs).
- **NC**: "Registrar ocorrência" + "Editar e excluir ocorrência" (suas).
- Sem acesso a Riscos, Oportunidades, Admin.

### Aprovador (gerência)
- Combinação dos papéis acima conforme escopo de gestão.
- Especialmente: "Validar oportunidades", "Editar configuração do módulo" (se aplicável).

## Permissão x designação como responsável

São coisas diferentes:

| | Permissão | Designação |
|---|---|---|
| **O que é** | Capacidade técnica de fazer | Ser escolhido para fazer um item específico |
| **Onde define** | Grupo de permissões (Admin) | No próprio cadastro do item |
| **Exemplo** | "Pode aprovar documentos" | "É aprovador deste documento POP-001" |

> Mesmo tendo permissão "Aprovar documentos", você só vê na sua aba **Tarefas → Aprovação** os documentos onde **você foi designado** como aprovador. Para ver de outros, precisa de `docs.tasks.read_all`.

## Diferença "Editar X" vs "Editar X aberta"

Algumas permissões têm essa nuance:
- **Editar RNC aberta**: pode editar enquanto está em fluxo (Causa, Aprovação, Implementação…).
- **(implícito)** Após encerrada, ninguém edita — preserva auditoria.

Para riscos:
- **Editar riscos concluídos**: precisa de permissão extra para mexer em risco já encerrado (raro, geralmente para correção).

## Quem pode mudar permissões

Apenas quem tem:
- `admin.roles.update` → editar grupos existentes.
- `admin.roles.create` → criar novos grupos.
- `admin.roles.delete` → excluir grupos.

Mudar **alguém de grupo** exige `admin.users.update`.

## Auditoria de permissões

Toda mudança em permissões e grupos vai pro audit log:
- "Beatriz Brito alterou o grupo 'Coord. Qualidade': adicionou permissão `nc.rnc.delete`"
- Útil para responder em auditoria: "Como esse cara conseguiu fazer isso?".

Você acessa em **Administrador → Registros de atividades** (precisa de `admin.audit.read`).

## Perdi acesso, e agora?

Se você não consegue mais acessar algo que conseguia antes:
1. Verifique se você ainda está logado (avatar no canto).
2. Provavelmente o admin trocou seu grupo. Pergunte para ele.
3. Olhe o audit log: lá fica registrado quem mudou seu grupo e quando.

## Lista completa de permissões

Ver [Permissões — referência completa](../../02-domain/permissions-matrix.md) (área técnica).
