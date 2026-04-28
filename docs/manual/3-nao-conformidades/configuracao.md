# Não Conformidades — Configuração do módulo

## Onde fica

`Não conformidades → menu (⋮ ou link "Configuração do módulo")` (URL: `/module-configuration`)

## Quem acessa

`nc.config.update`. Geralmente: Coord. Qualidade.

## O que se configura aqui

### 1. Origens

Taxonomia de "de onde vem a NC". Aparece como dropdown obrigatório no cadastro de Ocorrência e RNC.

```
Origens cadastradas:
[ + Nova origem ]

│ Nome                       │ Em uso? │ Editar │ Excluir         │
│ Auditoria interna          │ 18 NCs  │ ✏      │ 🗑 (bloqueada)  │
│ Auditoria externa          │ 5 NCs   │ ✏      │ 🗑 (bloqueada)  │
│ Inspeção                   │ 25 NCs  │ ✏      │ 🗑 (bloqueada)  │
│ Reclamação cliente         │ 8 NCs   │ ✏      │ 🗑 (bloqueada)  │
│ Reclamação interna         │ 12 NCs  │ ✏      │ 🗑 (bloqueada)  │
│ Indicador desviado         │ 3 NCs   │ ✏      │ 🗑 (bloqueada)  │
│ Análise crítica            │ 1 NC    │ ✏      │ 🗑 (bloqueada)  │
│ Reclamação ambiental       │ 0 NCs   │ ✏      │ 🗑              │
```

Adicionar (`nc.origin.create`): nome simples. Aparece imediatamente nos cadastros.
Excluir: bloqueado se origem está em uso. Inativar funciona como soft delete.

### 2. Métodos de análise habilitados

```
☑ 5 Porquês
☑ Diagrama de Ishikawa
☐ A3 / Outro
```

Marcar/desmarcar afeta o que aparece como opção no passo 2 da RNC.

### 3. Prazos padrão

```
Prazo padrão de eficácia (dias após última AC concluída): [ 90 ]

Prazo padrão de ação imediata (horas): [ 24 ]

Prazo padrão de ação corretiva (dias): [ 30 ]
```

Esses são **defaults** que aparecem pré-preenchidos nos formulários. O usuário pode mudar caso a caso.

### 4. Aprovador padrão (opcional)

```
Aprovador padrão para RNCs: [ Selecione                     ▾]
```

Se preenchido, é sugerido como aprovador no passo de plano de ação. Pode ser sobrescrito caso a caso.

### 5. Notificações

```
Lembrar responsável de ação corretiva:
☑ 3 dias antes do prazo
☑ 1 dia antes do prazo
☑ no dia do vencimento
☑ 1 dia depois do vencimento
☑ semanalmente após vencimento

Lembrar verificador de eficácia:
☑ no dia previsto da verificação
☑ semanalmente até verificação
```

### 6. Numeração

```
Prefixo do código de RNC: [ RNC- ]
Próximo número: [ 019 ]      ← apenas leitura, incrementa automaticamente
```

Você pode trocar prefixo se preferir (ex: para "NC-" ou "Q-").

### 7. Workflow opcional: aprovação

```
☑ Exigir aprovação do plano antes de iniciar implementação

   Quem aprova?
   ⊙ Aprovador definido caso a caso (escolhido pelo Coord ao criar a RNC)
   ⊙ Sempre o aprovador padrão configurado
   ⊙ Hierarquia (chefe direto do responsável)
```

Se desativar a aprovação, RNC vai direto de "Causa e Planejamento" para "Implementação". Não recomendado para SGQs maduros.

## Botão Gravar

Salva as configurações. Mudanças valem **a partir das próximas RNCs criadas** — RNCs já abertas não são afetadas.

## Estados especiais

### Origem em uso
Bloqueado para excluir. Mensagem: "Esta origem está em uso em N NCs. Inative em vez de excluir."

### Mudar prefixo de numeração com RNCs já criadas
Permitido. RNCs antigas mantêm prefixo antigo, novas usam o novo.

## Audit log

- `nc.config.updated` (com diff antes/depois)
- `nc.origin.created` / `nc.origin.updated` / `nc.origin.deactivated`

## Permissões

| Ação | Permissão |
|---|---|
| Ver / editar configurações gerais | `nc.config.update` |
| Adicionar origem | `nc.origin.create` |

## Exemplo Seven — configuração inicial

Ao implantar o sistema, Beatriz acessa Configuração:

1. **Origens**: deixa as 7 padrão e adiciona:
   - "Reclamação ambiental" (importante para Seven)
   - "Auto-denúncia regulatória"
   - "Comunidade vizinha"

2. **Métodos**: ☑ 5 Porquês ☑ Ishikawa (suficiente).

3. **Prazos**:
   - Eficácia: 90 dias.
   - AI: 24 horas.
   - AC: 30 dias.

4. **Aprovador padrão**: João Diretor (Diretor Operacional).

5. **Notificações**: marcar todas (exposição máxima a alertas).

6. **Numeração**: prefixo "RNC-", próximo número 001.

7. **Workflow**: ☑ Exigir aprovação (recomendado).

Clica Gravar. Sistema pronto para receber as primeiras RNCs.
