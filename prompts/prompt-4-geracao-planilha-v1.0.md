# CIVITAS — PROMPT 04 — Geração da Planilha de Resultados

**Versão:** 1.0 — especificação para validação
**Repositório:** `MGlienke/CIVITASProntidaoNormativa` (`main`)
**Dependências:** Coordenador v1.1; Prompt 03 v1.1; Método JSON v1.
**Saída:** arquivo Excel XLSX versionado, relatório de validação e manifesto de proveniência.

## 1. Objetivo
Produzir uma **planilha Excel de resultados da avaliação de Prontidão Normativa CIVITAS**, a partir de uma consolidação específica, íntegra e auditada do Prompt 03. A planilha serve para leitura, análise e uso de campo, não para reexecutar a avaliação, reinterpretar critérios ou recalcular índices por fórmulas alternativas.

A planilha original `CIVITAS Prontidão Normativa - Metodo - v1.xlsx`, disponível na Biblioteca `/CIVITAS/Prontidao Normativa/Metodo/`, é referência de **organização, conteúdo metodológico e identidade visual**, não arquivo a ser alterado durante a avaliação. A fonte metodológica computacional é `metodo/v1/metodo.json` na versão fixada.

## 2. Comandos
- `CIVITAS — Gerar planilha da avaliação [ID_AVALIACAO] a partir da consolidação [VERSAO]`
- `CIVITAS — Auditar planilha da avaliação [ID_AVALIACAO] [VERSAO]` (sem modificações)
- `CIVITAS — Regenerar planilha da avaliação [ID_AVALIACAO] a partir da consolidação [VERSAO]` (nova versão do artefato, sem sobrescrever anteriores)

Se a versão da consolidação não for fornecida, o Coordenador deve selecionar **explicitamente** uma versão auditada e registrar sua decisão; não presumir que o último arquivo por nome é o correto.

## 3. Entradas obrigatórias
1. `avaliacoes/{ID_AVALIACAO}/04-consolidacoes/consolidacao-vNNN.json`: **fonte única dos resultados**.
2. `metodo/v1/metodo.json` (ou versão fixada): enunciados, critérios, dimensões, regras e metadados.
3. `metodo/v1/validacao.json`: ressalvas metodológicas.
4. `avaliacoes/{ID_AVALIACAO}/01-ambiente.json` e `02-entidade.json`: somente para conferência de identidade e escopo, sem substituir valores auditados da consolidação.
5. `CIVITAS Prontidão Normativa - Metodo - v1.xlsx`: **referência visual** recuperada da Biblioteca para preservar as abas e a formatação pertinente. Se indisponível, registrar `REFERENCIA_VISUAL_INDISPONIVEL` e não alegar fidelidade visual.

Registrar versões, caminhos, SHA de blob GitHub dos JSONs, SHA-256 do XLSX original quando verificado e data da geração. Não confundir SHA Git com SHA-256 de bytes.

## 4. Pré-condições e bloqueios
- A consolidação existe, foi relida integralmente e sua estrutura, integridade e auditoria são verificáveis.
- A identidade, o escopo e a versão do Método são compatíveis.
- `resultados.quesitos` preserva a carga documental completa dos registros válidos, inclusive justificativas, evidências, fontes, achados e ações de preparação.
- `inventario.registros` representa todos os IDs previstos no Método, inclusive ausentes, inválidos e pendentes.
- `resultados.dimensoes` inclui todas as dez dimensões; `resultados.global` fornece os indicadores metodologicamente autorizados.

Se a consolidação tiver apenas contagens e índices sem a carga completa, **não gerar uma planilha aparentemente completa**: registrar `CONSOLIDACAO_INSUFICIENTE` e solicitar reconsolidação versionada. A consolidação legada `avaliacoes/5208707-001/04-consolidacoes/consolidacao-v001.json` tem essa limitação; não a tratar como fonte suficiente.

Se o Método estiver `PUBLICADO_COM_RESSALVAS`, reproduzir essa condição na planilha e no relatório. Não atribuir qualificação final quando a fórmula necessária não estiver homologada.

## 5. Estrutura do arquivo Excel
Preservar as seis abas metodológicas, com nomes reconhecíveis e função claramente identificada:

