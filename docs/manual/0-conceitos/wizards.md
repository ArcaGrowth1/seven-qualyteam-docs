# Como funcionam os wizards (cadastros multi-passo)

Quase todo cadastro do sistema usa **wizard** — passos numerados no topo, um de cada vez. Isso reduz erros e quebra forms longos em pedaços digeríveis.

## Anatomia

```
            ┌── 1. Etapa A ──┬── 2. Etapa B ──┬── 3. Opcional ──┐
            │      ●         │       2        │       3         │
            └────────────────┴────────────────┴─────────────────┘

[ campos da etapa atual ]

                          [ Voltar ]   [ Próximo ]
```

- Bola preta com `●` = etapa atual.
- Número = etapa ainda não preenchida.
- Bola verde com `✓` = etapa já completada e válida.
- "(opcional)" no label = etapa pode ser pulada.

## Como navegar

| Ação | Como |
|---|---|
| **Próximo** | Botão azul no rodapé. Só fica clicável quando a etapa atual está válida (todos os obrigatórios preenchidos) |
| **Voltar** | Botão "Voltar" no rodapé ou clicar diretamente em uma etapa anterior na barra de passos |
| **Pular etapa opcional** | Botão "Pular" (em algumas) ou simplesmente "Próximo" sem preencher |
| **Cancelar** | Botão "Cancelar" — descarta tudo e volta para a tela anterior |
| **Gravar** | Aparece como botão final apenas no último passo |

## Validação

- **Inline**: erro aparece logo abaixo do campo após você sair dele (blur).
- **Asterisco vermelho `*`**: campo obrigatório.
- **Contador `0/500`**: limite de caracteres em campos longos.
- **Botão Próximo desabilitado**: significa que tem campo obrigatório vazio ou inválido.

## Rascunho automático

Se você preenche metade do wizard, **fecha a aba do navegador sem querer**, e abre de novo:
- Sistema **detecta o rascunho** salvo localmente (no seu navegador).
- Pergunta: "Há um rascunho deste cadastro. Continuar?"
- "Sim" recupera o que você havia preenchido.
- "Não, começar do zero" descarta.

> Rascunho é **local do navegador** (não salvo no servidor). Trocou de máquina ou limpou cache = rascunho some.

## Wizards do sistema (visão geral)

| Cadastro | Etapas |
|---|---|
| Documento interno | 1. Informações · 2. Responsável · 3. Documentos associados (opcional) · 4. Configuração (opcional) |
| Documento externo | (mesmo padrão, com campos próprios) |
| Registrar ocorrência (NC) | 1. Detalhes · 2. Ações imediatas (opcional) |
| Registrar não conformidade | 1. Descrição · 2. Análise de causa · 3. Plano de ação · 4. Verificação |
| Nova oportunidade | 1. Identificação · 2. Avaliação inicial (P × I) |
| Identificar risco | 1. Identificação · 2. Avaliação · 3. Tratamento |
| Configurar unidade (Riscos) | 1. Informações Básicas · 2. Reavaliação · 3. Apetite ao risco |
| Novo campo personalizado | 1. Local (módulo + etapa) · 2. Configurações (tipo + rótulo) |
| Novo usuário | (form único, sem wizard) |

## Mensagem de sucesso ao gravar

Ao final, ao clicar **Gravar**:
1. Toast verde aparece no canto superior direito: "✓ [Item] criado com sucesso".
2. Você é redirecionado para o **detalhe do item** (não para a lista). Isso permite continuar trabalhando nele.
3. O item já aparece na lista mestra (Consulta) imediatamente.

## Erro ao gravar

Se o servidor recusa (ex: código duplicado, campo inválido):
1. Toast vermelho: "Erro: [mensagem técnica]".
2. Você fica na mesma etapa.
3. Os campos do erro são destacados em vermelho.
4. Os dados preenchidos **não são perdidos**.

## Permissão para criar

Cada wizard exige uma permissão. Se você não tem, o link/botão para abrir o wizard nem aparece.

| Wizard | Permissão |
|---|---|
| Documento interno | `docs.internal.create` |
| Documento externo | `docs.external.create` |
| Registrar ocorrência | `nc.occurrence.create` |
| Registrar RNC | `nc.rnc.create` |
| Nova oportunidade | `opp.create` |
| Identificar risco | (responsável da unidade ou admin) |
| Configurar unidade (Riscos) | `risk.unit_config.create` |
