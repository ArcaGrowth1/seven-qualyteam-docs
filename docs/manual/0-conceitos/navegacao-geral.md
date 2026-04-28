# Navegação geral do app

Toda página do sistema tem a mesma **barra superior**. Entender ela é metade do caminho.

## Anatomia da barra superior

```
┌──────────────────────────────────────────────────────────────────────┐
│ [Logo] │ [Módulo ▾] │ [Aba1] [Aba2] [Aba3] │     [🔔10] [⚙] [👤B]   │
└──────────────────────────────────────────────────────────────────────┘
   1         2              3                       4    5    6
```

### 1. Logo
Clicar leva para o dashboard do módulo atual (ou para o início, se já está no dashboard).

### 2. Seletor de módulo (dropdown "▾")
Clica e abre lista com os **módulos contratados** pela sua empresa:
- Administrador
- Documentos
- Não Conformidades
- Oportunidades
- Riscos

Mudar de módulo te leva para a página inicial daquele módulo (geralmente Tarefas ou Dashboard).

> **Observação**: o dropdown só mostra os módulos que **a sua empresa contratou** E que **seu grupo de permissões libera**. Se você não vê um módulo, é porque não tem permissão de ler dele.

### 3. Abas internas do módulo
Cada módulo tem suas próprias abas. Por exemplo:
- **Documentos**: Dashboard · Tarefas · Documento interno · Documento externo · Consulta
- **Não Conformidades**: Dashboard · Tarefas · Registrar ocorrência · Registrar não conformidade · Consulta

A aba ativa fica sublinhada.

### 4. Sino 🔔 (com número azul)
Mostra **novidades do sistema** (changelog do produto, não suas tarefas pessoais).
- Número = quantidade de novidades não lidas.
- Clicar abre painel lateral à direita com cards: "Novidade", módulo afetado, título, "Mais detalhes".
- Aqui aparecem coisas como "Documentos: notificações automáticas de validade para documentos externos".
- **Não confundir** com tarefas atribuídas a você — essas aparecem na aba **Tarefas** de cada módulo.

### 5. Engrenagem ⚙
Atalhos rápidos para o **Administrador** (sem precisar trocar de módulo):
- Unidades organizacionais
- Processos
- Registros de atividades
- Campos personalizados
- Usuários
- Grupos de permissões

Só aparecem os atalhos para os quais você tem permissão.

### 6. Avatar 👤
Inicial do seu nome (ex: "B" para Beatriz). Clicar abre dropdown:
- Seu nome completo
- Seu e-mail
- **Sair**

## Banner amarelo (trial)

Se sua conta está em período de teste, aparece no topo:
```
Você tem 6 dia(s) para continuar o teste · Gostei do teste! Quero adquirir
```
Some quando o plano é confirmado.

## Como saber em que módulo estou

3 sinais simultâneos:
1. **Subdomínio na URL**: `documents.qualyteam.com`, `nc.qualyteam.com`, etc.
2. **Texto no seletor de módulo** (item 2 da barra) está com o nome do módulo.
3. **Abas internas** mudam conforme o módulo.

## Voltar e avançar

Use os botões do navegador (← →) normalmente. O sistema preserva filtros aplicados na URL — se você copiar a URL e mandar pra alguém, a pessoa vai abrir a mesma visão filtrada que você.

## Tela "endereço errado"

Se você digitar uma URL inválida ou clicar num link quebrado:
```
[ www... ✕ ]
O endereço que você está acessando pode estar errado ou não existe mais.
[ Voltar para página anterior ]
```
Acontece também se você acessar uma página de uma área para a qual você **não tem permissão** — o sistema mostra essa mesma tela em vez de "acesso negado" (por questão de segurança).
