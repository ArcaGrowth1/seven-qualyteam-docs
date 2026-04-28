# Stack técnica

## Resumo

| Camada | Escolha | Por quê |
|---|---|---|
| Frontend | **Next.js 14+ (App Router)** + TypeScript | Mesma família do QualyTeam, ecossistema maduro, RSC quando útil |
| UI | **shadcn/ui + Tailwind** | Customizável, sem lock-in de design system |
| Estado client | **TanStack Query** + Zustand para UI local | Cache HTTP automático, devtools |
| Forms | **React Hook Form + Zod** | Validação shareable client/server |
| Backend | **Node 20+ + Fastify** ou **NestJS** | Performance, TypeScript first |
| ORM | **Drizzle** ou **Prisma** | Type-safe, migrations |
| DB | **PostgreSQL 16** | RLS para multi-tenancy, JSONB para campos personalizados |
| Cache/Queue | **Redis 7** + BullMQ | Filas para workers, sessions |
| Storage | **S3 / Cloudflare R2** | URLs assinadas para upload/download |
| PDF viewer | **PDF.js** embutido | Open source, sem download obrigatório |
| Auth | **Cookie httpOnly cross-subdomain** + **Lucia** ou **Auth.js** | SSO opcional via OIDC |
| E-mail | **Resend** ou **SES** | Templates React Email |
| Analytics | **PostHog** (self-hosted ou cloud) | Substitui Mixpanel + heatmap (Clarity) |
| Observabilidade | **Sentry** + **OpenTelemetry → Grafana Cloud** | Erros + métricas + traces |
| CI/CD | **GitHub Actions** + **Vercel** (front) + **Fly.io** ou **Railway** (back) | Simples e barato no início |
| IaC | **Terraform** (DB, storage, DNS) | Reproduzível |

## Decisões já tomadas

- **Next.js App Router**, não Pages — projeto greenfield, melhor DX para RSC e streaming.
- **Drizzle**, não Prisma, se quisermos `pg_dump`-friendly migrations e queries SQL-like.
- **Tailwind + shadcn/ui** ao invés de Material/Chakra para controle total do visual.

## Decisões em aberto (ADRs pendentes)

| Decisão | ADR | Status |
|---|---|---|
| Multi-tenancy: schema-per-tenant vs `tenant_id` em todas tabelas | [ADR-0001](../06-roadmap/adrs/0001-multi-tenancy.md) | Proposta |
| Monólito modular vs microsserviços | [ADR-0002](../06-roadmap/adrs/0002-monolith-vs-microservices.md) | Proposta |
| Próprio auth vs Clerk / Supabase Auth | (a criar) | — |

## Versões mínimas

```
node    >= 20.11
pnpm    >= 9
postgres >= 15
redis   >= 7
```

## Estrutura de monorepo proposta

```
seven-sgq/
├── apps/
│   ├── auth/                # Next.js — login
│   ├── admin/               # Next.js — administrador
│   ├── docs/                # Next.js — documentos
│   ├── nc/                  # Next.js — não conformidades
│   ├── opp/                 # Next.js — oportunidades
│   ├── risk/                # Next.js — riscos
│   └── api/                 # Backend único (monólito modular)
├── packages/
│   ├── ui/                  # Componentes compartilhados (shadcn)
│   ├── db/                  # Schema + migrations + queries
│   ├── auth/                # Lógica de sessão compartilhada
│   ├── notifications/       # Templates de e-mail + sender
│   └── tsconfig/            # Configs TS compartilhadas
├── docs/                    # ESTE REPO (ou submodule)
└── infra/                   # Terraform
```

**Gerenciador**: `pnpm` + `turborepo` para builds incrementais.
