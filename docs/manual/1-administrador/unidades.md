# Administrador — Unidades organizacionais

## Onde fica

`Administrador → Unidades organizacionais` (URL: `/organizational-units`)

## Quem acessa

- Cadastrar: `admin.org_units.create`.
- Editar: `admin.org_units.update`.
- Excluir: `admin.org_units.delete`.

## O que é "Unidade organizacional"

Uma **localização física ou jurídica** da empresa. Exemplos:
- Matriz
- Filial Amsterdã
- Fábrica Berlim
- Unidade de Tratamento de Resíduos São Paulo
- Aterro CETESB Caieiras

Diferente de **Processo** (que é área funcional), Unidade é **lugar**.

## O que aparece na tela

```
┌─────────────────────────────────────────────────────────────────┐
│ Unidades organizacionais        [+ Adicionar unidade] [Filtros] │
│ Total: 4                                                        │
├─────────────────────────────────────────────────────────────────┤
│ Sigla ↓ │ Nome ↓                  │ Data registro ↓ │ Editar  │
│ ─       │ Fábrica de Chennai      │ 27/04/2026      │   ✏     │
│ ─       │ Fábrica de Berlim       │ 27/04/2026      │   ✏     │
│ ─       │ Filial de Amsterdã      │ 27/04/2026      │   ✏     │
│ ─       │ Matriz                  │ 27/04/2026      │   ✏     │
└─────────────────────────────────────────────────────────────────┘
```

## Cada coluna

| Coluna | O que mostra |
|---|---|
| **Sigla** | Abreviação curta (ex: MTZ, FB, FA). Opcional |
| **Nome** | Nome completo. Obrigatório |
| **Data de registro** | Quando foi criada |
| **Editar** ✏ | Abre form para editar |

> Note que **não aparece** botão de inativar como em Processos. Unidade tem **excluir** mas só funciona se nada está vinculado a ela.

## Como criar uma Unidade

1. Clica em **+ Adicionar unidade**.
2. Form:
   - **Sigla** (opcional)
   - **Nome** *(obrigatório, max 100 caracteres)*
3. **Gravar**.
4. Aparece na lista.

## Como editar

- Ícone **✏** na linha → abre form preenchido → editar → Gravar.

## Como excluir

⚠️ Funciona **apenas se não houver nada vinculado** à unidade.

1. No detalhe da unidade (clica nela), botão **Excluir**.
2. AlertDialog: "Excluir unidade Y? Essa ação não pode ser desfeita."
3. Se houver vínculos (documentos, NCs, riscos, oportunidades, configurações de risco), o sistema **bloqueia** com mensagem: "Existem N registros vinculados. Migre-os antes de excluir."

## Onde "Unidade" aparece em outros módulos

- **Documentos**: campo obrigatório, multi-seleção. Um documento pode ser válido para várias unidades (ex: "POP Geral" vale para Matriz e Filial).
- **Não Conformidades**: ocorrências e RNCs são vinculadas a unidades onde aconteceram.
- **Oportunidades**: vinculada à unidade onde a melhoria se aplica.
- **Riscos**: ⭐ **caso especial** — Riscos é configurado **por unidade**, cada unidade tem sua própria matriz de apetite. Ver [Riscos](../5-riscos/visao-geral.md).
- **Filtros e Dashboards**: você filtra todos os módulos por unidade.

## Notificações

Não dispara notificações para usuários comuns.

Audit log:
- `admin.org_unit.created`
- `admin.org_unit.updated`
- `admin.org_unit.deleted`

## Exemplo Seven

Seven Resíduos provavelmente vai querer:
- **Matriz** (sede administrativa)
- **CT Caieiras** (centro de tratamento)
- **Aterro Sanitário X** (se a Seven opera)
- **Base operacional Norte / Sul** (depósitos de caminhões)
- **Cliente A — Unidade interna** (se houver gestão in-loco em cliente grande)

Cada unidade vai ter seu próprio conjunto de licenças (Documentos), suas próprias ocorrências (NCs) e sua própria matriz de risco (Riscos).
