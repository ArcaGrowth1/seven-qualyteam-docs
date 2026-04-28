# Administrador — Registros de atividades (Audit Log)

## Onde fica

Atalho pela engrenagem ⚙ no canto superior direito → "Registros de atividades" (URL: `/audit-log`)

## Quem acessa

`admin.audit.read`. Geralmente: Admin do tenant + Auditor.

## O que é

Histórico **imutável** de tudo que aconteceu no sistema. Ninguém pode editar nem apagar — só consultar.

Útil para:
- Auditoria ISO ("quem aprovou esse documento?")
- Investigação de incidente ("quem inativou aquela RNC?")
- Compliance (LGPD: "quem acessou o dado pessoal X?")
- Debug ("por que esse risco mudou de quadrante?")

## A tela

```
┌──────────────────────────────────────────────────────────────────────┐
│ Registros de atividades                          [Filtros] [Exportar]│
│ Total: 1.342                                                          │
├──────────────────────────────────────────────────────────────────────┤
│ Data ↓             │ Usuário        │ Ação              │ Entidade  │
│ 28/04/2026 14:32  │ Beatriz Brito  │ docs.published    │ POP-001   │
│ 28/04/2026 14:30  │ Beatriz Brito  │ docs.approved     │ POP-001   │
│ 28/04/2026 11:15  │ João Silva     │ nc.occurrence...  │ OC-042    │
│ 28/04/2026 09:02  │ (sistema)      │ docs.expired      │ POP-005   │
└──────────────────────────────────────────────────────────────────────┘
```

## Cada coluna

| Coluna | O que mostra |
|---|---|
| **Data** | Data e hora exatas (sua timezone) |
| **Usuário** | Quem fez a ação. "(sistema)" se foi o cron / processo automático |
| **Ação** | Código da ação (ex: `docs.published`, `nc.rnc.escalated`) |
| **Entidade** | Sobre qual registro (ex: documento POP-001, RNC #018) |

## Filtros disponíveis

| Filtro | O que filtra |
|---|---|
| **Período** | Data inicial e final (default: últimos 30 dias) |
| **Usuário** | Filtra ações de uma pessoa |
| **Tipo de entidade** | Documento / NC / Risco / Oportunidade / Usuário / Configuração |
| **ID da entidade** | Se você quer histórico de um registro específico (ex: tudo que aconteceu com a RNC #018) |
| **Ação** | Autocomplete dos códigos disponíveis (ex: `docs.published`) |

## Detalhe de um registro

Clicar em uma linha **expande** mostrando o diff:

```
┌────────────────────────────────────────────────────────────────┐
│ docs.published — POP-001                                        │
│ Quem: Beatriz Brito (meioambiente1@sevenresiduos.com.br)       │
│ Quando: 28/04/2026 14:32:18 (BRT)                              │
│ De onde: 200.158.42.13 (Chrome 124, Windows)                   │
│                                                                 │
│ Antes:                                                          │
│   status: "in_approval"                                         │
│   published_at: null                                            │
│                                                                 │
│ Depois:                                                         │
│   status: "published"                                           │
│   published_at: "2026-04-28T17:32:18Z"                          │
└────────────────────────────────────────────────────────────────┘
```

- **Antes / Depois**: o que mudou no registro.
- **IP / Navegador**: ajuda investigar acessos suspeitos.

## O que vai pro audit log

Sempre:
- Login / logout / reset de senha
- Criação / edição / exclusão de **qualquer** registro de domínio
- Aprovações, publicações, encerramentos
- Mudanças em permissões / grupos / usuários
- Configurações de módulo (matriz de risco, prazos, origens)
- Distribuição/revogação de cópias controladas
- Anexos: upload, download, exclusão

Nunca:
- Cliques de UI, navegação, abertura de telas (geraria ruído).
- Mudanças de filtros e ordenação.
- Eventos de tracking / analytics (esses vão pro PostHog separado).

## Exportar

Botão **Exportar** gera CSV com todos os registros do filtro atual. Formato:
```
data,usuario,acao,entidade_tipo,entidade_id,antes,depois,ip
```

> ⚠️ Pode ser um arquivo grande. Use filtros antes (ex: período de 1 mês).

## Retenção

- Padrão: **5 anos** (requisito ISO).
- Após 1 ano, registros mais antigos vão para "storage frio" (ainda consultáveis, mas com 2-3s de delay).
- Não dá para apagar.

## Audit log é à prova de manipulação?

Quase. Tecnicamente:
- Linhas só são **inseridas**, nunca atualizadas/excluídas.
- A inserção é **na mesma transação** da mudança que registra. Se alguém burlasse o code, ainda assim o sistema tem RLS no DB que protege.
- Backup diário do DB cobre tudo.
- Quem tem acesso direto ao banco (super admin, raríssimo) tecnicamente poderia falsificar — esse é um risco aceitável para um SaaS comum (não é nível "criptografia em blockchain").

## Casos práticos de uso

### "Documento foi publicado errado, quem fez?"
1. Filtra por entidade_tipo = Documento.
2. Filtra por id da entidade = POP-001.
3. Vê todo o histórico: criado, elaborado, aprovado, publicado.
4. Identifica o usuário em cada etapa.

### "Sumiram dados de uma RNC, o que aconteceu?"
1. Filtra por entidade_tipo = RNC, id = RNC-018.
2. Procura por ação `nc.rnc.deleted` ou `nc.rnc.updated` recente.
3. Vê o diff "antes/depois".

### "Auditoria ISO me pergunta como aprovamos doc X em data Y"
1. Filtra por período = data Y, ação `docs.published`.
2. Identifica documento, usuário, hora exata.
3. Exporta CSV como evidência.

## Estados especiais

### Lista enorme
Use filtros! Sem filtro pode dar timeout.

### Sem permissão
Página "endereço errado" (mesma de 404 — não vaza existência).
