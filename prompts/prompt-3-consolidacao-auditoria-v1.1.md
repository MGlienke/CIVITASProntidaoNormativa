# CIVITAS — PROMPT 03 — Consolidação e Auditoria das Avaliações

**Versão:** 1.1 — GitHub — proposta para validação  
**Dependências:** Prompt 01 v2.1 (preparação); Prompt 02 v2.1 (avaliação individual).  
**Repositório canônico:** `MGlienke/CIVITASProntidaoNormativa`, branch `main`.

## 1. Objetivo e princípio
Consolidar **sem reavaliar** os resultados individuais de uma avaliação CIVITAS, mediante leitura dos arquivos JSON persistidos no GitHub, confronto com o Método canônico, auditoria estrutural, semântica e aritmética, cálculo de indicadores metodologicamente autorizados e publicação de **um JSON consolidado, auditável e versionado**.

O JSON consolidado é a **fonte única de dados** dos futuros Prompt 04 (planilha Excel de campo) e Prompt 05 (painel executivo). Ambos deverão usar os mesmos indicadores calculados e não recalcular ou reclassificar independentemente.

**Proibições:** não pesquisar novas evidências normativas, não reinterpretar ou modificar a classificação dos quesitos, não criar quesitos ausentes, não editar XLSX, não produzir painel, não alterar os JSONs de origem e não presumir que arquivo existente é arquivo válido.

## 2. Comandos
- `CIVITAS — Consolidar avaliação [ID_AVALIACAO]`
- `CIVITAS — Auditar consolidação da avaliação [ID_AVALIACAO]` (somente auditoria; não publica novo consolidado)
- `CIVITAS — Reconsolidar avaliação [ID_AVALIACAO]` (nova versão explícita, nunca sobrescrita silenciosa)

Exemplo: `CIVITAS — Consolidar avaliação 5208707-001`.

**Padrão:** executar uma consolidação parcial quando existirem quesitos ainda não avaliados; não interromper por falta de cobertura total, mas jamais rotular a saída parcial como final. A ausência de qualquer resultado válido não autoriza publicar indicadores como zero.

## 3. Entradas e autoridade
Recuperar do GitHub, diretamente na execução:
1. `avaliacoes/{ID_AVALIACAO}/01-ambiente.json`;
2. `avaliacoes/{ID_AVALIACAO}/02-entidade.json`;
3. todos os arquivos `avaliacoes/{ID_AVALIACAO}/03-quesitos/Dxx/PN-xx.yy.json` existentes, mediante listagem recursiva ou consulta por **todos** os IDs previstos no Método. Não inferir completude a partir de arquivos conhecidos na conversa;
4. Método canônico **obrigatório**: `metodo/v1/metodo.json` e `metodo/v1/validacao.json`, ou a versão imutável explicitamente fixada no ambiente e no controle da avaliação. Registrar versão, Git blob SHA, estado de publicação e SHA-256 do XLSX de origem apenas como proveniência.

**Sem fallback silencioso para XLSX:** o Método JSON já foi publicado no GitHub como `PUBLICADO_COM_RESSALVAS`. Se `01-ambiente.json` ou JSONs individuais legados referirem apenas XLSX/Biblioteca, registrar `MIGRACAO_METODO_PENDENTE`, confrontar versões e critérios, e não tratar equivalência como comprovada. Não migrar, corrigir ou substituir fontes sem autorização. O estado do Método não é homologação metodológica.

O Método define o universo de quesitos, IDs, dimensões, critérios, categorias, regras de NA, pontuação e fórmulas. **Nunca** derivar o universo de quesitos apenas da contagem de JSONs presentes.

Conferir se `01-ambiente.json` e `02-entidade.json` possuem o mesmo ID e contexto institucional coerente. O escopo canônico é o registrado em `02-entidade.json`; diferenças textuais de apresentação nos JSONs individuais não implicam erro se forem semanticamente compatíveis, mas divergência de abrangência institucional é impeditiva para aquele registro.

## 4. Descoberta e inventário
Construir um inventário completo dos IDs do Método e dos arquivos encontrados. Para cada ID esperado, registrar: caminho esperado, presença, leitura, sintaxe JSON, identidade, dimensão, versão de esquema, referência metodológica, status da auditoria, classificação, pontos e problemas.