### `ID` — Identificação e proveniência
Entidade, identificadores oficiais quando disponíveis, abrangência, data de referência, ID da avaliação, Método (versão/SHA), consolidação (versão/SHA), versão da planilha, situação `PARCIAL`/`FINAL`, cobertura, ressalvas, data de geração e observação de que o resultado é uma fotografia auditada da avaliação.

### `METODO` — Referência metodológica congelada
Transcrever **literalmente** os 115 quesitos do Método JSON, mantendo ID, dimensão, bloco, enunciado, objetivo, fundamentações, relações, evidências esperadas, regra NA, critérios 0/1/2 e situação metodológica. Não substituir textos, preencher lacunas, reordenar IDs ou editar critérios.

### `AVALIACAO` — Resultados individualizados
Uma linha para cada ID previsto, na ordem canônica, com no mínimo as colunas do Método:
`ID`, `Dimensão`, `Bloco`, `Questão de Verificação`, `Resposta`, `Pontos`, `Evidências Encontradas`, `Objeto efetivamente evidenciado`, `Achado`, `Ação de Preparação`, `Justificativa NA`.

Adicionar, de maneira **separada e identificada**, campos de rastreabilidade: status de auditoria, justificativa, URLs/fontes, limitações, data de referência, caminho/SHA do JSON individual e pendências. Não eliminar ou truncar informações da carga documental. Se houver múltiplas evidências e URLs, usar aba complementar de evidências ou vínculo detalhado, preservando a correspondência por ID.

Para registros `AUSENTE`, `INVALIDO`, `INCOMPATIVEL` e `PENDENTE_SANEAMENTO`, apresentar status próprio e campos de resposta/pontuação **vazios ou nulos**. Nunca converter ausência em `NÃO ATENDE` nem pontuar zero. `NA` e `NV` são categorias distintas, com justificativas quando exigidas.

### `RESULTADOS` — Desempenho consolidado
Preservar os campos de referência: `Código`, `Dimensão`, `Total de quesitos`, `Respondidos`, `Atendidos`, `Parciais`, `Não atendidos`, `NA`, `Pontuação (%)`, `Qualificação`. Acrescentar explicitamente, sem misturar com os campos originais, `NV`, `Pendentes`, `Cobertura (%)` e situação da dimensão quando disponíveis.

Exibir **valores do JSON consolidado**, não fórmulas concorrentes. Fórmulas Excel podem ser usadas para apresentação, links e verificações auxiliares **sem substituir os índices consolidados**. Se uma fórmula metodológica estiver indeterminada, índice e qualificação permanecem em branco/`S/I` e a ressalva fica visível. Índice parcial deve ser rotulado como parcial e acompanhado da cobertura.

### `PAINEL` — Base tabular para o Painel Executivo
Preservar os campos de referência `Código`, `Dimensão`, `Qualificação`, `Pontuação`, `Cobertura de respostas`, alimentados pela **mesma consolidação**. Esta aba não é a fonte de dados do Prompt 05, que lerá o JSON diretamente; serve para conferência e uso da planilha.

### `LISTAS` — Domínios de classificação
Reproduzir os domínios e categorias oficiais do Método, mantendo a distinção entre `ATENDIDO`, `PARCIALMENTE_ATENDIDO`, `NAO_ATENDIDO`, `NAO_VERIFICAVEL`, `NAO_APLICAVEL` e estados de auditoria. Não criar novas categorias oficiais.

### Complementos permitidos
`EVIDENCIAS`: tabela normalizada por quesito/fonte, com URLs, tipo documental, evidência, referência e status de auditoria.
`CAMPO`: observações, encaminhamentos, responsável, data e ações posteriores, **vazios na geração** e claramente separados do resultado original; nunca alimentar automaticamente `RESULTADOS` a partir dessas observações.
`AUDITORIA`: inventário de problemas, ressalvas e reconciliação de contagens.
Criar abas complementares apenas quando necessárias para preservar integralidade e utilidade, sem comprometer as seis abas-base.

