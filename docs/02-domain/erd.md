# Modelo de Dados — ERD

## Visão geral

```mermaid
erDiagram
    TENANT ||--o{ USER : has
    TENANT ||--o{ ORG_UNIT : has
    TENANT ||--o{ PROCESS : has
    TENANT ||--o{ ROLE : has
    TENANT ||--o{ CUSTOM_FIELD : defines
    TENANT ||--o{ AUDIT_LOG : records

    USER }o--|| ROLE : member_of
    ROLE ||--o{ PERMISSION : grants

    USER ||--o{ NOTIFICATION : receives
```

## Espinha dorsal (Admin)

```mermaid
erDiagram
    TENANT {
        uuid id PK
        string name
        string slug "subdomínio"
        timestamp created_at
        boolean active
    }
    USER {
        uuid id PK
        uuid tenant_id FK
        string name
        string email "unique per tenant"
        string password_hash
        uuid role_id FK
        string locale "pt-BR, en-US…"
        boolean active
        timestamp last_login_at
    }
    ORG_UNIT {
        uuid id PK
        uuid tenant_id FK
        string code "sigla"
        string name
        boolean active
    }
    PROCESS {
        uuid id PK
        uuid tenant_id FK
        string code "sigla"
        string name
        boolean active
    }
    ROLE {
        uuid id PK
        uuid tenant_id FK
        string name
        boolean is_system "Admin* etc"
        json permissions "array de permission keys"
    }
    PERMISSION {
        string key PK "ex: docs.create"
        string module
        string description
        boolean essential "não pode ser desmarcada"
    }
    CUSTOM_FIELD {
        uuid id PK
        uuid tenant_id FK
        string module "docs|nc|opp|risk"
        string step "etapa do fluxo"
        string label
        string type "text|select|multiselect"
        boolean required
        json options "para select"
        int order
    }
    CUSTOM_FIELD_VALUE {
        uuid id PK
        uuid custom_field_id FK
        uuid entity_id "id do registro alvo"
        json value
    }
    AUDIT_LOG {
        uuid id PK
        uuid tenant_id FK
        uuid user_id FK
        string action
        string entity_type
        uuid entity_id
        json before
        json after
        timestamp created_at
        string ip_address
    }

    TENANT ||--o{ USER : has
    TENANT ||--o{ ORG_UNIT : has
    TENANT ||--o{ PROCESS : has
    TENANT ||--o{ ROLE : has
    USER }o--|| ROLE : assigned
    TENANT ||--o{ CUSTOM_FIELD : defines
    CUSTOM_FIELD ||--o{ CUSTOM_FIELD_VALUE : filled_in
```

## Documentos

```mermaid
erDiagram
    DOCUMENT {
        uuid id PK
        uuid tenant_id FK
        string kind "internal|external"
        string code "POP0001 etc"
        string title
        uuid category_id FK
        uuid process_id FK
        string status "draft|in_elaboration|in_approval|published|expired|inactive"
        date valid_until
        timestamp created_at
        boolean read_required
    }
    DOCUMENT_ORG_UNIT {
        uuid document_id FK
        uuid org_unit_id FK
    }
    DOCUMENT_CATEGORY {
        uuid id PK
        uuid tenant_id FK
        string name
    }
    DOCUMENT_REVISION {
        uuid id PK
        uuid document_id FK
        int revision_number
        uuid elaborated_by FK
        uuid approved_by FK
        timestamp elaborated_at
        timestamp approved_at
        timestamp published_at
        string file_url "S3 key"
        string status "draft|in_review|approved|published"
    }
    ATTACHMENT {
        uuid id PK
        uuid revision_id FK
        string filename
        string content_type
        int size_bytes
        string storage_key
    }
    READ_RECEIPT {
        uuid id PK
        uuid revision_id FK
        uuid user_id FK
        timestamp read_at
    }
    CONTROLLED_COPY {
        uuid id PK
        uuid revision_id FK
        uuid recipient_user_id FK "ou local físico"
        string recipient_label
        timestamp distributed_at
        timestamp revoked_at
    }

    DOCUMENT ||--o{ DOCUMENT_REVISION : has
    DOCUMENT }o--|| DOCUMENT_CATEGORY : categorized
    DOCUMENT ||--o{ DOCUMENT_ORG_UNIT : scoped
    DOCUMENT_REVISION ||--o{ ATTACHMENT : files
    DOCUMENT_REVISION ||--o{ READ_RECEIPT : tracks
    DOCUMENT_REVISION ||--o{ CONTROLLED_COPY : distributed
```

## Não Conformidades

