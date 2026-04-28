# Não Conformidades — Registrar ocorrência

## Onde fica

`Não conformidades → Registrar ocorrência` (URL: `/occurrence/new`)

## Quem acessa

`nc.occurrence.create`. **Geralmente liberado para todos** os usuários da empresa (operacional registra a maioria).

## Quando usar

Para registro **leve, rápido**, de qualquer evento não conforme. Pode ser **antes** de virar RNC formal — algumas ocorrências viram RNC, outras só ficam registradas.

Exemplos:
- Vazamento de óleo (pequeno, contornado).
- Atraso de coleta.
- Equipamento avariado.
- Reclamação de cliente.
- Quase-acidente.
- Inspeção apontou irregularidade.

> Quem registra a ocorrência **não precisa saber a causa** nem definir plano de ação. Só descreve o que aconteceu. O Coord. Qualidade decide depois.

## O wizard (2 passos)

```
            ┌── 1. Detalhes ──┬── 2. Ações imediatas (opcional) ──┐
```

### Passo 1: Detalhes

```
Registrar ocorrência
[1] Detalhes ●  [2] Ações imediatas (opcional)

Descrição *
[ Vazamento de óleo lubrificante na bombona 042. Aproximadamente 3L vazaram   ]
[ no piso do galpão de manutenção. Conteve com pano absorvente.                ]
                                                                        65/3000

Data da ocorrência *
[ DD/MM/AA ]

Unidades organizacionais *
[ Selecione                                                                   ▾]

Processos *
[ Selecione                                                                   ▾]

Origem *
[ Selecione                                                                   ▾]

Notificar outros usuários por e-mail
☐
[ ........... ]

Inserir anexos
[ + Inserir anexos ]
[ Também é possível arrastar arquivos para esta área. ]

[ Próximo ]
[ Cancelar ]
```

#### Cada campo

| Campo | O que é | Obrigatório? |
|---|---|---|
| **Descrição** | Texto livre descrevendo o evento. Seja específico mas conciso | ✅ Max 3000 caracteres |
| **Data da ocorrência** | Quando aconteceu (não quando você está registrando) | ✅ |
| **Unidades organizacionais** | Onde aconteceu (multi-select) | ✅ |
| **Processos** | Em qual processo (multi-select) | ✅ |
| **Origem** | Como/de onde foi identificado | ✅ |
| **Notificar outros** | Mandar e-mail de ciência | Opcional |
| **Anexos** | Fotos, vídeos, documentos | Opcional |

#### Origens (configurável no módulo)

Default no QualyTeam (ajuste na sua empresa):
- **Auditoria interna** (auditoria do SGQ identificou)
- **Auditoria externa** (cliente, certificadora)
- **Inspeção** (rotina, supervisor encontrou)
- **Reclamação de cliente**
- **Reclamação interna** (pessoa do próprio time)
- **Indicador desviado** (KPI estourou)
- **Análise crítica** (reunião de gestão identificou)
- **Outro**

> Origens são cadastradas em **Configuração do módulo**. Quem tem `nc.origin.create` pode adicionar.

### Passo 2: Ações imediatas (opcional)

```
[2] Ações imediatas (opcional)

Você quer registrar ações imediatas tomadas?

[ + Adicionar ação imediata ]

Ação 1
   Descrição: [ ............................ ]
   Responsável: [ Selecione                  ▾]
   Prazo: [ DD/MM/AA ]
   Status inicial: ⊙ Pendente / ⊙ Em execução / ⊙ Concluída

[ Voltar ]   [ Criar ações imediatas ]   [ Pular e gravar ]
```

> "Ações imediatas" = **contenção rápida** (horas/dias). Não confundir com Ações Corretivas (eliminação de causa raiz, semanas/meses).

#### Cada AI

- **Descrição**: o que foi/precisa ser feito.
- **Responsável**: quem executa.
- **Prazo**: data limite.
- **Status**: se já foi feita ao registrar (caso comum — operacional já conteve antes de registrar).

