# CIVITAS — PROMPT 00 — Publicação Canônica do Método

**Versão:** 1.0 — proposta para validação
**Repositório:** `MGlienke/CIVITASProntidaoNormativa` — branch `main`
**Planilha-base identificada:** `CIVITAS Prontidão Normativa - Metodo - v1.xlsx`
**Biblioteca:** `/CIVITAS/Prontidao Normativa/Metodo/`

## 1. Finalidade
Converter uma **versão explicitamente selecionada** da planilha Excel do Método CIVITAS em representação JSON **fiel, auditável, completa e versionada**, publicando no GitHub um artefato canônico para os Prompts 01 a 05. A planilha permanece como instrumento de autoria, desenvolvimento, validação e homologação. O JSON é uma publicação técnica da versão selecionada, **não** uma nova interpretação do Método.

**Não** avaliar entidades, executar quesitos, alterar a planilha, preencher lacunas metodológicas por inferência, recalcular avaliações existentes, produzir planilha de resultados ou painel.

## 2. Comandos
- `CIVITAS — Preparar publicação do Método v1`: converter e auditar; não publicar sem autorização explícita quando houver ressalvas metodológicas.
- `CIVITAS — Publicar Método v1`: converter, auditar e publicar como **PUBLICADO_COM_RESSALVAS** ou **HOMOLOGADO**, conforme aprovação e evidências documentadas; jamais inferir homologação.
- `CIVITAS — Auditar Método v1`: verificar a fonte e o JSON publicado, sem modificar arquivos.
- `CIVITAS — Publicar nova versão do Método v2`: selecionar novo Excel e criar nova pasta imutável; não sobrescrever v1.

Se a versão solicitada for ambígua ou houver mais de uma planilha candidata, pedir seleção antes de publicar. O nome da versão deriva da fonte e da autorização do usuário, nunca de uma contagem automática.

## 3. Fonte e prioridade
1. Recuperar **a planilha original em bytes XLSX** da Biblioteca, na versão selecionada. Usar acesso à Biblioteca e materialização de arquivo quando necessário.
2. Registrar: caminho, nome, tamanho, SHA-256 dos **bytes originais**, versão declarada, data da conversão e identificação da origem. Não calcular hash de texto extraído como se fosse hash do XLSX.
3. Inspecionar o conteúdo **real das células e fórmulas** do arquivo; usar extração tabular apenas como apoio. Não depender de trechos truncados ou resumos de conversas.
4. Conferir abas, intervalos de dados, cabeçalhos, células mescladas, tipos, valores e fórmulas. Preservar informação relevante mesmo que uma aba seja predominantemente operacional.

**Planilha v1 examinada:** seis abas `ID`, `METODO`, `AVALIACAO`, `RESULTADOS`, `PAINEL`, `LISTAS`; identificação declara 10 domínios e 115 quesitos. A aba `METODO` contém colunas:
`ID`, `Dimensão`, `Bloco`, `Questão de Verificação`, `Objetivo da Verificação`, `Fundamentação Conceitual`, `Relações Semânticas`, `Fundamentação Normativa`, `Evidências Esperadas`, `Objeto Evidenciado`, `Critério 0`, `Critério 1`, `Critério 2`, `Regra NA`, `Situação metodológica`.

## 4. Separação dos conteúdos
**Núcleo normativo-metodológico:** identidade e versão do instrumento; dimensões e seus IDs; blocos; 115 quesitos e todos os campos da aba METODO; categorias de resposta; pontos; NA; regras de verificabilidade, exclusões, denominadores, fórmulas e faixas de qualificação **somente quando expressamente suportados pela planilha**.

**Estruturas operacionais:** cabeçalhos, mapeamentos e convenções das abas AVALIACAO, RESULTADOS e PAINEL; registrar como `especificacoes_operacionais`, sem transportar valores de avaliação de exemplo, zeros iniciais, campos vazios ou qualificações de apresentação como resultados da entidade.

**Proveniência:** indicar, para cada regra extraída, a aba, célula/intervalo ou referência verificável; distinguir valor literal, fórmula efetivamente lida, regra textual e interpretação ainda não homologada.