```mermaid
erDiagram
    OCCURRENCE {
        uuid id PK
        uuid tenant_id FK
        text description
        date occurrence_date
        uuid process_id FK
        uuid origin_id FK
        uuid created_by FK
        string status "open|closed|escalated"
        timestamp created_at
    }
    OCCURRENCE_ORG_UNIT {
        uuid occurrence_id FK
        uuid org_unit_id FK
    }
    NC_ORIGIN {
        uuid id PK
        uuid tenant_id FK
        string name
    }
    IMMEDIATE_ACTION {
        uuid id PK
        uuid occurrence_id FK
        text description
        uuid responsible_id FK
        date due_date
        timestamp completed_at
        text evidence
    }
    NCR {
        uuid id PK
        uuid occurrence_id FK
        string code
        string current_step "cause|approval|implementation|effectiveness|closed"
        timestamp opened_at
        timestamp closed_at
    }
    ROOT_CAUSE_ANALYSIS {
        uuid id PK
        uuid ncr_id FK
        string method "5whys|ishikawa|other"
        json content "estrutura específica do método"
    }
    CORRECTIVE_ACTION {
        uuid id PK
        uuid ncr_id FK
        text description
        uuid responsible_id FK
        date planned_due_date
        date actual_completion_date
        string status "planned|approved|in_progress|done|cancelled"
    }
    EFFECTIVENESS_CHECK {
        uuid id PK
        uuid ncr_id FK
        date check_date
        boolean effective
        text evidence
        uuid checked_by FK
    }

    OCCURRENCE }o--|| NC_ORIGIN : from
    OCCURRENCE ||--o{ IMMEDIATE_ACTION : has
    OCCURRENCE ||--o| NCR : escalates_to
    NCR ||--|| ROOT_CAUSE_ANALYSIS : has
    NCR ||--o{ CORRECTIVE_ACTION : has
    NCR ||--o| EFFECTIVENESS_CHECK : verified_by
```

## Oportunidades

```mermaid
erDiagram
    OPP_MATRIX_CONFIG {
        uuid id PK
        uuid tenant_id FK
        int size "3, 4 ou 5"
        json axis_labels "{x: [...], y: [...]}"
        json quadrant_actions "matriz NxN com ações"
        int validation_days "default 30"
        boolean approval_required
    }
    OPPORTUNITY {
        uuid id PK
        uuid tenant_id FK
        text description
        uuid process_id FK
        uuid org_unit_id FK
        uuid created_by FK
        int probability_score "1..N"
        int impact_score "1..N"
        string action "implement|evaluate|archive"
        string status "draft|validated|in_action|approved|implemented|closed"
        date validated_at
    }
    OPP_ACTION_PLAN {
        uuid id PK
        uuid opportunity_id FK
        text description
        uuid responsible_id FK
        date due_date
        string status
    }

    OPPORTUNITY }o--|| OPP_MATRIX_CONFIG : evaluated_with
    OPPORTUNITY ||--o{ OPP_ACTION_PLAN : has
```

## Riscos

```mermaid
erDiagram
    RISK_UNIT_CONFIG {
        uuid id PK
        uuid tenant_id FK
        uuid org_unit_id FK
        uuid responsible_id FK
        int reassessment_days
        int matrix_size "3, 4 ou 5"
        json axis_labels
        json quadrant_actions "Reter|Mitigar|Evitar"
        boolean locked "após primeiro risco, trava modelo"
    }
    RISK {
        uuid id PK
        uuid tenant_id FK
        uuid unit_config_id FK
        text description
        string category
        int probability_score
        int impact_score
        string treatment "retain|mitigate|avoid|transfer"
        string status "identified|in_treatment|monitored|inactive"
        date identified_at
        date next_reassessment_at
    }
    RISK_TREATMENT {
        uuid id PK
        uuid risk_id FK
        text plan
        uuid responsible_id FK
        date due_date
        string status
    }
    RISK_REASSESSMENT {
        uuid id PK
        uuid risk_id FK
        int probability_score
        int impact_score
        text comments
        timestamp reassessed_at
        uuid reassessed_by FK
    }

    RISK }o--|| RISK_UNIT_CONFIG : governed_by
    RISK ||--o{ RISK_TREATMENT : has
    RISK ||--o{ RISK_REASSESSMENT : history
```

## Multi-tenancy

Toda tabela de domínio carrega `tenant_id`. Uso de **PostgreSQL Row-Level Security (RLS)** com a policy:

```sql
CREATE POLICY tenant_isolation ON <table>
  USING (tenant_id = current_setting('app.tenant_id')::uuid);
```

`app.tenant_id` é setado pela API a cada conexão, baseado no JWT/cookie do usuário.

Detalhes em [`02-domain/multi-tenancy.md`](multi-tenancy.md).