## Botões finais

| Botão | O que faz |
|---|---|
| **Pular e gravar** (passo 2) | Cria a ocorrência sem AIs |
| **Criar ações imediatas** | Cria ocorrência + cria as AIs definidas. Notifica responsáveis das AIs |

## O que acontece ao gravar

1. Ocorrência é criada com **status "Aberta"**.
2. Toast verde: "Ocorrência OC-042 registrada".
3. Redireciona para o **detalhe da ocorrência**.
4. **Notificações disparadas**:
   - Responsável definido (campo opcional, ou Coord. Qualidade default) recebe e-mail.
   - Pessoas em "Notificar outros" recebem e-mail de ciência.
   - Cada responsável de AI recebe notificação separada.

## Onde a ocorrência aparece depois

- **Tarefas → Ocorrências** do responsável (e do Coord. Qualidade).
- **Consulta → Ocorrências** (lista mestra).
- **Dashboard → widgets de Ocorrência**.
- Audit log: `nc.occurrence.created`.

## Encerrar a ocorrência (no detalhe)

Após registrar, no detalhe da ocorrência:
- Botão **"Encerrar"** (precisa de `nc.occurrence.close`):
  - Pede justificativa: "Por que está sendo encerrada sem virar RNC?".
  - Ocorrência muda para **"Encerrada"**.
- Botão **"Escalar para RNC"** (precisa de `nc.rnc.create`):
  - Cria uma RNC vinculada à ocorrência.
  - Ocorrência fica como **"Escalada"**.
  - Você é redirecionado para o detalhe da RNC para iniciar **Causa e Planejamento**.

## Estados especiais

### Origem não cadastrada
Sistema vem com origens padrão. Se sua empresa removeu todas, lista fica vazia e cadastro é bloqueado. Solução: cadastrar pelo menos 1 origem em Configuração do módulo.

### Data futura
Bloqueado: "Data não pode ser futura".

### Sem unidade ou processo cadastrado
Wizard bloqueado. Cadastrar primeiro em Admin.

## Permissão x notificação

- **Permissão**: define quem **pode registrar**.
- **Notificação**: define quem **fica sabendo** (independente de permissão de NC — o user só precisa estar ativo).

## Audit log

- `nc.occurrence.created`
- `nc.immediate_action.created` (uma por AI)
- `nc.occurrence.notified` (uma por user notificado)

## Exemplo Seven — registro real

**Cenário**: Carlos (motorista) volta da rota e percebe vazamento na bombona que estava transportando.

**Passo 1 — Detalhes**:
- Descrição: "Bombona 042 com vazamento detectado ao final da rota R-15. Aprox. 2L de resíduo Classe IIA caíram no compartimento traseiro do caminhão. Sem contaminação externa. Bombona substituída."
- Data da ocorrência: hoje, 14h30.
- Unidades: CT Caieiras
- Processos: Operações
- Origem: Inspeção
- Notificar: Beatriz Brito (Coord. Qualidade) ☑
- Anexos: 2 fotos da bombona + foto do compartimento

**Passo 2 — Ações imediatas**:
- AI 1: "Limpar compartimento traseiro com produto absorvente" — Carlos, prazo: 4h, status: Concluída (já fez)
- AI 2: "Inspecionar lote de bombonas similar (lote 2026-08)" — Pedro Almoxarifado, prazo: 48h, status: Pendente

**Gravar**:
- OC-042 criada.
- Beatriz recebe e-mail.
- Pedro recebe notificação para fazer AI-2.
- Beatriz analisa, vê que pode ser caso isolado (pano absorvente já conteve), encerra como ocorrência sem virar RNC.
- 48h depois, Pedro inspeciona o lote, conclui que está OK, conclui AI-2.
- OC-042 fica encerrada com lições aprendidas registradas.
