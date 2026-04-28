# Glossário

## Termos transversais

| Termo | Sigla | Definição |
|---|---|---|
| Sistema de Gestão da Qualidade | SGQ | Conjunto de processos para garantir conformidade com requisitos (ISO 9001) |
| Sistema de Gestão Ambiental | SGA | ISO 14001 |
| Tenant | — | Organização cliente isolada no sistema (ex.: Seven Resíduos é 1 tenant) |
| Unidade organizacional | UO | Filial, fábrica ou matriz de um tenant |
| Processo | — | Área funcional do tenant (Vendas, Produção, Gestão Ambiental…) |
| Audit log | — | Registro imutável de quem fez o quê e quando |
| Soft delete | — | Marcar como inativo sem apagar — preserva histórico/auditoria |
| Multi-tenancy | — | Capacidade de servir múltiplos clientes isolados na mesma instância |

## Documentos

| Termo | Definição |
|---|---|
| **Documento interno** | Procedimento, política, manual elaborado dentro da organização. Tem ciclo de elaboração → aprovação → publicação |
| **Documento externo** | Documento vindo de fora (licença, certificado, norma). Apenas controlado, não elaborado |
| **Categoria** | Tipo de documento (POP, Política, Manual, Norma…). Definido pelo tenant |
| **Revisão** | Versão de um documento. Numerada (00, 01, 02…) |
| **Validade** | Data de expiração de um documento (gera tarefa de revisão) |
| **Cópia controlada** | Distribuição rastreada de um documento publicado para um usuário/local específico |
| **Leitura obrigatória** | Marcação que exige que um conjunto de usuários confirme leitura |
| **Lista mestra** | Visão consolidada de todos os documentos vigentes |

## Não Conformidades

| Termo | Sigla | Definição |
|---|---|---|
| **Ocorrência** | — | Registro inicial leve de um evento (pode ou não virar RNC) |
| **Não conformidade** | NC | Desvio em relação a um requisito |
| **Relatório de Não Conformidade** | RNC | Tratamento completo de uma NC com causa, plano e verificação |
| **Origem** | — | Fonte da NC (auditoria, reclamação, inspeção…). Taxonomia configurável |
| **Ação imediata** | AI | Contenção rápida antes da análise de causa |
| **Ação corretiva** | AC | Ação que elimina a causa raiz |
| **Ação preventiva** | AP | Ação para evitar recorrência (ISO 9001:2015 substituiu por "ações para tratar riscos") |
| **5 Porquês** | — | Técnica de análise de causa por iteração |
| **Ishikawa** | — | Diagrama espinha-de-peixe para análise de causa |
| **Eficácia** | — | Verificação posterior se a ação corretiva eliminou a causa |

## Oportunidades

| Termo | Definição |
|---|---|
| **Oportunidade de melhoria** | Sugestão para melhorar processo, produto ou sistema |
| **Matriz Probabilidade × Impacto** | Grade NxN para classificar uma oportunidade |
| **Quadrante** | Célula da matriz, com ação recomendada (Implementar / Ponderar / Arquivar) |

## Riscos

| Termo | Definição |
|---|---|
| **Risco** | Efeito da incerteza sobre objetivos (ISO 31000) |
| **Apetite ao risco** | Quantidade de risco que a organização aceita reter |
| **Tratamento** | Ação para modificar o risco (Reter, Mitigar, Evitar, Transferir) |
| **Reavaliação** | Reexame periódico do risco (a cada 3/6/9/12 meses ou personalizado) |
| **Matriz de risco** | Grade Probabilidade × Impacto com ações por quadrante |

## Resíduos (domínio Seven)

| Termo | Sigla | Definição |
|---|---|---|
| **Manifesto de Transporte de Resíduos** | MTR | Documento que acompanha a movimentação de resíduos (futuro módulo) |
| **Certificado de Destinação Final** | CDF | Comprova destinação ambientalmente correta |
| **Gerador** | — | Quem gera o resíduo |
| **Transportador** | — | Quem leva o resíduo do gerador ao destinador |
| **Destinador** | — | Quem dá destinação final (aterro, incineração, reciclagem) |
| **Classes de resíduo** | — | NBR 10004: Classe I (perigoso), Classe IIA (não perigoso não inerte), Classe IIB (inerte) |
| **PNRS** | — | Política Nacional de Resíduos Sólidos (Lei 12.305/2010) |