Identificar separadamente:
- `AUSENTE`: nenhum arquivo do quesito;
- `INVALIDO`: arquivo ilegível, JSON inválido ou estrutura insuficiente;
- `INCOMPATIVEL`: ID, dimensão, entidade, escopo, versão do Método ou data de referência incompatíveis;
- `PENDENTE_SANEAMENTO`: registro recuperável com omissões, discrepâncias ou rastreabilidade insuficiente;
- `VALIDO_COM_RESSALVAS`: registro utilizável com ressalvas não impeditivas;
- `VALIDO`: registro íntegro e compatível.

Arquivos extras, IDs inesperados, caminhos duplicados ou múltiplas versões conflitantes devem constar do inventário e nunca ser somados ao universo do Método.

## 5. Auditoria em camadas
**A. Estrutural:** JSON legível; `schema_version` suportado; presença e tipos dos campos obrigatórios (`id_avaliacao`, `id_quesito`, `dimensao`, `entidade`, `metodo_referencia`, `quesito`, `investigacao`, `analise`, `resultado`, `auditoria`); arrays e valores nulos tratados explicitamente.

**B. Identidade e compatibilidade:** correspondência com os dois arquivos de preparação; ID existente e único no Método; dimensão e bloco corretos; mesmo Método, data de referência e escopo. Diferenciar divergência material de variação textual não material.

**C. Integridade metodológica:** comparar com a linha **literal** do Método os campos `enunciado`, `objetivo`, `fundamentacao_conceitual`, `relacoes_semanticas`, `fundamentacao_normativa_metodo`, `evidencias_esperadas`, `criterios` (0,1,2), `regra_na` e demais campos aplicáveis. Não exigir campos inexistentes no Método, mas não aceitar paráfrases como transcrição fiel. Preservar diferenças de pontuação e espaços não substantivos com regra explícita de normalização textual, sem reescrever conteúdo.

**D. Evidências e rastreabilidade:** conferir fontes, URLs, identificação documental, justificativa, achado, limitações e ligação lógica entre evidência registrada e classificação. O consolidador verifica **a documentação registrada**, não reabre fontes nem substitui a investigação do Prompt 02. Ausência de dados essenciais ou afirmação sem fonte identificável é pendência.

**E. Consistência de resultado:** confrontar categoria, pontos, aplicabilidade, verificabilidade e regra NA com o Método. Para a escala 0–2, quando confirmada: `ATENDIDO=2`, `PARCIALMENTE_ATENDIDO=1`, `NAO_ATENDIDO=0`; `NAO_VERIFICAVEL` e `NAO_APLICAVEL` não recebem pontos. Não aplicável exige fundamentação e permissão expressa. Uma investigação ausente não deve ser convertida em Não verificável.

**F. Auditoria de origem:** registrar o caminho, SHA do blob GitHub, commit/versão disponível e data da consolidação. Se hash do Método não existir, marcar ressalva de reprodutibilidade; não inventar SHA-256.

**Severidade:** classificar cada problema como `BLOQUEANTE`, `SANEAMENTO` ou `RESSALVA`, com código, ID, campo, descrição e ação requerida. Falhas de método/identidade, classificação inválida ou pontuação incoerente impedem utilização do registro. Transcrições metodológicas resumidas ou incompletas exigem saneamento antes de considerá-lo válido. Ressalvas não impeditivas não mudam a classificação original.

**Regra essencial:** o consolidador nunca corrige silenciosamente um JSON individual. Ele marca o problema e aponta o arquivo que deve ser corrigido por reavaliação ou saneamento autorizado.

## 6. Política de inclusão e indicadores
Somente `VALIDO` e `VALIDO_COM_RESSALVAS` entram nos cálculos. `PENDENTE_SANEAMENTO`, `INVALIDO`, `INCOMPATIVEL` e `AUSENTE` aparecem no inventário e nos indicadores de cobertura, mas **não** entram como nota zero.

