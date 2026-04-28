# Implantação Seven — passo a passo do dia 1

Roteiro pragmático de **o que configurar e em que ordem**, do zero até começar a usar.

## Pré-requisitos

- ✅ Conta no QualyTeam ativa (trial ou pago).
- ✅ Você tem usuário Admin (recebeu credenciais).
- ✅ Lista pronta: pessoas que vão usar, unidades da empresa, processos, primeiros documentos a cadastrar.

## Etapa 1 — Login e exploração inicial (Dia 1, 30 min)

1. Acesse `https://auth.qualyteam.com/login`.
2. Faça login com seu usuário Admin.
3. Sistema redireciona para `admin.qualyteam.com/processes`.
4. Navegue rapidamente nos seletores de módulo para ver o que tem disponível.
5. Confira o trial: "Você tem N dias para continuar o teste".

## Etapa 2 — Configurar Administrador (Dia 1, 2-3 horas)

### 2.1 Cadastrar Processos

`Administrador → Processos`. Sistema vem com 5 processos genéricos. Inativar e cadastrar os da Seven:

- ✅ Operações
- ✅ Tratamento e Destinação
- ✅ Comercial
- ✅ Ambiental
- ✅ Manutenção
- ✅ Administrativo / Financeiro
- ✅ RH e Saúde Ocupacional
- ✅ Qualidade

Detalhes em [Processos](../1-administrador/processos.md).

### 2.2 Cadastrar Unidades organizacionais

`Administrador → Unidades organizacionais`. Sistema vem com unidades-exemplo (Matriz, Berlim, Amsterdã, Chennai). Inativar e cadastrar as da Seven:

- ✅ Matriz (sede administrativa)
- ✅ CT Caieiras (centro de tratamento)
- ✅ Base Norte (depósito de caminhões)
- ✅ Base Sul
- (outras unidades específicas)

Detalhes em [Unidades](../1-administrador/unidades.md).

### 2.3 Criar Grupos de permissões

`Administrador → Grupos de permissões`. Já vem 1 (Admin\*) — não mexa.

Criar:

- **Coord. Ambiental Seven**: tudo exceto exclusão de RNC, riscos e usuários
- **Encarregado Seven**: lê documentos, registra ocorrência, sem acesso a Riscos / Oportunidades / Admin
- **Auditor Externo**: só Consulta + Exportar de cada módulo
- **Diretor / Gerência**: leitura de tudo + aprovações

Detalhes em [Grupos de permissões](../1-administrador/grupos-permissoes.md).

### 2.4 Cadastrar Usuários

`Administrador → Usuários`. Cadastrar as pessoas da Seven, atribuindo cada uma ao grupo certo.

Mínimo recomendado pra começar:
- Você (Admin Seven)
- 1 backup admin (importante)
- Coord. Ambiental
- 2-3 encarregados
- Diretor

Detalhes em [Usuários](../1-administrador/usuarios.md).

### 2.5 (Opcional) Campos personalizados

Se você precisa de campos extras, cadastre agora antes de começar a usar os módulos. Senão pode ficar para depois — campos personalizados podem ser adicionados a qualquer momento.

Sugestões para Seven:
- Em **Riscos → Identificação**: "Órgão fiscalizador" (seleção: IBAMA / CETESB / Outro), "Lei aplicável" (texto)
- Em **NC → Detalhes**: "Cliente afetado" (texto), "Houve dano ambiental?" (sim/não)
- Em **Documentos → Configuração**: "Número da licença" (texto)

Detalhes em [Campos personalizados](../1-administrador/campos-personalizados.md).

## Etapa 3 — Configurar módulos operacionais (Dia 2)

### 3.1 Não Conformidades

`Não conformidades → Configuração do módulo`:

- Origens: deixar as 7 padrão + adicionar "Reclamação ambiental", "Auto-denúncia regulatória", "Comunidade vizinha"
- Métodos: ☑ 5 Porquês ☑ Ishikawa
- Prazos: 90/24/30 dias (eficácia/AI/AC)
- Aprovador padrão: Diretor
- ☑ Exigir aprovação do plano

Detalhes em [Configuração NC](../3-nao-conformidades/configuracao.md).

### 3.2 Oportunidades

`Oportunidades → Configuração do módulo`:

- Tamanho: 3x3
- Eixos personalizados (se quiser): "Viabilidade técnica" × "Benefício ambiental"
- Quadrantes: matriz padrão
- Prazo de validação: 60 dias
- Aprovação do plano: ☐ desativada

Detalhes em [Configuração Oportunidades](../4-oportunidades/configuracao-inicial.md).

### 3.3 Riscos — configurar primeira unidade

`Riscos → Unidades → Configurar unidade`:

Para cada unidade da empresa, criar configuração própria. Comece pela **CT Caieiras** (a mais crítica para Seven):

- Unidade: CT Caieiras
- Responsável: Coord. Ambiental
- Período: 6 meses
- Tamanho: 4x4
- Matriz: agressiva (qualquer combinação Provável+ × Crítico = Evitar)

