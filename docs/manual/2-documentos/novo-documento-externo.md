# Documentos — Cadastrar documento externo

## Onde fica

`Documentos → Documento externo` (URL: `/new/external-document`)

## Quem acessa

`docs.external.create`. Geralmente: Coord. Qualidade, equipe ambiental.

## Quando usar

Documento que **vem de fora** e você só quer controlar / catalogar:

| Caso | Exemplo Seven |
|---|---|
| Licença ambiental | Licença CETESB de operação, IBAMA, alvará municipal |
| Certificado | ISO 14001, NBR 10004 (laudo de classificação) |
| Norma técnica | NBR 10004, NBR 12235, ABNT |
| Lei / regulamento | Lei 12.305 (PNRS), Decreto Estadual |
| Contrato | Contrato com cliente, com transportador |
| Certificado de calibração | Equipamento de medição vindo de laboratório acreditado |

> Para documento que **sua empresa elabora**, use **Documento interno**.

## Diferença em relação ao Interno

| | Interno | Externo |
|---|---|---|
| Tem ciclo Elaboração → Aprovação? | ✅ | ❌ |
| Wizard | 4 passos | 2-3 passos (mais simples) |
| Numeração? | Sim, com sigla da categoria | Sim, mas geralmente baseada na numeração do órgão emissor |
| Validade? | Opcional | Quase sempre tem |
| Leitura obrigatória? | Comum | Raro |
| Cópia controlada? | Comum | Raro |

## O wizard (3 passos típicos — pode variar)

### Passo 1: Informações

```
Cadastrar documento externo
[1] Informações ●  [2] Validade  [3] Configuração (opcional)

Informações do documento

Título *
[ Licença CETESB nº 12345/2026 — Operação CT Caieiras       ]

Categoria *
[ Licença Ambiental                                          ▾]

Unidades organizacionais *
[ CT Caieiras                                                ✕]

Processo *
[ Ambiental                                                  ▾]

Órgão / fonte emissora
[ CETESB                                                     ]

Número de origem
[ 12345/2026                                                 ]

Anexos *
[ + Inserir anexos ]
[ ✓ licenca-cetesb-12345-2026.pdf  3.2 MB  ✕ ]

[ Próximo ]
```

#### Campos do passo 1 (típicos para externos)

| Campo | O que é | Obrigatório? |
|---|---|---|
| **Título** | Identificação humana (ex: "Licença CETESB n° X/2026") | ✅ |
| **Categoria** | Tipo (Licença Ambiental, Certificado, Norma, Lei…) | ✅ |
| **Unidades** | Para quais unidades aplica | ✅ |
| **Processo** | Área funcional | ✅ |
| **Órgão / fonte** | Quem emitiu (CETESB, IBAMA, ABNT, cliente) | Opcional mas recomendado |
| **Número de origem** | Número que o órgão deu (LO 12345/2026) | Opcional |
| **Anexos** | O PDF / arquivo do documento em si | ✅ |

### Passo 2: Validade

```
[2] Validade ●

☑ Tem data de validade
   Validade até *: [ 11/05/2027 ]
   Notificar X dias antes do vencimento: [ 30 ]

   ☑ Notificar 30 dias antes
   ☑ Notificar 7 dias antes
   ☑ Notificar 1 dia antes
   ☑ Notificar quando vencido

Responsável pela renovação *
[ Beatriz Brito                                              ▾]

[ Voltar ]   [ Próximo ]
```

#### Campos

| Campo | O que é |
|---|---|
| **Validade até** | Data exata em que o documento perde validade |
| **Antecedências de notificação** | Múltiplos checkboxes — quando você quer ser avisado |
| **Responsável pela renovação** | Quem recebe os avisos. Geralmente o Coord. Qualidade ou Ambiental |

### Passo 3: Configuração (opcional)

```
[3] Configuração (opcional)

☐ Substituir automaticamente versão anterior
   (se já existe um doc externo com mesmo número de origem,
    inativar o antigo após salvar este)

Observações / contexto
[ ........................................................... ]

[ Voltar ]   [ Gravar ]
```

## Botão "Gravar"

1. Documento é criado com status **"Vigente"** (já está publicado, não tem fluxo interno).
2. Anexo é processado pelo antivírus.
3. Toast verde: "Documento externo cadastrado".
4. Redireciona para detalhe.
5. Se configurada notificação imediata: lista de notificados recebe e-mail.

## Onde aparece depois

- **Consulta → Documentos externos**: na lista mestra.
- **Tarefas → Validade**: quando chegar perto do vencimento.
- **Dashboard**: nos widgets de validade.

## Como atualizar (renovar quando vencer)

Quando a licença é renovada (você recebe a nova versão do órgão):

1. Acessa o documento existente em **Consulta → Documentos externos**.
2. Botão **"Substituir versão"** (ou "Revisar") no painel direito.
3. Faz upload do novo PDF.
4. Atualiza nova data de validade.
5. Sistema mantém histórico da versão anterior (não apaga).
6. Status fica novamente "Vigente".
7. Tarefa de validade some.

## Notificações

| Quando | Quem |
|---|---|
| Cadastro inicial | Responsável pela renovação |
| 30 dias antes do vencimento | Responsável + lista configurada |
| 7 dias antes | Responsável + lista |
| 1 dia antes | Responsável + Admin |
| Vence hoje | Responsável + Admin + Gerência |
| Vencido (semanal) | Todos os envolvidos |

## Estados especiais

### Anexo obrigatório
Não dá para gravar sem anexar arquivo. Diferente do Interno, onde o Elaborador anexa depois.

### Mesma versão, novo arquivo
Use **Substituir** (não "Cadastrar novo"). Substituir mantém vínculo histórico.

### Documento sem validade
Pode existir (ex: norma técnica que não vence) — basta deixar o toggle desmarcado no passo 2.

## Permissões necessárias

| Ação | Permissão |
|---|---|
| Cadastrar | `docs.external.create` |
| Substituir versão | `docs.update` ou `docs.published_file.replace` |
| Inativar | `docs.update` |

## Exemplo Seven — passo a passo real

**Cenário**: cadastrar a Licença de Operação CETESB do CT Caieiras.

**Passo 1**:
- Título: "Licença de Operação CETESB n° 32145/2026 — CT Caieiras"
- Categoria: "Licença Ambiental"
- Unidades: CT Caieiras
- Processo: Ambiental
- Órgão: CETESB
- Número de origem: 32145/2026
- Anexo: PDF da licença escaneada (4 MB)

**Passo 2**:
- ☑ Tem data de validade: 11/05/2027
- Notificar 30 / 7 / 1 dia antes
- Responsável: Beatriz Brito

**Passo 3**:
- Pular.

**Gravar**:
- Documento aparece em Consulta → Externos com status Vigente.
- 11/04/2027 (30 dias antes): Beatriz recebe alerta. Tarefa em Validade.
- Beatriz contata o consultor, renova licença.
- Beatriz acessa o doc → Substituir versão → upload do novo PDF + nova validade 11/05/2028.
- Documento volta a 🟢 Vigente. Histórico mostra as 2 versões.
