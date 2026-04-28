# Administrador — Campos personalizados

## Onde fica

`Administrador → Campos personalizados` (URL: `/custom-fields/<modulo>`)

## Quem acessa

- Cadastrar: `admin.custom_fields.create`.
- Editar: `admin.custom_fields.update`.

## O que é "Campo personalizado"

Um campo extra que você adiciona em formulários do sistema **sem mexer em código**. Útil quando o cadastro padrão (do Documento, NC, etc.) não tem um campo que sua empresa precisa.

Exemplo Seven:
- Em **Risco**: adicionar campo "Lei aplicável" (texto), "Órgão fiscalizador" (seleção única: IBAMA, CETESB, etc.), "Tipos de resíduo afetados" (seleção múltipla).
- Em **Não Conformidade**: adicionar campo "Cliente afetado", "Custo estimado", "Houve dano ambiental?".

## A tela

Quando você entra:

```
┌─────────────────────────────────────────────────────────────────┐
│ Campos personalizados                          Módulo            │
│                                                [ Riscos      ▾]  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                       [ 📁 ilustração ]                          │
│                                                                 │
│              Você ainda não tem campos personalizados.           │
│                Clique abaixo para adicionar.                     │
│                                                                 │
│                  [ + Adicionar campo personalizado ]             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Seletor de módulo (canto superior direito)

Os campos personalizados são **escopados por módulo**. Você precisa escolher o módulo antes de cadastrar:
- Documentos
- Não conformidades
- Oportunidades
- Riscos

> Só aparece módulo que sua empresa tem contratado.

## Como criar um campo (wizard 2 passos)

### Passo 1: Local

```
Novo campo personalizado
[1] Local         [2] Configurações

Local

Módulos *
[ Riscos                                                       ▾ ]

Etapa
Identificação do risco

[ Próximo ]
[ Cancelar ]
```

- **Módulo**: pré-selecionado pelo seletor anterior. Você pode trocar aqui.
- **Etapa**: depende do módulo. Indica em qual passo do wizard de cadastro o campo vai aparecer.

#### Etapas disponíveis por módulo

| Módulo | Etapas |
|---|---|
| Documentos | Informações · Responsável · Documentos associados · Configuração |
| Não conformidades (Ocorrência) | Detalhes · Ações imediatas |
| Não conformidades (RNC) | Descrição · Análise de causa · Plano de ação · Verificação |
| Oportunidades | Identificação · Avaliação · Plano de ação |
| Riscos | Identificação do risco · Avaliação · Tratamento · Reavaliação |

### Passo 2: Configurações

```
Novo campo personalizado
[1] Local        [2] Configurações ●

Configurações

Tipo do campo *
[ Selecione o tipo de campo                                   ▾ ]

  📝 Parágrafo
  ⊙ Seleção única
  ⊙ Seleção múltipla

☐ Campo obrigatório

Rótulo *
[ Escrever                                                       ]
                                                          0/120

[ Gravar ]
[ Voltar ]
```

#### Tipos disponíveis

| Tipo | O que é | Exemplo |
|---|---|---|
| **Parágrafo** | Texto livre, área de várias linhas | "Observações detalhadas", "Justificativa" |
| **Seleção única** | Dropdown com 1 opção escolhida | "Órgão fiscalizador" → IBAMA / CETESB / Outros |
| **Seleção múltipla** | Lista com várias selecionáveis | "Tipos de resíduo afetados" → marca quantos quiser |

#### Para Seleção única / múltipla

Após escolher o tipo, abre uma área para configurar **as opções**:
```
Opções
[ + Adicionar opção ]
[ chip: IBAMA ✕ ]
[ chip: CETESB ✕ ]
[ chip: Secretaria municipal ✕ ]
```

#### Outros parâmetros

- **Campo obrigatório**: se marcar, o campo precisa ser preenchido para gravar o cadastro.
- **Rótulo**: nome que aparece no formulário (ex: "Órgão fiscalizador *"). Limite 120 caracteres.

### Salvar e usar

1. Clica **Gravar**.
2. O campo aparece na lista de campos personalizados do módulo.
3. **Imediatamente**, todos os formulários de cadastro daquele módulo (no passo correspondente) passam a mostrar o novo campo.

## Lista de campos cadastrados

Após cadastrar um, a tela passa a mostrar a lista:

```
Campos personalizados                      Módulo
                                           [ Riscos      ▾]
                            [ + Adicionar campo personalizado ]

