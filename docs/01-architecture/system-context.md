# Arquitetura — Contexto do sistema (C4 nível 1)

Visão de "30 mil pés": quem usa, com quê o sistema conversa.

```mermaid
flowchart LR
    User([👤 Usuário SGQ<br/>Coord ambiental, auditor,<br/>operacional, gerência])
    Admin([👤 Admin do tenant])

    Sys[(Seven SGQ)]

    Email[📧 Provedor de e-mail<br/>SES / Resend]
    Storage[(💾 Object storage<br/>S3 / R2)]
    Analytics[📊 Analytics<br/>PostHog]
    Auth[🔐 SSO / OAuth<br/>Google, Microsoft<br/>opcional]

    User -->|usa via navegador| Sys
    Admin -->|configura| Sys
    Sys -->|envia notificações| Email
    Sys -->|guarda anexos / PDFs| Storage
    Sys -->|telemetria / heatmaps| Analytics
    Sys -.->|login federado opcional| Auth
```

## Atores

| Ator | Necessidades principais |
|---|---|
| **Coord. ambiental** | Cadastrar licenças, registrar ocorrências, gerar evidências |
| **Auditor interno** | Acessar histórico, exportar relatórios |
| **Operacional de campo** | Registrar ocorrências rápido (mobile-friendly) |
| **Gerência** | Dashboards, indicadores, KPIs |
| **Admin do tenant** | Cadastrar usuários, configurar processos/unidades, definir permissões |

## Sistemas externos

| Sistema | Tipo | Por quê |
|---|---|---|
| Provedor de e-mail | Saída | Notificações de tarefas, prazos, validade |
| Object storage | Saída/entrada | Anexos, PDFs publicados |
| PostHog | Saída | Telemetria de produto e session replay |
| SSO opcional | Entrada | Login federado para tenants enterprise |

## O que **não** está no escopo

- Integrações com SISNAMA / IBAMA / órgãos estaduais (futuro)
- Pagamentos / billing (módulo separado, fora deste sistema)
- Mobile nativo (mobile-first responsivo é suficiente para MVP)
