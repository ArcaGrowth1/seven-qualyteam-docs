# Documentos — Tarefas → Validade

## Onde fica

`Documentos → Tarefas → aba Validade` (URL: `/tasks/expiration`)

## Quem acessa

Default: você. Outros: `docs.tasks.read_all`.

## O que é

Lista de **documentos com validade próxima ou já vencida** que você é responsável por revisar / renovar.

Tipos típicos de documentos com validade:
- **Externos**: Licenças ambientais (CETESB, IBAMA), certificados, contratos, normas técnicas com data.
- **Internos**: POPs e Manuais com periodicidade de revisão obrigatória (ex: revisar a cada 12 meses).

## Anatomia da tela

```
┌──────────────────────────────────────────────────────────────────────┐
│ Tarefas                                                               │
│ ┌─Elaboração─┬─Solicitação─┬─Validade ●─┐                            │
│                                                                       │
│ [chip: Beatriz Brito ✕]                              [Filtro] [Exportar]│
├──────────────────────────────────────────────────────────────────────┤
│ # │ Código  │ Documento     │ Tipo    │ Validade   │ Dias restantes │
│ 1 │ LIC-001 │ Lic. CETESB.. │ Externo │ 11/05/2026 │ 13 dias 🟡     │
│ 2 │ POP-002 │ Procediment.. │ Interno │ 28/04/2026 │ Vencido 🔴     │
│ 3 │ MAN-005 │ Manual de S.. │ Interno │ 15/06/2026 │ 48 dias 🟢     │
└──────────────────────────────────────────────────────────────────────┘
```

## Como a tarefa cai aqui

Automaticamente, todo dia o sistema roda um **cron** (job agendado) que verifica:
- Documentos com `valid_until < hoje + 30 dias` → coloca em "Vencendo".
- Documentos com `valid_until < hoje` → coloca em "Vencido".

Para cada um, gera (ou atualiza) uma tarefa de Validade atribuída ao **responsável do documento** (geralmente quem é Aprovador, ou o user definido no campo "Responsável pela validade").

Você recebe **e-mail** nos seguintes momentos:
- 30 dias antes: "Documento X vence em 30 dias".
- 7 dias antes: "Documento X vence em 7 dias".
- No dia que vence: "Documento X vence hoje".
- 1 dia depois: "Documento X venceu ontem".

## Cada coluna

| Coluna | O que mostra |
|---|---|
| **Código** | Código do documento |
| **Documento** | Título |
| **Tipo** | Interno / Externo |
| **Validade** | Data exata de vencimento |
| **Dias restantes** | Cálculo: validade − hoje. Com ícone colorido: 🟢 > 30d / 🟡 ≤ 30d / 🔴 vencido |

## Filtros disponíveis

| Filtro | O que filtra |
|---|---|
| **Status de validade** | Vencido / Vencendo em até 7d / Vencendo em até 30d / Sem validade |
| **Responsável** | Default = você |
| **Tipo** | Interno / Externo |
| **Unidade organizacional** | Documentos da unidade |
| **Período de vencimento** | Faixa de datas |

## Ações por linha

### Iniciar revisão / renovação
- Clica na linha → abre detalhe.
- No painel direito, botão **"Revisar"**.
- Para documento interno: abre formulário de **nova revisão** (incrementa número da revisão, vai para "Em elaboração").
- Para documento externo: abre formulário para **substituir o arquivo** + atualizar nova data de validade.

### Inativar (se não vai mais ser usado)
- Botão "Inativar" no painel direito.
- AlertDialog pedindo justificativa.
- Documento sai de circulação (não some — fica como "Inativo" para histórico).

### Reatribuir responsabilidade
- Botão "Reatribuir" se você não é mais o responsável.

## O que acontece se você ignora

⚠️ Se ninguém revisar a tempo:
- Documento fica com status **"Vencido"** (badge vermelho).
- Continua aparecendo na lista mestra com badge.
- Continua na sua aba Validade.
- Notificações continuam — semanais, depois mensais.
- Aparece em vermelho no Dashboard.
- Em auditoria interna, é não-conformidade automática.

> ❗ Para Seven Resíduos, **licenças vencidas** podem ter implicação **legal** (multa, embargo). Ter alarme aqui é crítico.

## Notificações

| Quando | Quem | Canal |
|---|---|---|
| 30 dias antes do vencimento | Responsável | E-mail + in-app |
| 7 dias antes | Responsável | E-mail + in-app |
| 1 dia antes | Responsável + Admin | E-mail + in-app |
| No dia 0 (vence hoje) | Responsável + Admin | E-mail + in-app |
| Após vencido (semanal) | Responsável + Admin + Gerência (configurável) | E-mail |

> Os prazos das notificações podem ser configurados por documento (campo "Notificar X dias antes").

## Permissões necessárias

| Ação | Permissão |
|---|---|
| Ver aba | (default) |
| Iniciar revisão (interno) | `docs.update` ou ser responsável designado |
| Substituir arquivo (externo) | `docs.update` ou `docs.published_file.replace` |
| Inativar | `docs.update` |
| Reatribuir | `docs.update` |

## Estados especiais

### Lista vazia
"Nenhum documento próximo de vencimento". 🎉

### Documentos sem validade configurada
Não aparecem aqui. Só aparece quem tem `valid_until` definido.

## Exemplo Seven

**Caso 1**: Licença ambiental CETESB de operação de aterro.
- Cadastrada como documento externo, com validade 11/05/2026.
- 11/04/2026 (30 dias antes): Beatriz recebe e-mail. Tarefa aparece como 🟡 Vencendo.
- Beatriz contata o consultor ambiental que cuida de renovações.
- Consultor renova. Beatriz pega o novo arquivo PDF.
- Beatriz acessa o detalhe da licença → Revisar → Faz upload do novo PDF + atualiza validade para 11/05/2027.
- Tarefa some da Validade. Documento volta a ser 🟢 Vigente.

**Caso 2**: POP de derramamento, com periodicidade de revisão de 12 meses.
- Última revisão: 28/04/2025 (rev 02).
- 28/03/2026: alerta 30 dias.
- Beatriz inicia revisão (botão Revisar).
- POP volta para "Em elaboração" (rev 03), cai na aba **Elaboração** do elaborador designado.
- Após aprovado, vira novamente **Publicado**, sai da Validade.