│ Etapa                  │ Rótulo                 │ Tipo       │ Editar │
│ Identificação do risco │ Órgão fiscalizador     │ Seleção... │   ✏    │
│ Identificação do risco │ Lei aplicável          │ Parágrafo  │   ✏    │
│ Avaliação              │ Custo estimado em R$   │ Parágrafo  │   ✏    │
```

## Como editar um campo

- Ícone **✏** abre form preenchido.
- Você pode mudar:
  - Rótulo
  - Obrigatório (sim/não)
  - Opções (em selects)
- Você **não pode** mudar:
  - Tipo (ex: Parágrafo → Seleção única) — porque dados antigos não fariam sentido.
  - Módulo / Etapa — porque rebquebraria os dados.

Se precisar mudar tipo: criar campo novo, migrar dados manualmente, depois excluir o antigo.

## Como reordenar campos

(Funcionalidade pode estar em roadmap.) No MVP, a ordem segue a ordem de cadastro. Para reordenar, exclua e recadastre.

## Como excluir

Botão "Excluir" no detalhe do campo.
⚠️ Atenção: dados já preenchidos nesse campo **são apagados** junto. Sistema avisa antes.

## Onde os campos aparecem para o usuário final

Quando alguém abre o wizard do módulo (ex: "Identificar risco"), ao chegar na etapa correspondente, o campo aparece **junto com os campos nativos do sistema**, como se sempre tivesse estado lá.

Exemplo no cadastro de risco, etapa "Identificação":

```
Identificação do risco

Unidade organizacional *
Categoria *
Descrição do risco *
Causa potencial
Consequência potencial

─── Campos personalizados ───
Órgão fiscalizador *  [Selecione  ▾]
Lei aplicável         [...]
```

## Onde aparecem na consulta / detalhe

- **Detalhe do registro**: aparecem em uma seção dedicada "Campos personalizados".
- **Listas mestras**: por padrão **não** aparecem como colunas (para não poluir). Você pode habilitar via "configurar colunas visíveis" no menu três-pontos.
- **Filtros**: campos do tipo seleção (única/múltipla) viram filtros disponíveis automaticamente.
- **Exportações** (CSV/Excel): incluem todos os campos personalizados como colunas.

## Audit log

- `admin.custom_field.created`
- `admin.custom_field.updated`
- `admin.custom_field.deleted`

## Estados especiais

### Lista vazia
Mostra ilustração + CTA "Adicionar campo personalizado".

### Tentar editar com registros já preenchidos
Mudanças não-destrutivas (rótulo, novas opções) são liberadas. Mudanças destrutivas (remover opção em uso) bloqueiam ou avisam.

## Exemplo Seven — campos sugeridos

### Em Riscos (etapa Identificação do risco)
- "Órgão fiscalizador" (seleção única: IBAMA, CETESB, FEAM, Sec. Municipal, Outros)
- "Lei / norma aplicável" (parágrafo)
- "Tipos de resíduo afetados" (seleção múltipla: Classe I, IIA, IIB, Específicos…)

### Em Não Conformidades (etapa Detalhes)
- "Cliente afetado" (parágrafo)
- "Houve dano ambiental?" (seleção única: Sim / Não / Investigar)
- "Custo estimado em R$" (parágrafo)

### Em Documentos (etapa Configuração)
- "Número da licença ambiental" (parágrafo)
- "Órgão emissor" (seleção única)
