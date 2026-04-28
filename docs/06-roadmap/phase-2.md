# Roadmap — Fase 2

Após MVP estável (3-6 meses pós-launch).

## Módulos novos (vertical de resíduos)

### MTR — Manifesto de Transporte de Resíduos

Entidade central do segmento. Suporta integração com SINIR (federal) e sistemas estaduais (CETESB, FEAM, etc.).

- Cadastro de **Geradores / Transportadores / Destinadores** (similar ao módulo Fornecedores do QualyTeam)
- Emissão de MTR (numeração sequencial)
- Vinculação com licenças do módulo Documentos
- Status: emitido → em transporte → recebido → CDF emitido
- Exportação em PDF padrão

### Inventário de Resíduos

- Cadastro de tipos de resíduo (NBR 10004 — Classes I, IIA, IIB)
- Geração mensal por unidade (kg / litros / unidades)
- Composição do resíduo (% reciclável, % perigoso)
- Histórico para indicadores ambientais

### Auditorias

- Programa anual (planejamento)
- Execução com checklist personalizável
- Achados (vinculam a NCs)
- Relatório final
- Suporta auditorias internas e em fornecedores

### Indicadores / KPIs

- Cadastro de indicador (nome, unidade, fonte de dado, meta)
- Coleta periódica (manual ou automática quando há fonte)
- Gráficos comparando real × meta
- Alertas de desvio

### Calibrações (se relevante para Seven)

- Cadastro de equipamentos
- Periodicidade de calibração
- Registro de calibração (com certificado)
- Notificação de vencimento

## Melhorias cross-cutting

### SSO / OIDC
- Google Workspace, Microsoft Entra
- Login federado para usuários enterprise

### Mobile (PWA primeiro, nativo depois)
- App PWA com offline para registro de ocorrências em campo
- Foto rápida + GPS + timestamp
- Sincronização ao reconectar

### Workflows configuráveis
- Editor visual para customizar etapas dos fluxos (NC, Documentos)
- Permite adaptação a outras certificações (IATF, ISO 45001…)

### Integrações
- API pública (REST) para integração com ERP do cliente
- Webhooks de eventos (NC criada, Doc publicado…)
- Zapier / n8n

### Multi-language
- en-US e es-ES com tradução completa
- Suporte a tenants em outros idiomas

### White-label
- Customização de logo, cores, domínio por tenant
- Export de "marca branca" para gestoras de resíduos

## Melhorias técnicas

- **Search global** com Elasticsearch / Meilisearch
- **Real-time collab** (Yjs) para edição simultânea
- **Cache Redis** mais agressivo em consultas pesadas
- **Read replicas** PostgreSQL para dashboards
- **Particionamento** de tabelas grandes (audit_log, notifications)
- **Observabilidade**: traces distribuídos com OpenTelemetry

## Saúde do negócio

- Billing (Stripe) — para virar produto comercial multi-cliente
- Self-service onboarding
- Trial de 14 dias
- Plano por número de usuários ou módulos