## 5. Regras de conversão
- Preservar **literalmente** os textos do Método, inclusive acentuação, terminologia, critérios e observações de situação metodológica. Não parafrasear, simplificar, revisar ou corrigir.
- Cada quesito terá `id_quesito`, `dimensao`, `bloco`, `enunciado`, `objetivo`, `fundamentacao_conceitual`, `relacoes_semanticas`, `fundamentacao_normativa_metodo`, `evidencias_esperadas`, `objeto_evidenciado_metodo`, `criterios` com chaves `0`, `1`, `2`, `regra_na`, `situacao_metodologica` e `origem_celulas`.
- Valores vazios devem ser `null` quando semanticamente ausentes, não texto inventado. Diferenciar célula vazia de texto literal `"A definir"` ou `"Critério específico pendente de validação metodológica"`.
- Preservar a ordem original dos quesitos e das dimensões.
- Usar `PN-01.01` como ID literal, não gerar IDs por posição.
- Não tratar referência federal genérica como fundamento específico de uma entidade; a conversão apenas preserva a fundamentação metodológica existente.
- Não converter critérios metodologicamente pendentes em regras homologadas.
- Se uma regra depender de fórmula Excel, extrair e registrar a fórmula original e, quando comprovável, a interpretação matemática equivalente. Se não for possível verificar, marcar `NAO_DETERMINADA` e impedir que um prompt posterior a invente.
- Não substituir `Não verificável`, `Não aplicável` e `Não respondido` por zero. Registrar explicitamente suas definições e efeitos no denominador, quando confirmados na fonte.

## 6. Auditoria de integridade
**Estrutural:** seis abas esperadas ou divergência explicitada; 10 dimensões; 115 quesitos; unicidade de IDs; pertinência de cada quesito a uma dimensão; presença dos 15 campos da aba METODO; coerência de contagens entre ID, METODO e RESULTADOS.

**Fidelidade:** comparação célula a célula entre o Excel e o JSON gerado, com contagem de campos, valores vazios, diferenças e exemplos de conferência; auditoria literal dos critérios 0/1/2 e regras NA de todos os quesitos. Nunca declarar 100% de fidelidade se a comparação não foi executada.

**Semântica:** identificar critérios ainda em validação, campos em branco, contradições, instruções provisórias e fórmulas ou faixas ambíguas. Registrar os problemas sem corrigir a fonte.

**Operacional:** verificar se existem regras suficientes para a pontuação, exclusão de NA, cobertura, cálculo dimensional e global, arredondamento e qualificação. Distinguir `CONFIRMADA_NA_PLANILHA`, `INFERENCIA_NAO_HOMOLOGADA` e `NAO_DETERMINADA`.

**Reprodutibilidade:** SHA-256 do XLSX, versão, identificação de origem, contagem de registros, validação de sintaxe JSON e confronto do conteúdo publicado com o gerado.

Classificar cada achado como `BLOQUEANTE`, `RESSALVA_METODOLOGICA` ou `INFORMATIVO`. Divergência de IDs, perda de quesitos, campos trocados, erro de transcrição ou JSON inválido são bloqueantes. Critérios originalmente pendentes são ressalvas metodológicas **da fonte**, não falhas de conversão.

## 7. Estrutura do JSON canônico
```json
{
  "schema_version": "1.0",
  "tipo_artefato": "CIVITAS_METODO",
  "versao_metodo": "v1",
  "estado_publicacao": "PUBLICADO_COM_RESSALVAS",
  "identificacao": {
    "instrumento": "CIVITAS Prontidao Normativa",
    "dominios_declarados": 10,
    "quesitos_declarados": 115
  },
  "proveniencia": {
    "arquivo_excel": "CIVITAS Prontidão Normativa - Metodo - v1.xlsx",
    "biblioteca": "/CIVITAS/Prontidao Normativa/Metodo/",
    "sha256_xlsx": null,
    "data_conversao": null
  },
  "dimensoes": [],
  "quesitos": [],
  "regras_avaliacao": {
    "classificacoes": [],
    "nao_verificavel": null,
    "nao_aplicavel": null,
    "formulas": [],
    "faixas_qualificacao": []
  },
  "especificacoes_operacionais": {
    "avaliacao": {},
    "resultados": {},
    "painel": {}
  },
  "auditoria_referencia": "validacao.json"
}
```
O exemplo define o **contrato de estrutura**, não substitui a extração real. Os campos `null` do exemplo devem ser preenchidos quando houver evidência na planilha.

