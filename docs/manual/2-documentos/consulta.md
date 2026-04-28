# Documentos — Consulta

## Onde fica

`Documentos → Consulta ▾` no top-bar, com 2 opções:
- **Documentos internos** (URL: `/search/internal-documents`)
- **Documentos externos** (URL: `/search/external-documents`)

## Quem acessa

`docs.read`. Geralmente: **todos** os usuários que precisam consultar documentos.

## O que é

A **lista mestra** de documentos da empresa. Substitui aquela "pasta SGQ" no servidor de arquivos. Aqui você acha tudo que está vigente, vencido, em fluxo.

## A lista de Documentos internos

```
┌──────────────────────────────────────────────────────────────────────┐
│ Documentos internos                                                   │
│         [Controle de leitura obrigatória] [Filtro] [⋮]               │
│ Total: 42                                                             │
├──────────────────────────────────────────────────────────────────────┤
│ Resumo│ Código │ Documento     │ Categoria │ Unidades  │ Processo... │
│       │ POP-001│ Lista mestra  │ POP       │ Berlim    │ GQ          │
│ 🟡    │ POP-007│ Procediment.. │ POP       │ Caieiras  │ Operações   │
│       │ MAN-002│ Manual de Seg │ Manual    │ Matriz    │ RH          │
└──────────────────────────────────────────────────────────────────────┘
```

## Cada coluna

| Coluna | O que mostra | De onde vem |
|---|---|---|
| **Resumo** | Ícone com status visual rápido (verde / amarelo / vermelho / cinza) | Calculado pelo status do doc |
| **Código** | Sigla + número + revisão (POP0001:00) | Gerado automaticamente |
| **Documento** | Título completo | Cadastro |
| **Categoria** | Tipo (POP, Manual…) | Cadastro |
| **Unidades organizacionais** | Quais unidades usam o doc | Cadastro |
| **Processo** | Área funcional | Cadastro |
| **Status** (rolando para a direita) | Em elaboração / Publicado / Vencido… | Calculado pelo fluxo |
| **Validade** | Data se houver | Cadastro |
| **Última revisão** | Data da última publicação | Histórico |
| **Elaborador / Aprovador** | Pessoas responsáveis | Cadastro |

> Nem todas as colunas aparecem por padrão — algumas são opcionais. Acesse o menu **⋮** (três pontos) → **Configurar colunas** para escolher quais mostrar.

## Filtros disponíveis

| Filtro | O que filtra |
|---|---|
| **Categoria** | POP / Manual / Política / etc. |
| **Unidades** | Multi-select de unidades |
| **Processo** | Vendas / Produção / Ambiental / etc. |
| **Status** | Em elaboração / Em aprovação / Publicado / Vencido / Inativo |
| **Validade** | Vigente / Vencendo em 30d / Vencendo em 90d / Vencido / Sem validade |
| **Período de cadastro** | Faixa de datas |
| **Período de revisão** | Faixa de datas (última revisão) |
| **Elaborador / Aprovador** | Pessoa específica |
| **Categoria personalizada** | (se sua empresa cadastrou Campos Personalizados do tipo seleção, viram filtros) |

## Ordenação (sort)

Clique no cabeçalho de qualquer coluna:
- 1ª clique: crescente.
- 2ª clique: decrescente.
- 3ª clique: sem ordenação.

Default: **última atualização decrescente** (mais recente primeiro).

## Ações por linha

### Clicar na linha
Abre o **detalhe do documento** (ver [Detalhe](detalhe-documento.md)).

### Menu três-pontos da linha (⋮)
- Editar cadastro
- Revisar (iniciar nova revisão)
- Inativar
- Distribuir cópia controlada
- Copiar link compartilhável
- Ver histórico

## Ações em massa

Marque checkboxes de múltiplas linhas:
```
[☑] [☐] [☑]      ← seleção
3 selecionados   [Distribuir cópia ▾] [Exportar selecionados] [Inativar]
```

## Botões do topo

### Controle de leitura obrigatória

Botão **"Controle de leitura obrigatória"** abre tela dedicada:

```
┌──────────────────────────────────────────────────────────────────────┐
│ Controle de leitura obrigatória                                       │
│                                                                       │
│ Filtros: [Documento  ▾]  [Usuário  ▾]  [Status: Pendente / Lido]      │
├──────────────────────────────────────────────────────────────────────┤
│ Documento │ Usuário      │ Atribuído em │ Lido em      │ Status     │
│ POP-007   │ Carlos F.    │ 28/04/2026   │ —            │ Pendente   │
│ POP-007   │ João Silva   │ 28/04/2026   │ 28/04/2026   │ Lido ✓     │
│ MAN-002   │ Maria S.     │ 22/04/2026   │ —            │ Pendente   │
└──────────────────────────────────────────────────────────────────────┘
```

Útil para auditoria: "todo mundo da operação leu o POP novo?".

### Filtro
Abre painel lateral.

### Menu três-pontos do topo (⋮)
- Exportar lista filtrada para Excel/CSV
- Imprimir lista
- Configurar colunas visíveis

## A lista de Documentos externos

Mesma estrutura, com diferenças nas colunas:

| Coluna específica de externos |
|---|
| **Órgão emissor** (CETESB, IBAMA…) |
| **Número de origem** (número do órgão, ex: 32145/2026) |
| **Validade** |
| **Responsável pela renovação** |

Filtros específicos:
- **Órgão emissor**.
- **Status de validade** (Vigente / Vencendo / Vencido).

## Como funcionam as cores do "Resumo"

| Cor | Significado |
|---|---|
| 🟢 verde | Vigente, sem alarme |
| 🟡 amarelo | Vencendo em < 30 dias |
| 🔴 vermelho | Vencido |
| ⚫ cinza | Inativo |
| 🔵 azul | Em fluxo (elaboração / aprovação) |

## Permissão para ver o quê

- **Todos com `docs.read`** veem todos os documentos da sua empresa, **respeitando filtros de unidade**.
- Documentos vinculados a unidades para as quais você não tem acesso (futuramente, com escopo por unidade) ficam invisíveis.

## Exportar

Botão **Exportar** (precisa de `docs.export`) gera Excel/CSV com:
- Todas as colunas visíveis.
- Todas as linhas do filtro atual (não só a página visível).
- Inclui campos personalizados.
- Limite: 10.000 linhas por export. Acima, pede para refinar filtros.

## Estados especiais

### Lista totalmente vazia
```
   Você ainda não tem documentos cadastrados.
       [ + Novo documento interno ]
       [ + Novo documento externo ]
```

### Lista vazia por filtro
"Sem resultados. [ Limpar filtros ]"

## Exemplo Seven — uso típico

**Caso 1 — Auditor pergunta**: "Quero ver todas as licenças ambientais vigentes da Seven".

1. Vai em Consulta → Documentos externos.
2. Filtros: Categoria = "Licença Ambiental" + Status = "Vigente".
3. Lista mostra ~20 licenças.
4. Exportar → Excel para apresentar ao auditor.

**Caso 2 — Operacional pergunta**: "Onde está o POP de derramamento?".

1. Atalho: tecla `S` para busca global → digita "derramamento".
2. Resultado mostra POP-007.
3. Clica → abre detalhe.
4. Lê o PDF embutido. Marca "Confirmar leitura" se for leitura obrigatória.

**Caso 3 — Coord. faz revisão semestral**: "Quais documentos vencem nos próximos 90 dias?".

1. Vai em Consulta → Documentos externos (e depois internos).
2. Filtro: Validade = "Vencendo em até 90 dias".
3. Vê 12 itens. Distribui revisões entre a equipe.