Para cada dimensão e para o total, manter separadamente:
- `n_previstos`, `n_localizados`, `n_validos`, `n_validos_com_ressalvas`, `n_pendentes_saneamento`, `n_invalidos`, `n_incompativeis`, `n_ausentes`;
- contagens válidas por classificação;
- `n_pontuaveis` (somente classificações 0/1/2);
- `pontos_obtidos`, `pontos_possiveis`;
- `indice_percentual`, `cobertura_percentual`, `cobertura_pontuavel_percentual`, quando definidos;
- estado `PARCIAL`, `FINAL` ou `SEM_BASE_SUFICIENTE`.

**Cálculo condicionado:** utilizar a fórmula de índice, denominadores, arredondamento e agregação **expressamente previstos no Método**. Se o Método não definir inequivocamente uma fórmula, não criar índice oficial; preencher `indice_percentual: null`, registrar `FORMULA_NAO_DEFINIDA` e apresentar apenas contagens e somas factuais. Se existir fórmula válida de índice percentual baseada em pontos, aplicar estritamente a regra canônica e excluir NV/NA do denominador quando assim previsto. Com denominador zero, índice é `null`, nunca zero.

Não calcular o índice global como média simples das dimensões, a menos que o Método o determine expressamente. Não arredondar somas intermediárias; conservar precisão e registrar a regra de apresentação.

**Cobertura não é desempenho:** a porcentagem de quesitos avaliados/validados deve ser mostrada separadamente da pontuação de desempenho. Um índice parcial, se autorizado, deve ser identificado como parcial e acompanhado da cobertura; nunca apresentado como resultado final da entidade.

## 7. Critérios de publicação parcial e final
**PARCIAL:** existem quesitos ausentes, pendentes ou inválidos, mas há ao menos um registro válido; publicar inventário completo, problemas, contagens e indicadores parciais somente se permitidos pelo Método. Se nenhum registro for válido, publicar somente auditoria, com índices nulos.

**FINAL:** somente quando todos os IDs previstos estiverem presentes e com status `VALIDO` ou `VALIDO_COM_RESSALVAS`, sem problemas bloqueantes ou de saneamento, e os indicadores obrigatórios estiverem metodologicamente definidos e verificáveis. Classificações válidas `NAO_VERIFICAVEL` e `NAO_APLICAVEL` não impedem a completude se devidamente justificadas e autorizadas. A aprovação final é registrada explicitamente; não inferir homologação do usuário.

O Prompt 03 não homologa resultados individuais nem altera as notas. Um estado `FINAL` significa **tecnicamente apto para publicação**, não aprovação institucional automática.

## 8. Contrato do JSON consolidado
Gerar um JSON UTF-8 com estrutura mínima:

```json
{
  "schema_version": "1.0",
  "tipo_artefato": "CIVITAS_CONSOLIDACAO",
  "id_avaliacao": "5208707-001",
  "versao_consolidacao": 1,
  "estado": "PARCIAL",
  "data_consolidacao": "YYYY-MM-DD",
  "contexto": {
    "entidade": {},
    "escopo": {},
    "data_referencia": "YYYY-MM-DD",
    "metodo": {"arquivo": "metodo/v1/metodo.json", "versao": "v1", "git_blob_sha": null, "sha256_xlsx_origem": null, "estado_publicacao": "PUBLICADO_COM_RESSALVAS"},
    "arquivos_preparacao": []
  },
  "inventario": {
    "quesitos_previstos": 115,
    "ids_previstos": [],
    "arquivos_extras": [],
    "registros": []
  },
  "resultados": {
    "quesitos": [],
    "dimensoes": [],
    "global": {},
    "painel": {"schema_version": "1.0", "estado_mapeamento": "PENDENTE", "entidade": null, "municipio": null, "cnpj": null, "data": null, "instrumento": null, "total": null, "indice_geral": null, "aplicados": null, "atendido": null, "parcial": null, "nao_atendido": null, "na": null, "nv": null, "pendentes": null, "dimensoes": [], "sintese": {"fortes": [], "atencao": [], "lacunas": [], "recomendacoes": []}, "nota": null}
  },
  "auditoria": {
    "estado": "CONCLUIDA_COM_RESSALVAS",
    "problemas": [],
    "contagens": {},
    "formulas_aplicadas": [],
    "formulas_nao_definidas": [],
    "integridade": {}
  },
  "publicacao": {
    "apto_planilha": false,
    "apto_painel": false,
    "ressalvas": []
  }
}
```

