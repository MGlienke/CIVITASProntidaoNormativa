# CIVITAS — PROMPT 05 — Geração do Painel Executivo HTML

**Versão:** 1.0 — especificação para validação
**Repositório:** `MGlienke/CIVITASProntidaoNormativa`, branch `main`
**Dependências:** Coordenador v1.1, Prompt 03 v1.1, Método JSON fixado, template HTML v0.3.
**Saídas:** painel HTML autocontido por avaliação, dados de apresentação JSON, relatório de validação e manifesto de proveniência.

## 1. Objetivo e limites
Gerar o Painel Gráfico Executivo CIVITAS de Prontidão Normativa **exclusivamente a partir de uma consolidação auditada e versionada**, utilizando a composição visual do template HTML canônico já publicado.

O Prompt 05 é um **adaptador e renderizador**, não um avaliador, consolidador ou calculador alternativo. Não pesquisa normas, não reclassifica quesitos, não altera resultados, não recalcula índices, não cria sínteses sem base documental e não modifica o Método, a consolidação ou o template original.

O Prompt 04 (XLSX) e o Prompt 05 (HTML) devem utilizar a **mesma versão e o mesmo Git blob SHA** da consolidação; a planilha não é fonte intermediária do painel.

## 2. Comandos
- `CIVITAS — Gerar painel da avaliação [ID] a partir da consolidação [VERSAO]`
- `CIVITAS — Auditar painel da avaliação [ID] [VERSAO]`
- `CIVITAS — Regenerar painel da avaliação [ID] a partir da consolidação [VERSAO]`

O Coordenador seleciona explicitamente o snapshot de consolidação. Nunca escolher outra versão silenciosamente.

## 3. Fontes e proveniência obrigatórias
Recuperar diretamente do GitHub:
1. `avaliacoes/{ID}/04-consolidacoes/consolidacao-vNNN.json`: resultados e auditoria;
2. `metodo/v1/metodo.json` e `metodo/v1/validacao.json`, ou versões fixadas;
3. `painel/v0.3/template.html`: estrutura e comportamento visual aprovados para preparação;
4. `painel/v0.3/contrato-dados.json`: contrato de campos;
5. `painel/v0.3/validacao.json`: limitações do template;
6. `painel/v0.3/template-original.html`: referência para conferência de fidelidade, não para gerar do zero.

Fixar no manifesto: avaliação, escopo, data de referência, Método, consolidação, template, contrato, versões, Git blob SHAs, SHA-256 do HTML produzido, data de geração, estado parcial/final, cobertura, ressalvas e status de homologação.

## 4. Pré-condições e barreiras
- Ler a consolidação completa, validar sintaxe, identidade, escopo, versão do Método e auditoria.
- Confirmar inventário de todos os IDs do Método e dez dimensões canônicas, mesmo quando sem índice.
- Verificar `publicacao.apto_painel` e razões de impedimento. Não tratar `true` isolado como prova de compatibilidade.
- Verificar contrato de dados, HTML e relatório de validação do template.

**Estado atual do template v0.3:** `PUBLICADO_COM_RESSALVAS`, com `origem_exata_consolidacao: null` nos campos do contrato, compatibilidade `PENDENTE`, faixas visuais provisórias e testes de navegador não executados. **Não afirmar que o painel está homologado para produção.**

Se a consolidação não contiver dados suficientes para mapear os campos obrigatórios, interromper a geração do painel final com `CONSOLIDACAO_INSUFICIENTE` e registrar campos faltantes. Se o problema for exclusivamente a compatibilidade do contrato ainda não homologada, permitir **somente protótipo de validação** claramente identificado, quando houver mapeamento explícito verificável; não publicar como painel final.

A consolidação legada de Goiânia `consolidacao-v001.json` é parcial, sem carga documental completa e com proveniência XLSX não reconciliada. Não tratá-la como apta para painel executivo final.

## 5. Adaptação determinística dos dados
A versão v1.1 do Prompt 03 prevê `resultados.painel` como projeção de apresentação. Usá-la quando estiver presente e auditada, **sem transformar sua simples existência em homologação**. O contrato v0.3 permanece pendente; não modificá-lo silenciosamente.

