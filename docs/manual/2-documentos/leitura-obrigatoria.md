# Documentos — Controle de leitura obrigatória

## Onde fica

`Documentos → Consulta → Documentos internos → botão "Controle de leitura obrigatória"` no canto superior direito.

URL: `/search/internal-documents` (e botão abre tela dedicada).

## Quem acessa

`docs.read_required.manage`. Geralmente: Coord. Qualidade, Coord. Setoriais.

## O que é

Funcionalidade que **obriga determinados usuários a confirmarem leitura** de um documento.

Casos típicos:
- Toda equipe da operação precisa confirmar leitura de um POP novo.
- Toda gerência precisa ler nova versão da Política da Qualidade.
- Equipe ambiental precisa ler protocolo de derramamento atualizado.

Diferente de "Visualizações" (qualquer abertura do PDF é registrada). Aqui é confirmação **explícita**: a pessoa precisa abrir e clicar em "Confirmo que li".

## A tela

```
┌──────────────────────────────────────────────────────────────────────┐
│ Controle de leitura obrigatória                                       │
│                                                                       │
│ Filtros: [Documento ▾]  [Usuário ▾]  [Status: Pendente / Lido / Todos]│
├──────────────────────────────────────────────────────────────────────┤
│ Documento │ Usuário        │ Atribuído em │ Lido em      │ Status   │
│ POP-007   │ Carlos F.      │ 28/04/2026   │ 28/04/2026   │ Lido ✓   │
│ POP-007   │ João Silva     │ 28/04/2026   │ —            │ Pendente │
│ POP-007   │ Maria Souza    │ 28/04/2026   │ 30/04/2026   │ Lido ✓   │
│ MAN-002   │ Carlos F.      │ 22/04/2026   │ —            │ Pendente │
│ MAN-002   │ Beatriz Brito  │ 22/04/2026   │ 22/04/2026   │ Lido ✓   │
└──────────────────────────────────────────────────────────────────────┘
```

## Cada coluna

| Coluna | O que mostra |
|---|---|
| **Documento** | Código + título do doc (clicável → abre detalhe) |
| **Usuário** | Quem precisa ler (clicável → abre perfil) |
| **Atribuído em** | Data em que a obrigação foi criada |
| **Lido em** | Data da confirmação. Vazio = ainda não leu |
| **Status** | "Pendente" 🟡 ou "Lido" ✅ |

## Como leituras obrigatórias são criadas

### Forma 1: Configurada no cadastro do documento
No passo 4 do wizard de **Cadastrar documento interno**:
```
☑ Definir leitura obrigatória após publicação
   Para quem? [ Selecione usuários ou grupos ]
```

Quando o documento é publicado, sistema cria automaticamente um registro de leitura obrigatória para cada pessoa selecionada.

### Forma 2: Configurada no detalhe do documento
Em **Detalhe do documento → seção Controle → Leitura obrigatória → Editar lista**.

Adicionar/remover pessoas a qualquer momento (mesmo após publicação).

### Forma 3: Em massa por grupo
Selecionar **um grupo de permissões inteiro** (ex: "Operacionais CT Caieiras"). Sistema cria registros para cada membro ativo do grupo.

> Quando alguém entra no grupo depois, é **automaticamente** incluído na leitura obrigatória de docs que tinham leitura por grupo.

## Filtros disponíveis

| Filtro | O que filtra |
|---|---|
| **Documento** | Filtra por documento específico — útil para "ver quem ainda não leu o POP-007" |
| **Usuário** | Quais documentos o user X tem pendente |
| **Status** | Pendente / Lido / Todos |
| **Período de atribuição** | Faixa de datas |
| **Período de leitura** | Faixa de datas (quando confirmaram) |

## Ações nesta tela

### Cobrar usuário (linha pendente)
- Botão na linha "Cobrar".
- Reenviar e-mail para a pessoa lembrando da leitura pendente.

