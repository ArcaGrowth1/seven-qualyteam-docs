# Como anexar arquivos

Quase todo cadastro permite anexar arquivos (foto da ocorrência, PDF da licença, certificado de calibração, evidência de ação corretiva).

## Como anexar

A interface é sempre a mesma:

```
┌─────────────────────────────────────────────┐
│   [ Inserir anexos ]                        │
│                                             │
│   ╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌    │
│   ⤓ Também é possível arrastar arquivos    │
│     para esta área.                         │
│   ╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌    │
└─────────────────────────────────────────────┘
```

**Duas formas**:
1. Clicar **Inserir anexos** → abre seletor de arquivos do sistema operacional.
2. **Arrastar e soltar** arquivos do seu computador na área pontilhada.

## Quando o arquivo está sendo enviado

```
✓ contrato.pdf      2.3 MB  ✕
↻ foto-vazamento.jpg 4.1 MB  68%   ← barra de progresso
```

- ✓ verde = upload concluído.
- ↻ + porcentagem = upload em andamento.
- ✕ = remover anexo (descarta).

## Tipos aceitos

| Categoria | Extensões |
|---|---|
| Documentos | PDF, DOCX, DOC, ODT |
| Planilhas | XLSX, XLS, CSV, ODS |
| Imagens | JPG, JPEG, PNG, GIF, WEBP |
| CAD / técnicos | DWG, DXF |
| Apresentações | PPTX, PPT, ODP |

**Bloqueados** (por segurança):
- EXE, BAT, CMD, MSI (executáveis)
- HTML, JS, PHP (códigos)
- ZIP, RAR (compactados — anexar arquivos individuais)

## Limite de tamanho

- **Padrão**: 50 MB por arquivo.
- **Documento publicado** (área principal de Documentos): 200 MB por arquivo.
- Se passar do limite, mostra erro "Arquivo muito grande" e não envia.

## Antivírus automático

Todo arquivo anexado passa por scan antes de ficar disponível:
- Status "Aguardando verificação" por alguns segundos após upload.
- Se limpo: 🟢 ícone verde, baixar/visualizar liberado.
- Se infectado: 🔴 anexo é bloqueado, admin recebe notificação, evento gravado no audit log.

## Visualizar / baixar

Clicar no nome do arquivo:
- **PDF**: abre no visualizador embutido do sistema (não baixa) — paginação, zoom, rotação, fullscreen, print.
- **Imagem**: abre em lightbox sobre a tela atual.
- **Outros tipos**: faz download direto.

## Onde os arquivos ficam guardados

Tecnicamente: storage seguro (S3 / R2) com URL assinada que expira em 5 minutos a cada acesso. Você não precisa se preocupar com isso — o sistema regenera o link sempre que você clica.

Funcionalmente: ficam **vinculados ao registro** (documento, ocorrência, RNC, risco). Se o registro for excluído, os anexos vão junto. Se você só remove o anexo, o registro continua.

## Cópia controlada (só Documentos)

Documentos publicados podem ter **cópias controladas distribuídas** — uma versão registrada como entregue a uma pessoa ou local específico. Essa é uma cópia diferente do anexo principal:
- O **anexo principal** é o arquivo do documento em si (o PDF do POP, por exemplo).
- A **cópia controlada** é um registro de que aquele arquivo foi entregue oficialmente para X (com timestamp).
- Quando o documento é revisado, todas as cópias controladas distribuídas são marcadas como "obsoletas" automaticamente, e novas cópias precisam ser distribuídas.

Detalhes em [Detalhe do Documento](../2-documentos/detalhe-documento.md).

## Anexos no audit log

Toda ação de anexo gera registro:
- Upload: quem subiu, qual arquivo, tamanho, em qual entidade.
- Download: quem baixou, quando.
- Exclusão: quem removeu, quando.

Útil em auditoria para responder "quem viu o documento X?" e "quem tirou o anexo Y do registro Z?".

## Permissão para anexar

- **Anexar**: geralmente quem pode editar o registro pode anexar (`docs.update`, `nc.rnc.update_open`, etc.).
- **Excluir anexo**: mesma permissão de editar — exceto cópia controlada distribuída, que tem permissão dedicada (`docs.controlled_copy.delete`).
- **Baixar**: quem pode ler o registro pode baixar seus anexos.
