# Arquitetura — Containers (C4 nível 2)

Decomposição em processos/serviços implantáveis.

## Diagrama

```mermaid
flowchart TB
    User([👤 Usuário])

    subgraph Browser["Navegador"]
        AuthApp[/🔐 auth.seven.app<br/>Next.js — login/]
        AdminApp[/⚙ admin.seven.app<br/>Next.js/]
        DocsApp[/📄 docs.seven.app<br/>Next.js/]
        NCApp[/⚠ nc.seven.app<br/>Next.js/]
        OppApp[/💡 opp.seven.app<br/>Next.js/]
        RiskApp[/🎯 risk.seven.app<br/>Next.js/]
    end

    subgraph Edge["Edge / CDN"]
        CDN[Cloudflare / Vercel Edge]
    end

    subgraph API["APIs (REST)"]
        Gateway[API Gateway<br/>auth + rate limit]
        AuthAPI[auth-api]
        AdminAPI[admin-api]
        DocsAPI[docs-api]
        NCAPI[nc-api]
        OppAPI[opp-api]
        RiskAPI[risk-api]
    end

    subgraph Workers["Background workers"]
        EmailWorker[email-worker]
        ExpiryWorker[expiry-watcher<br/>licenças, validades]
        ReassessWorker[risk-reassessment<br/>scheduler]
    end

    subgraph Data["Persistência"]
        DB[(🐘 PostgreSQL<br/>multi-tenant)]
        Redis[(⚡ Redis<br/>cache, queues, sessions)]
        S3[(📦 S3 / R2<br/>anexos)]
    end

    User --> CDN
    CDN --> AuthApp & AdminApp & DocsApp & NCApp & OppApp & RiskApp

    AuthApp -.cookie httpOnly<br/>Domain=.seven.app.-> AdminApp
    AuthApp -.-> DocsApp & NCApp & OppApp & RiskApp

    AuthApp & AdminApp & DocsApp & NCApp & OppApp & RiskApp --> Gateway
    Gateway --> AuthAPI & AdminAPI & DocsAPI & NCAPI & OppAPI & RiskAPI

    AuthAPI & AdminAPI & DocsAPI & NCAPI & OppAPI & RiskAPI --> DB
    DocsAPI & NCAPI --> S3
    AuthAPI --> Redis

    DocsAPI & NCAPI & OppAPI & RiskAPI -. enfileira .-> Redis
    Redis -. consome .-> EmailWorker
    ExpiryWorker --> DB
    ReassessWorker --> DB
    EmailWorker -.-> Email[(📧 SES/Resend)]
```

## Por que separar por subdomínio (e não rotear tudo em `/admin`, `/docs`)

| Vantagem | Detalhe |
|---|---|
| **Isolamento de bundle** | Cada app carrega só seus chunks. Trocar UI do módulo Riscos não invalida cache do Documentos. |
| **Deploy independente** | Time pode dar deploy de NC sem mexer em Documentos. |
| **CSP/Cookies por app** | Política de segurança específica por área. |
| **White-label** | Mais fácil ter `nc.cliente1.com` + `nc.cliente2.com` futuramente. |

## Decisão pendente: microsserviços vs monólito modular

Ver [ADR-0002](../06-roadmap/adrs/0002-monolith-vs-microservices.md). Recomendação atual: **monólito modular** (uma API com pastas por módulo) até ter > 5 devs no time. Apenas o **frontend** já nasce dividido por subdomínio.

## Componentes-chave

| Componente | Responsabilidade |
|---|---|
| **API Gateway** | Valida cookie de sessão, injeta `tenant_id` e `user_id` no request, faz rate limit |
| **email-worker** | Processa fila de notificações (NC notifica responsáveis, Doc notifica vencimento) |
| **expiry-watcher** | Cron diário: marca licenças/documentos vencidos, dispara notificação |
| **risk-reassessment** | Cron diário: marca riscos que precisam reavaliação conforme período da unidade |