## 8. Relatório de validação
Produzir `validacao.json` com: versão, SHA-256 do Excel, estado da auditoria, abas lidas, contagens, IDs duplicados, ausências, problemas por quesito/campo, critérios pendentes, fórmulas conferidas, divergências entre Excel e JSON, decisões não homologadas, verificações executadas e recomendação de publicação.

O relatório não deve afirmar que o Método inteiro está homologado apenas porque a conversão é fiel. **Fidelidade técnica e homologação metodológica são dimensões distintas.**

## 9. Persistência no GitHub
Publicar em caminhos versionados:
```text
metodo/v1/metodo.json
metodo/v1/validacao.json
metodo/v2/metodo.json
metodo/v2/validacao.json
```
1. Verificar se os arquivos da versão já existem.
2. Não sobrescrever versões existentes; qualquer retificação de uma versão publicada exige decisão expressa e trilha de auditoria, preferindo nova revisão/versionamento.
3. Para `Publicar`, exigir auditoria técnica sem falhas bloqueantes. Se houver ressalvas metodológicas, publicar somente com estado `PUBLICADO_COM_RESSALVAS` e autorização do usuário para essa condição.
4. Criar os dois arquivos no GitHub; reler ambos; comparar integralmente o conteúdo; registrar os SHA/commits retornados.
5. Não declarar publicação concluída se um dos arquivos falhar; reportar publicação parcial e caminho exato, sem encobrir o erro.
6. A publicação do JSON **não altera** avaliações anteriores nem muda automaticamente o Método selecionado por uma avaliação.

## 10. Integração com os Prompts 01–05
- **Prompt 01:** vincular a avaliação a uma versão exata do Método e registrar o SHA do JSON canônico, além do SHA-256 do XLSX original.
- **Prompt 02:** ler a versão vinculada, recuperar o quesito por ID e transcrever seus campos literais; registrar SHA do Método e origem do quesito.
- **Prompt 03:** ler a mesma versão, auditar todos os resultados contra os 115 registros canônicos, aplicar somente fórmulas homologadas e gerar JSON consolidado; não reabrir o XLSX como fonte paralela.
- **Prompts 04 e 05:** gerar Excel e painel a partir de uma **versão explícita da consolidação**, sem recalcular índices por regras alternativas.
- Avaliações antigas com `sha256: null` devem ser sinalizadas como legado a reconciliar, **sem alteração automática**.

## 11. Teste de aceitação — Método v1
1. Ler o XLSX real da Biblioteca e calcular seu SHA-256.
2. Extrair as seis abas, 10 dimensões e 115 IDs únicos.
3. Conferir os textos literais de `PN-01.01`, `PN-01.02`, `PN-02.01` e **todos** os demais, inclusive critérios, campos vazios e situações pendentes.
4. Extrair as regras das abas LISTAS, RESULTADOS e PAINEL, verificando fórmulas efetivas; por exemplo, a aba PAINEL registra faixas operacionais `Baixa <50%`, `Intermediária 50–79,9%`, `Alta ≥80%` e alerta sobre exclusão de NA e não respondidos.
5. Gerar `metodo.json` e `validacao.json`; conferir equivalência com o XLSX.
6. Apresentar auditoria e ressalvas; publicar quando autorizado no estado apropriado; confirmar ambos os arquivos por releitura.
7. **Não** executar consolidação de Goiânia nem corrigir seus JSONs nesta etapa.

## 12. Resposta ao usuário
Informar versão, SHA-256 do XLSX, contagem de quesitos e dimensões, fidelidade auditada, ressalvas metodológicas, estado de publicação, links dos dois arquivos e confirmação de persistência. Se não houver publicação, declarar explicitamente `PREPARADO_NAO_PUBLICADO` e o motivo.