### Cobrar todos pendentes (em massa)
- Selecionar várias linhas Pendentes.
- Botão "Cobrar selecionados" → envia e-mail para cada um.

### Remover obrigação
- Botão "Remover" na linha.
- Tira a pessoa da lista de leitura obrigatória daquele doc.
- Útil quando a pessoa saiu do setor / da empresa.

### Exportar
- Gera Excel com a lista filtrada.
- Útil para auditoria: "lista de pessoas que ainda não leram o POP-007 em 30 dias".

## Como o usuário comum confirma leitura

1. Recebe **e-mail**: "Leitura obrigatória de POP-007. Acesse para confirmar."
2. Recebe **notificação no sino** (in-app).
3. Aparece **na aba Tarefas → Solicitação** (ou área dedicada, dependendo).
4. Clica → abre o detalhe do documento.
5. Painel direito mostra alerta: "Você precisa confirmar a leitura deste documento. [Confirmo que li]".
6. Lê o PDF (visualizador embutido).
7. Clica em **"Confirmo que li"**.
8. Sistema registra: data + IP. Audit log: `docs.read_required.confirmed`.
9. Notificação some.

## Notificações enviadas

| Quando | Quem |
|---|---|
| Documento publicado com leitura obrigatória configurada | Lista inteira (e-mail + in-app) |
| Lembrete (depois de N dias sem leitura) | Quem ainda não leu |
| Cobrança manual ("Cobrar pendentes") | Quem ainda não leu |
| Documento revisado (nova revisão publicada) | Lista inteira recebe nova obrigação |
| Lista atualizada (nova pessoa adicionada) | Apenas a nova pessoa |

> Quando há **nova revisão**, **todas as leituras antigas viram obsoletas** automaticamente. Toda a lista precisa confirmar leitura da nova versão. Isso é por desenho — auditoria exige que confirmações sejam por revisão.

## Auditoria — perguntas que esta tela responde

| Pergunta | Filtro |
|---|---|
| Quem leu o POP-007? | Documento = POP-007, Status = Lido |
| Quem ainda não leu o POP-007? | Documento = POP-007, Status = Pendente |
| O João leu todos os documentos obrigatórios para ele? | Usuário = João, Status = Pendente (deve estar vazio) |
| Quantos % da equipe leu o novo Manual? | Documento = MAN-005 → conta Lido vs Total |

## Estados especiais

### Lista vazia
"Nenhuma leitura obrigatória cadastrada".

### Documento sem leitura obrigatória
Não aparece nesta tela. Para criar, vá no detalhe do documento.

### Usuário inativo na lista
Mostra com badge "Inativo". Pode remover, mas registros antigos (já confirmados) ficam para histórico.

## Permissões

| Ação | Permissão |
|---|---|
| Ver esta tela | `docs.read_required.manage` |
| Cobrar pendente | `docs.read_required.manage` |
| Remover obrigação | `docs.read_required.manage` |
| Exportar | `docs.export` |
| Confirmar leitura (como usuário comum) | qualquer um na lista |

## Exemplo Seven

**Cenário**: Beatriz publica POP-007 novo. Define leitura obrigatória para grupo "Operacionais CT Caieiras" (15 pessoas).

1. POP-007 vira Publicado.
2. Sistema cria 15 registros de leitura obrigatória.
3. 15 e-mails são disparados.
4. Em 7 dias: 12 confirmaram.
5. Beatriz acessa "Controle de leitura obrigatória", filtra POP-007 + Pendente.
6. Vê 3 nomes: Pedro X, Ana Y, Bruno Z.
7. Seleciona os 3 → "Cobrar selecionados".
8. Pedro lê no mesmo dia. Ana e Bruno demoram mais 3 dias.
9. Em 10 dias: 100% confirmaram.
10. Beatriz exporta evidência para auditoria interna: "Todos os operacionais leram o POP-007".
