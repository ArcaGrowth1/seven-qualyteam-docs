# 00 — Visão geral

## O que estamos construindo

Um SaaS de **Gestão da Qualidade e Ambiental** para a Seven Resíduos, focado em conformidade ISO 9001 / ISO 14001 / ISO 31000 e regulação ambiental brasileira (PNRS, NBR 10004, IBAMA, órgãos estaduais).

## Para quem

- **Cliente principal**: Seven Resíduos — gestora de resíduos de interesse ambiental.
- **Usuários internos**: Coordenação ambiental, auditores internos, operacional de campo, gerência.
- **Possível extensão futura**: white-label para outras gestoras de resíduos.

## Princípios de produto

1. **Modularidade** — cada módulo (Documentos, NC, Riscos…) deve poder ser ligado/desligado sem quebrar os outros.
2. **Multi-tenant desde o dia 1** — mesmo que comecemos com 1 cliente, todo o modelo de dados isola por `tenant_id`.
3. **Auditável** — toda ação relevante grava log; nada é deletado de verdade (soft delete).
4. **Multi-unidade** — clientes têm filiais/fábricas; tudo é dimensionável por unidade organizacional.
5. **Configurável > Customizável** — preferimos parâmetros e campos personalizados a forks de código.

## Módulos do MVP

Em ordem de prioridade:

1. **Administrador** — usuários, permissões, processos, unidades, campos personalizados (espinha dorsal)
2. **Documentos** — controle de licenças e procedimentos com validade
3. **Não conformidades** — ocorrências ambientais, RNCs, planos de ação
4. **Riscos** — riscos ambientais, matriz de apetite por unidade
5. **Oportunidades** — oportunidades de melhoria com matriz Probabilidade × Impacto

Detalhes em [`06-roadmap/mvp.md`](06-roadmap/mvp.md).

## Módulos futuros (fora do MVP)

- **MTR** — Manifesto de Transporte de Resíduos (entidade central do segmento)
- **Geradores / Transportadores / Destinadores** — cadastro de stakeholders
- **Inventário de resíduos** — classes I/II por NBR 10004
- **Auditorias** — internas e de fornecedores
- **Indicadores** — KPIs ambientais

## Inspiração funcional: QualyTeam

A análise inicial do domínio veio do **QualyTeam** (referência funcional, não de código). Capturamos o modelo de fluxos, permissões e arquitetura em [`02-domain/`](02-domain/) e nos diagramas de cada módulo. Nossa implementação é independente.

## Não-objetivos (não vamos fazer)

- Concorrer com ERPs financeiros — não fazemos contas a pagar/receber.
- Substituir sistemas de manutenção (CMMS) — fazemos calibração no contexto de qualidade, não gestão de ativos completa.
- Gerar relatórios fiscais oficiais (SISNAMA, IBAMA) — exportamos dados, mas a submissão fica fora do MVP.

## Glossário rápido

| Termo | Significado |
|---|---|
| **SGQ** | Sistema de Gestão da Qualidade |
| **NC** | Não Conformidade |
| **RNC** | Relatório de Não Conformidade (tratamento completo de uma NC) |
| **PA** | Plano de Ação |
| **MTR** | Manifesto de Transporte de Resíduos |
| **Apetite ao risco** | Nível de risco que a organização aceita reter |
| **Tenant** | Cliente / organização isolada no sistema |

Glossário completo em [`02-domain/glossary.md`](02-domain/glossary.md).