O exemplo indica **estrutura**, não valores reais. O objeto `resultados.painel` é uma **projeção de apresentação sem recálculo** e não substitui `resultados.global` ou `resultados.dimensoes`. Campos não disponíveis ou não autorizados ficam `null` e o estado de mapeamento registra a limitação. A existência dessa projeção não torna o painel automaticamente publicável. `ids_previstos` deve conter todos os IDs extraídos do Método. `inventario.registros` terá uma entrada por quesito esperado com status, problemas e referência ao JSON original. `resultados.quesitos` conterá **todos os registros válidos**, preservando os dados necessários à planilha: dimensão, bloco, enunciado, classificação, pontos, achado, justificativa, evidências, fontes, URLs, ação de preparação, recomendação, limitações e caminho/SHA da origem. Não perder informações importantes para uso do consultor em campo. `resultados.dimensoes` terá uma entrada por dimensão do Método, inclusive dimensões sem quesitos avaliados.

Se houver necessidade de dados de campo futuros, reservar estrutura para o Prompt 04, **sem misturar observações posteriores à avaliação original**.

## 9. Persistência e versionamento GitHub
Destino imutável de cada consolidação:
`avaliacoes/{ID_AVALIACAO}/04-consolidacoes/consolidacao-vNNN.json`

Exemplo: `avaliacoes/5208707-001/04-consolidacoes/consolidacao-v001.json`.

Antes de gravar:
1. Descobrir as versões já existentes no diretório; selecionar `v001` se nenhuma existir, ou a próxima versão sequencial. Não sobrescrever versões anteriores.
2. Construir o JSON integralmente e validar sintaxe, tipos, referências, contagens, consistência aritmética e ausência de duplicidade de IDs.
3. Registrar no próprio JSON os SHA dos arquivos de origem, quando disponíveis, e os parâmetros do cálculo. A integridade do JSON de consolidação poderá ser verificada pelo SHA retornado pelo GitHub após o commit.
4. Criar arquivo no GitHub com commit específico; **reler o arquivo**, comparar integralmente o conteúdo e só então declarar `PERSISTIDO`.
5. Se falhar a escrita, a releitura ou a comparação, informar o estágio e não declarar sucesso.

Uma reconsolidação produz **nova versão**, inclusive quando saneamentos modificarem resultados individuais. O Prompt 03 não modifica `01-ambiente.json`, `02-entidade.json` ou arquivos em `03-quesitos/`. Não usar arquivo `latest.json` como autoridade sem controle transacional; consumidores devem selecionar explicitamente uma versão consolidada.

**Observação:** a numeração `04-consolidacoes/` identifica o quarto tipo de artefato na pasta da avaliação (após ambiente, entidade e quesitos), **não** o número do prompt. O componente responsável continua sendo o Prompt 03.

## 10. Contrato explícito com o Painel HTML v0.3

Referências publicadas:
- `painel/v0.3/template.html`
- `painel/v0.3/template-original.html`
- `painel/v0.3/contrato-dados.json`
- `painel/v0.3/validacao.json`

O contrato HTML está `PUBLICADO_COM_RESSALVAS` e contém mapeamentos `origem_exata_consolidacao: null`. O Prompt 03 v1.1 define uma projeção `resultados.painel` para permitir futura vinculação determinística, **sem modificar o contrato do HTML já publicado**. A atualização/homologação do contrato será tarefa versionada posterior.

| Campo da projeção `resultados.painel` | Origem autorizada |
| --- | --- |
| `entidade`, `municipio`, `cnpj`, `data`, `instrumento` | Identificação e escopo validados de `01-ambiente.json` / `02-entidade.json` e referência do Método, sem inferir dados ausentes |
| `total` | Contagem de IDs únicos em `metodo.quesitos`, conferida com `inventario.quesitos_previstos` |
| `indice_geral` | Índice global **já calculado e auditado** em `resultados.global`, somente se a fórmula estiver autorizada |
| `aplicados` | Indicador metodologicamente definido e auditado; **não** presumir `total - nv - na` |
| `atendido`, `parcial`, `nao_atendido`, `na`, `nv`, `pendentes` | Contagens das classificações e estados válidos, mantendo diferenças entre NA, NV, pendentes e ausentes |
| `dimensoes` | Exatamente dez entradas em ordem canônica de `metodo.dimensoes`, com `codigo`, `nome`, `total`, `atendido`, `parcial`, `nao_atendido`, `na`, `nv`, `pendentes`, `indice`; índice não disponível = `null`, nunca 0 |
| `sintese` | `fortes`, `atencao`, `lacunas`, `recomendacoes` **somente** se fundamentados em achados e ações dos quesitos válidos; não gerar conclusões novas sem rastreabilidade |
| `nota` | Texto derivado de estado parcial/final, cobertura, ressalvas do Método, limitações de fórmula e auditoria; sem ocultar insuficiência de base |

