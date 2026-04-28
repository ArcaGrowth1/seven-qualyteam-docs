# Como funcionam as notificações

Você é notificado de coisas que precisa fazer ou saber. **3 canais**:

| Canal | Quando aparece | Exemplo |
|---|---|---|
| **Sino 🔔 (in-app)** | Sempre. Aparece no canto superior direito | Você foi designado elaborador de um documento |
| **E-mail** | Eventos importantes ou conforme suas preferências | Mesmo evento + lembrete de prazo |
| **Aba Tarefas** do módulo | Sempre. Trabalho a fazer | Lista das tarefas pendentes para você |

## ⚠️ Atenção: o sino não é o mesmo que Tarefas

| Sino 🔔 (canto superior) | Aba **Tarefas** (dentro de cada módulo) |
|---|---|
| Mostra **novidades do produto** (changelog do sistema) | Mostra **trabalho seu a fazer** |
| Itens são gerais (todos da empresa veem o mesmo) | Itens são pessoais (só você vê os seus) |
| Conteúdo: "Documentos: novas notificações de validade" | Conteúdo: "POP-042 esperando sua aprovação" |

> No QualyTeam, o sino mostra **release notes** do produto. Suas tarefas pessoais ficam na aba **Tarefas** do módulo correspondente.

## Sino 🔔 — em detalhe

Ao clicar:
```
┌─────────────────────────────────────────┐
│ Novidades no sistema                  ✕ │
├─────────────────────────────────────────┤
│ 📣 Novidade        [Não conformidades]  │
│ 🔵 NÃO CONFORMIDADES — Supervisão da     │
│    ocorrência                            │
│              [ Mais detalhes ]           │
├─────────────────────────────────────────┤
│ 📣 Novidade            [Documentos]      │
│ 🔵 DOCUMENTOS — Notificações automáticas │
│    de validade para documentos externos  │
│              [ Mais detalhes ]           │
└─────────────────────────────────────────┘
```

- 🔵 = não lida.
- "Mais detalhes" abre página externa com explicação completa da feature.
- Número no badge azul = quantas novidades não vistas.

## Aba Tarefas — em detalhe

Cada módulo tem sua própria. As tarefas correspondem a **etapas do fluxo do módulo**:

### Documentos → Tarefas
- **Elaboração**: documentos esperando você elaborar (anexar arquivo, escrever conteúdo).
- **Solicitação**: solicitações de novos documentos onde você é responsável.
- **Validade**: documentos com validade vencendo / vencidos onde você é responsável.

### Não Conformidades → Tarefas
- **Ocorrências**: ocorrências em aberto onde você é responsável.
- **Ações imediatas**: ações imediatas atribuídas a você.
- **Causa e planejamento**: RNCs onde você precisa fazer análise de causa.
- **Aprovação**: planos esperando sua aprovação.
- **Implementação**: ações corretivas que você precisa executar.
- **Eficácia**: verificações de eficácia esperando você.

### Oportunidades → Tarefas
- **Validação**: oportunidades esperando sua avaliação.

### Riscos → Tarefas
- **Riscos em tratamento**: planos de mitigação seus em andamento.
- **Riscos para reavaliação**: riscos que chegaram na data de reavaliação.

> Todas as listas de Tarefas vêm **filtradas pelo seu nome** por padrão. Para ver tarefas dos outros (se você for gerente, por exemplo), remova o chip do filtro de responsável — exige permissão `<modulo>.tasks.read_all`.

## E-mail

Você recebe e-mail nos seguintes eventos (configuráveis):

### Documentos
| Evento | E-mail vai para |
|---|---|
| Você foi designado elaborador | Você |
| Você foi designado aprovador | Você |
| Documento aprovado e publicado | Lista de leitura obrigatória |
| Documento próximo do vencimento (30 dias antes) | Responsável |
| Documento vencido | Responsável + Admin |

### Não Conformidades
| Evento | E-mail vai para |
|---|---|
| Ocorrência registrada (com você na lista de notificados) | Você |
| Ação imediata atribuída a você | Você |
| Sua ocorrência foi escalada para RNC | Você (criador) |
| Plano enviado para sua aprovação | Aprovador |
| Plano aprovado | Responsáveis das ações |
| Ação corretiva com prazo < 3 dias | Responsável |
| Hora de verificar eficácia | Verificador designado |

### Oportunidades
| Evento | E-mail vai para |
|---|---|
| Oportunidade nova esperando validação | Validador (coord. qualidade) |
| Oportunidade priorizada (matriz cai em "Implementar") | Responsável da ação |
| Oportunidade encerrada | Solicitante original |

### Riscos
| Evento | E-mail vai para |
|---|---|
| Novo risco identificado | Responsável da unidade |
| Plano de tratamento próximo do prazo | Responsável do tratamento |
| Hora de reavaliar risco | Responsável da unidade |
| Risco piorou após reavaliação | Responsável + Admin |

## Notificar manualmente (campo "Notificar outros")

Em alguns cadastros (Registrar ocorrência, por exemplo) tem checkbox:
```
☐ Notificar outros usuários por e-mail
```

Marcar abre seletor:
```
[ + Adicionar pessoa ]
[ chip: João Silva ✕ ]   [ chip: Maria Souza ✕ ]
```

Esses usuários recebem **um único e-mail** quando o registro é criado, com link para abrir.
- Útil para envolver gerência ou pessoa do processo afetado.
- Notificados **não viram responsáveis** automaticamente — só recebem ciência.

## Preferências do usuário (futuro)

Em uma versão posterior, você poderá configurar:
- Por módulo: quero ou não quero e-mails desse módulo.
- Frequência: imediato / digest diário / digest semanal.
- Apenas in-app, sem e-mail.

No MVP, todos os e-mails listados acima são enviados **imediatamente** quando o evento acontece.

## Anti-spam / rate limit

- Máximo **5 e-mails do mesmo tipo** por usuário em 5 minutos.
- Se você for responsável de muita coisa, o sistema não vai te encher de e-mail no mesmo segundo.
- Em vez disso, eventuais excessos viram um "digest" curto único.

## Onde fica registrado

Toda notificação enviada por e-mail grava no **audit log** (sem o conteúdo do e-mail, só o ID e timestamp). Útil em auditoria para responder "fulano foi avisado disso?".