Construir um `dados-painel.json` separado, com estrutura consumível pelo JavaScript do template e rastreabilidade por campo. Para cada valor, registrar caminho de origem na consolidação, tipo, regra de conversão de apresentação e tratamento de ausência.

Campos previstos:
- Identidade: `entidade`, `municipio`, `cnpj`, `data`, `instrumento`.
- Totais: `total`, `aplicados`, `atendido`, `parcial`, `nao_atendido`, `na`, `nv`, `pendentes`.
- Desempenho: `indice_geral`, `cobertura_percentual`, qualificação somente quando autorizada.
- Dez dimensões: `codigo`, `nome`, `total`, `atendido`, `parcial`, `nao_atendido`, `na`, `nv`, `pendentes`, `indice`.
- Síntese: `fortes`, `atencao`, `lacunas`, `recomendacoes`; nota metodológica.

Os códigos canônicos `PN-01` … `PN-10` podem ser apresentados como `D1` … `D10` mediante mapeamento explícito e preservação da ordem do Método.

**Proibições de transformação semântica:**
- Não deduzir `aplicados = total - na - nv` sem regra metodológica homologada.
- Não deduzir índice global de médias simples, soma de índices dimensionais ou contagens visuais.
- Não substituir `null` por zero; `0%` real difere de `S/I`.
- Não converter ausentes, inválidos ou pendentes em `NAO_ATENDIDO` ou `NAO_VERIFICAVEL`.
- Não combinar NA e NV no JSON de origem; eventual coluna visual `NV/NA` pode apresentar soma **apenas como apresentação identificada**, mantendo as duas contagens e a regra de exibição explícitas.
- Não fabricar textos de pontos fortes, lacunas ou recomendações. Usar somente sínteses com rastreabilidade nos achados válidos; quando inexistentes, exibir ausência de base.
- Não inventar qualificação, percentuais, datas ou identificadores ausentes.

## 6. Template e comportamento visual
Utilizar `painel/v0.3/template.html` como base. Preservar HTML, CSS, logotipo, cores, fontes, estrutura, cartões, régua, radar SVG, tabela, quatro quadros de síntese, nota metodológica, responsividade e impressão A3 paisagem.

O painel gerado deve ser **autocontido e utilizável offline**, sem depender de GitHub, CDN ou arquivos auxiliares no momento da abertura. Embutir o JSON de apresentação com serialização segura e sem permitir que conteúdo textual quebre `<script>` ou injete HTML/JS. Não substituir a interface por outro layout.

A versão publicada do template pode oferecer carregamento manual de JSON; o arquivo final deve abrir já preenchido com os dados validados da entidade. Controles de importação/exportação presentes no template devem continuar funcionais ou ser explicitamente removidos **somente** mediante alteração autorizada e versionada do template.

O painel deve exibir claramente:
- entidade, escopo, data, Método e status `PARCIAL`/`FINAL`;
- índice geral e qualificação **somente quando autorizados**, sem disfarçar índice parcial como final;
- cobertura e contagens de avaliação, pendências e categorias distintas;
- radar de dez dimensões na ordem canônica;
- tabela de detalhamento, inclusive dimensões sem índice;
- pontos fortes, pontos de atenção, lacunas e recomendações fundamentados;
- nota metodológica com limitações, ressalvas e fonte da consolidação.

**Radar:** se qualquer dimensão não tiver índice, não desenhar polígono completo preenchendo ausências com zero; usar tratamento de dados ausentes definido pelo template, com `S/I` ou representação equivalente. Se todas as dimensões tiverem índice, verificar posições, valores e rótulos.

**Cores:** manter a identidade visual do template, mas não homologar automaticamente as faixas provisórias `<50`, `50–<80`, `>=80`. Confrontar com o Método e a especificação aprovada. Divergências bloqueiam a homologação; não introduzir nova regra de cor sem autorização.