**Regra de dimensionalidade:** o Método usa códigos `PN-01` a `PN-10`; o template visual pode exibir `D1` a `D10`. Registrar o mapeamento explícito `PN-01 → D1` etc. como **rótulo de apresentação**, sem mudar os IDs canônicos nem reordenar dimensões.

**Indicadores e qualificações:** se a fórmula do Método for indeterminada, `indice_geral` e os índices dimensionais afetados devem permanecer `null`, com problema `FORMULA_NAO_DEFINIDA`. O Prompt 03 não escolhe faixas de cores. As faixas visuais provisórias do template v0.3 não são regras de pontuação e não se tornam homologadas por esta atualização.

**Condição de prontidão:** `publicacao.apto_painel` só pode ser `true` quando (a) o consolidado foi auditado, (b) todos os campos obrigatórios têm origem verificável ou tratamento de ausência aprovado, (c) o contrato do template está compatível e validado e (d) os avisos de parcialidade são exibíveis. Enquanto o contrato v0.3 tiver mapeamentos pendentes e testes funcionais não realizados, registrar `apto_painel: false` e os motivos. Não bloquear a **publicação da consolidação** por indisponibilidade do painel.

**Compatibilidade com legados:** `avaliacoes/5208707-001/04-consolidacoes/consolidacao-v001.json` é parcial, não contém `resultados.quesitos` com cargas completas e refere XLSX; não reclassificar como compatível. A primeira reconsolidação conforme v1.1 deve criar **v002**, após reconciliar o Método e os registros individuais, sem sobrescrever v001.

## 11. Contrato dos consumidores
O **Prompt 04 — Planilha** e o **Prompt 05 — Painel** recebem `ID_AVALIACAO` e `versao_consolidacao`, leem **exclusivamente o JSON consolidado indicado** e verificam `auditoria`, `publicacao` e `estado`. Não reabrem fontes para reavaliar, não corrigem classificações e não recalculam índices com fórmulas alternativas. O Prompt 04 poderá gerar planilha parcial claramente marcada para acompanhamento; o Prompt 05 só poderá apresentar indicadores parciais com aviso visível de cobertura e ressalvas. O relatório final exige estado `FINAL` e requisitos de publicação satisfeitos.

A planilha é instrumento de campo: resultados originais devem permanecer identificáveis e imutáveis, enquanto observações e atualizações de campo ficam em campos separados.

## 12. Resposta ao usuário
Informar: ID, versão, estado PARCIAL/FINAL, cobertura de quesitos, quantidade válida, quantidade pendente de saneamento, quantidade ausente, situação dos indicadores, principais problemas e link do JSON confirmado. Não usar rótulo de sucesso quando a persistência não foi confirmada.

Se houver problemas, listar os IDs prioritários e a ação requerida, **sem corrigir a origem**. Não interromper uma consolidação parcial só porque ainda faltam quesitos; a publicação parcial deve refletir fielmente as lacunas.

## 13. Teste de aceitação — Goiânia
Executar `CIVITAS — Consolidar avaliação 5208707-001`. Esperado: identificar os **115 quesitos** do Método, descobrir os três JSONs de avaliação já gravados (`PN-01.01`, `PN-01.02`, `PN-02.01`) e registrar os demais como ausentes, sem inventar resultados. O `PN-01.01` deve ser identificado como `PENDENTE_SANEAMENTO` por não reproduzir integralmente os campos do Método. Verificar consistência dos outros registros, publicar `consolidacao-v002.json` com estado `PARCIAL` **somente após reconciliar a versão do Método e conferir a carga completa dos registros**, confirmar leitura e conteúdo, e **não** gerar Excel ou HTML.