Depois faça outras unidades.

Detalhes em [Configurar unidade Riscos](../5-riscos/configurar-unidade.md).

## Etapa 4 — Cadastrar primeiros documentos (Dia 3-7)

### 4.1 Documentos externos primeiro (mais simples)

Cadastre as **licenças e certificados ambientais vigentes**:

- Licença CETESB de operação CT Caieiras
- Licença CETESB de outras unidades
- Alvarás municipais
- Certificados de calibração
- Normas técnicas relevantes (NBR 10004, etc.)

Para cada um:
1. Vai em `Documentos → Documento externo`.
2. Preenche título, categoria, unidade, validade, anexa o PDF.
3. Define responsável pela renovação (você).

Detalhes em [Cadastrar documento externo](../2-documentos/novo-documento-externo.md).

### 4.2 Documentos internos (POPs principais)

Identifique 5-10 POPs / procedimentos principais da Seven que **já existem em arquivo** e migre para o sistema.

Para cada um:
1. `Documentos → Documento interno`.
2. Wizard 4 passos.
3. Designa Elaborador (geralmente quem já cuidava do documento) e Aprovador (você ou Diretor).
4. Elaborador anexa o PDF existente.
5. Aprovador aprova.
6. Documento fica Publicado.

Em 1-2 semanas você terá o backbone do SGQ no sistema.

Detalhes em [Cadastrar documento interno](../2-documentos/novo-documento-interno.md).

## Etapa 5 — Treinar a equipe (Dia 5-15)

Pessoas que precisam de treinamento:
- **Encarregados / operacionais**: como **registrar ocorrência** + como **consultar documento** + como **confirmar leitura obrigatória**.
- **Coord. Ambiental e Qualidade**: tudo do MVP.
- **Diretor**: como aprovar planos de NC + como ler dashboards.

Cria, dentro do sistema, um **POP "Como usar o QualyTeam"** com leitura obrigatória para todos os usuários. Fica auto-documentado.

## Etapa 6 — Mapear riscos iniciais (Dia 10-30)

Workshop interno para identificar os primeiros 20-30 riscos:

- Por unidade.
- Categorias: Ambiental, Operacional, Saúde e Segurança, Legal.
- Cada risco passa pelo wizard `Identificar risco`.
- Define plano de tratamento para os críticos.

Detalhes em [Identificar risco](../5-riscos/identificar-risco.md).

## Etapa 7 — Operação em regime (Dia 30+)

A partir daqui o sistema funciona sozinho:
- Operacionais registram ocorrências quando acontecem.
- Cron monitora validade de documentos e dispara alertas.
- Cron monitora reavaliações de risco.
- Coord. Ambiental olha **Dashboards** semanalmente.
- Reuniões mensais usam dados do sistema.

## Checklist resumido

```
[ ] Login OK
[ ] Processos cadastrados (Seven)
[ ] Unidades cadastradas (Seven)
[ ] Grupos de permissões criados
[ ] Usuários cadastrados
[ ] Campos personalizados (se necessário)
[ ] Configuração NC (origens, prazos, aprovador)
[ ] Configuração Oportunidades (matriz)
[ ] Configuração Riscos (1ª unidade — CT Caieiras)
[ ] Configuração Riscos (outras unidades)
[ ] Licenças ambientais cadastradas como documentos externos
[ ] POPs principais migrados como documentos internos
[ ] Equipe treinada
[ ] Riscos iniciais mapeados
```

## Cronograma estimado

| Etapa | Tempo |
|---|---|
| 1 + 2 (Admin) | Dia 1 |
| 3 (Configurar módulos) | Dia 2 |
| 4 (Documentos) | Dia 3-7 |
| 5 (Treinamento) | Dia 5-15 (paralelo) |
| 6 (Riscos iniciais) | Dia 10-30 |
| 7 (Operação) | Dia 30+ |

## Quem deve estar envolvido

| Papel | Quanto tempo |
|---|---|
| Beatriz (você) | 80h no primeiro mês |
| Diretor | 4h (aprovações iniciais + treinamento) |
| Coord. de cada unidade | 2-3h cada (configuração da unidade no Riscos) |
| Encarregados | 1h (treinamento + cadastro inicial de leitura obrigatória) |

## Riscos da implantação (meta!)

- ⚠️ **Pular configuração** e começar a usar: vai bagunçar dados e ter que refazer.
- ⚠️ **Cadastrar documentos sem ter Categorias e Unidades**: vai ter que voltar para corrigir.
- ⚠️ **Não treinar a equipe**: ninguém usa, sistema vira jardim abandonado.
- ⚠️ **Apenas você usa**: vira ponto único de falha. Tenha 1 backup.

## Suporte

- **Suporte oficial**: `suporte@qualyteam.com.br`.
- **Documentação oficial**: `https://suporte.qualyteam.com.br/pt-BR/support/home`.
- **Esta documentação interna**: nesta wiki, atualizada conforme implantação.