## 6. Identidade visual e integridade do XLSX
Ler a referência Excel original sem alterá-la; aproveitar quando tecnicamente possível suas cores, tipografia, larguras, cabeçalhos, filtros, congelamento de painéis, validações e formatos. Não preservar fórmulas antigas se puderem gerar valores inconsistentes com o JSON; substituir por valores consolidados e registrar a mudança no relatório. Não criar gráficos ou indicadores sem dados autorizados.

Garantir células legíveis, quebra de texto, filtros nas tabelas extensas, navegação e impressão adequadas. URLs devem permanecer acessíveis. Evitar conteúdo de célula que execute fórmulas inadvertidamente; tratar textos de fontes externas como texto, não fórmulas.

**Não** editar a planilha original, não atualizar planilhas durante a execução de cada dimensão e não depender de recálculo automático para exibir resultados oficiais.

## 7. Auditoria da geração
Verificar:
1. XLSX válido e reaberto após exportação.
2. Exatamente 115 IDs canônicos, únicos e ordenados em `METODO` e `AVALIACAO`, conforme versão do Método.
3. Dez dimensões em `RESULTADOS` e `PAINEL`, com mesma ordem do Método.
4. Correspondência exata de classificações, pontos, totais, percentuais e qualificações com a consolidação.
5. Distinção de `null`, `S/I`, `NA`, `NV`, ausência e zero real.
6. Preservação de justificativas, evidências, fontes, links, achados e ações para cada registro válido.
7. Somatórios e contagens consistentes com o inventário; cobertura e desempenho separados.
8. Ausência de `#REF!`, `#DIV/0!`, `#VALUE!` e fórmulas não autorizadas em indicadores oficiais.
9. Identidade visual confrontada com a referência quando disponível; registrar testes visuais realizados ou não.
10. Conferência de versão/SHA do Método e da consolidação, inclusive ressalvas e estado parcial/final.

Produzir relatório com `APROVADO`, `APROVADO_COM_RESSALVAS` ou `REPROVADO`, distinguindo bloqueios de observações.

## 8. Publicação e versionamento
Destino proposto:
```text
avaliacoes/{ID_AVALIACAO}/05-planilhas/
  resultados-v001.xlsx
  resultados-v001-validacao.json
```
Cada nova geração confirmada usa versão sequencial; não sobrescrever. Publicar **somente** quando houver mecanismo de gravação binária compatível no GitHub. Ferramentas de criação de arquivos UTF-8 textuais não são apropriadas para gravar XLSX binário; não converter XLSX para texto fingindo que é planilha. Se não houver mecanismo compatível, entregar o XLSX e a validação como arquivos para download e registrar `PUBLICACAO_GITHUB_PENDENTE`, sem declarar persistência no repositório.

O relatório de validação registra caminho/versão/SHA da consolidação, versão do Método, SHA-256 do XLSX produzido, número de linhas, indicadores comparados, testes, ressalvas e estado da publicação. Reabrir o XLSX para verificar integridade; quando houver publicação, reler e verificar o arquivo persistido.

## 9. Contrato com Coordenador e Prompt 05
O Coordenador seleciona e fixa `id_avaliacao`, `versao_consolidacao`, `git_blob_sha_consolidacao`, `versao_metodo` e `git_blob_sha_metodo`.
O Prompt 04 gera a planilha a partir desse snapshot. O Prompt 05 gera o HTML a partir **do mesmo snapshot**, independentemente da planilha. A aba `PAINEL` é apenas uma representação de conferência, não um intermediário obrigatório nem fonte autoritativa.
Se a consolidação mudar, gerar nova versão da planilha; não alterar retrospectivamente o arquivo anterior.

## 10. Resposta ao usuário
Informar ID da avaliação, versão da consolidação, estado, cobertura, resultados disponíveis, limitações, validação, arquivo XLSX gerado e sua localização efetivamente verificada. Não afirmar sucesso de publicação GitHub sem confirmação de persistência.

## 11. Teste de aceitação
A consolidação legada de Goiânia `consolidacao-v001.json` deve ser recusada como fonte suficiente, por ausência de carga documental completa. Após reconsolidação íntegra e auditada em nova versão, gerar planilha com 115 linhas de quesitos, dez dimensões, todas as evidências dos quesitos válidos e demais linhas explicitamente pendentes. Verificar a coincidência dos indicadores com a consolidação, inclusive índices nulos quando não autorizados.