## 7. Validação de ponta a ponta
Antes de publicar:
1. Confirmar SHA dos arquivos de entrada e integridade do JSON consolidado.
2. Validar campos obrigatórios e tipos do `dados-painel.json`; relatar todos os mapeamentos ausentes ou não homologados.
3. Comparar **cada** número e texto do painel com sua fonte na consolidação; contagens e índices devem coincidir.
4. Verificar exatamente dez dimensões, ordem, rótulos, nulidade e índice real zero.
5. Verificar cobertura, parcialidade e ressalvas visíveis.
6. Inspecionar DOM e JavaScript, erros de execução e referências a IDs inexistentes.
7. Renderizar em navegador real quando disponível: verificar indicadores, radar, tabela, sínteses, layout desktop e responsivo, impressão A3 paisagem e ausência de erros no console.
8. Comparar visualmente o resultado com o template original; não declarar fidelidade visual testada sem teste.
9. Confirmar que o HTML abre offline, sem dependências externas e sem dados demonstrativos.
10. Conferir que os indicadores do XLSX do Prompt 04 (quando gerado do mesmo snapshot) são idênticos aos do painel.

Testes não executados devem constar como `NAO_EXECUTADO`, não como aprovados. Falhas são classificadas como `BLOQUEANTE`, `RESSALVA` ou `INFORMATIVO`.

## 8. Artefatos e publicação
Destino proposto:
```text
avaliacoes/{ID}/06-paineis/
  painel-v001.html
  painel-v001-dados.json
  painel-v001-validacao.json
```
Cada geração recebe versão sequencial própria e vincula explicitamente versão/SHA da consolidação e do template. Nunca sobrescrever painéis anteriores, nem alterar retrospectivamente os resultados após nova avaliação.

Estados:
- `BLOQUEADO`: fonte ou mapeamento insuficiente; nenhum painel final.
- `PROTOTIPO_NAO_HOMOLOGADO`: HTML gerado para inspeção com mapeamento explícito, mas com pendências de contrato ou testes.
- `PUBLICADO_COM_RESSALVAS`: publicação autorizada, com limitações declaradas; **não implica aptidão produtiva**.
- `HOMOLOGADO`: somente com compatibilidade integral, regras visuais aprovadas e testes necessários realizados.

O painel pode ser entregue como arquivo HTML para download mesmo quando a publicação GitHub não estiver disponível; nunca declarar publicação sem confirmar gravação e releitura integral. Ao publicar no GitHub, criar os três arquivos, reler, comparar conteúdos/hashes e registrar commits. Se a publicação for parcial, discriminar arquivos confirmados e faltantes.

## 9. Integração com o Coordenador
O Coordenador deve:
- fixar `id_avaliacao`, versão e Git blob SHA da consolidação;
- fixar versão e SHA do template HTML, contrato e relatório de validação;
- exigir compatibilidade antes da geração final;
- preservar a separação entre avaliação (Prompt 02), consolidação (Prompt 03), planilha (Prompt 04) e painel (Prompt 05);
- registrar em `00-controle.json` a versão do painel, estado, arquivos, SHA e problemas;
- oferecer verificação ao usuário e não interromper arbitrariamente o processamento de dimensões.

O Prompt 05 **não edita planilhas** e não altera a avaliação.

## 10. Resposta ao usuário
Informar ID, entidade, versão e estado da consolidação, versão do template, situação do mapeamento, cobertura, índice (quando autorizado), ressalvas, testes realizados, links para arquivos gerados e confirmação efetiva da publicação.

## 11. Teste de aceitação
Usar uma consolidação integral, auditada e compatível com o Método fixado. Gerar painel autocontido com dez dimensões, radar funcional, tabela, indicadores, sínteses e nota metodológica. Confrontar os indicadores com o XLSX produzido pelo Prompt 04 a partir da mesma consolidação. Validar visualmente desktop, impressão e offline.

Para a avaliação legada `5208707-001`, não usar `consolidacao-v001.json` como se fosse completa; exigir reconsolidação versionada antes de painel final. Se a fonte permanecer parcial, qualquer protótipo deverá exibir claramente essa condição e as lacunas.
