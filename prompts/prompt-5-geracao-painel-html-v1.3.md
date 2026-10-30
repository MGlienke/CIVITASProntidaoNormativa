# CIVITAS — PROMPT 05 — Geração do Painel Executivo HTML

**Versão:** 1.3 — contrato institucional v1.2, template visual HTML v1.1
**Repositório:** `MGlienke/CIVITASProntidaoNormativa`, branch `main`
**Dependências:** Coordenador resolvido pelo Registro de Versões, Prompt 03 v1.3, Método JSON fixado, template HTML v1.1.
**Saídas:** painel HTML autocontido por avaliação, dados de apresentação JSON, relatório de validação e manifesto de proveniência.

## 1. Objetivo e limites
Gerar o Painel Gráfico Executivo CIVITAS de Prontidão Normativa **exclusivamente a partir de uma consolidação auditada e versionada**, utilizando a composição visual do template HTML canônico já publicado.

O Prompt 05 é um **adaptador e renderizador**, não um avaliador, consolidador ou calculador alternativo. Não pesquisa normas, não reclassifica quesitos, não altera resultados, não altera os índices oficiais da consolidação, não cria sínteses sem base documental e não modifica o Método, a consolidação ou o template original.

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
3. `painel/v1.1/template.html`: template visual e lógica de cálculo para apresentação;
4. `painel/v1.2/CIVITAS-Painel-HTML-v1.2-contrato-dados.json`: contrato efetivamente publicado;
5. `painel/v1.1/CIVITAS-Painel-HTML-v1.1-README.md`: instruções e ressalvas;
6. `painel/v1.1/CIVITAS-Painel-HTML-v1.1-exemplo-dados.json`: teste estrutural, nunca fonte de avaliação.

Os caminhos acima correspondem aos arquivos existentes na branch `main`. A normalização posterior dos nomes dos arquivos exige nova revisão versionada deste prompt.

Fixar no manifesto: avaliação, escopo, data de referência, Método, consolidação, template, contrato, versões, Git blob SHAs, SHA-256 do HTML produzido, data de geração, estado parcial/final, cobertura, ressalvas e status de homologação.

## 4. Pré-condições e barreiras
- Ler a consolidação completa, validar sintaxe, identidade, escopo, versão do Método e auditoria.
- Confirmar inventário de todos os IDs do Método e dez dimensões canônicas, mesmo quando sem índice.
- Verificar `publicacao.apto_painel` e razões de impedimento. Não tratar `true` isolado como prova de compatibilidade.
- Verificar contrato de dados, HTML e README do template v1.1; gerar relatório de validação específico da execução.

**Estado do template v1.1:** publicado no GitHub, mas sua fórmula e a política de exclusão de NV/NA/pendências ainda precisam de confirmação frente ao Método oficial v1.2; testes de navegador e compatibilidade da consolidação devem ser registrados individualmente. **Publicação do arquivo não equivale a homologação metodológica ou produtiva.**

Se a consolidação não contiver dados suficientes para mapear os campos obrigatórios, interromper a geração do painel final com `CONSOLIDACAO_INSUFICIENTE` e registrar campos faltantes. Se o problema for exclusivamente a compatibilidade do contrato ainda não homologada, permitir **somente protótipo de validação** claramente identificado, quando houver mapeamento explícito verificável; não publicar como painel final.

A consolidação legada de Goiânia `consolidacao-v001.json` é parcial, sem carga documental completa e com proveniência XLSX não reconciliada. Não tratá-la como apta para painel executivo final.

## 5. Adaptação determinística dos dados
A versão v1.3 do Prompt 03 prevê `resultados.painel` como projeção de apresentação. Usá-la quando estiver presente e auditada, **sem transformar sua simples existência em homologação**. O contrato de integração v1.2 é a referência de entrada do template visual v1.1; verificar explicitamente sua compatibilidade com a consolidação e o Método fixados.

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

## 5-A. Mapeamento de CNPJ, data e síntese executiva

O Prompt 05 **não consulta cadastros, não define data de avaliação e não cria conclusões**. Consumir exclusivamente o snapshot de `resultados.painel` do Prompt 03 v1.3:
- `cnpj`: string de 14 dígitos apenas quando `proveniencia_campos.cnpj` confirmar `VERIFICADO`, titularidade e vínculo institucional; formatar na interface como `00.000.000/0000-00` se o template suportar, sem modificar o JSON canônico. Se `null`, apresentar `Não informado — verificação pendente`, com ressalva; nunca substituir por CNPJ pesquisado no Prompt 05.
- `data`: data ISO `YYYY-MM-DD` de referência da avaliação, transportada de `contexto.data_referencia`; exibir em `DD/MM/AAAA` conforme template, sem utilizar data de geração.
- `sintese`: quatro arrays de strings da projeção auditada do Prompt 03, com referência aos quesitos. Não criar nem resumir livremente novos textos. Se um cartão não tiver conclusões fundamentadas, exibir `Sem base suficiente nos quesitos válidos`, e não um texto inventado.
- `proveniencia_campos`: conservar no manifesto/JSON de validação, não necessariamente exibir no HTML.

**Barreiras:** CNPJ contraditório, data de referência divergente ou item de síntese sem suporte bloqueiam a publicação como painel final; CNPJ apenas ausente permite protótipo com ressalva. O contrato v1.2 é de integração; a apresentação permanece no template v1.1, que deve ser validado quanto à aceitação do payload.

## 6. Template e comportamento visual
Utilizar `painel/v1.1/template.html` como base. Preservar HTML, CSS, logotipo, cores, fontes, estrutura, cartões, régua, radar SVG, tabela, quatro quadros de síntese, nota metodológica, responsividade e impressão A3 paisagem.

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

**Cores:** manter a identidade visual do template v1.1. Confirmar as faixas do índice e sua correspondência com a especificação aprovada antes de homologar; divergências bloqueiam a homologação, sem alterar a identidade visual de modo silencioso.

## 6-A. Cálculo, reconciliação e contrato de integração v1.2

1. A consolidação auditada continua sendo a fonte autoritativa de **classificações, justificativas, escopo e estado**. O HTML não pode mudar notas nem classificações.
2. O template v1.1 implementa o cálculo de apresentação: `pontos = 2 × atendido + parcial`; `máximo = 2 × (atendido + parcial + nao_atendido)`; `índice = 100 × pontos / máximo`. Quando o denominador for zero, exibir `S/I` (nunca zero fictício). O contrato v1.2 mantém a regra de pontuação do v1.1 e exclui `nv`, `na` e `criterio_pendente` do denominador. **Essa regra é provisória até confronto com o Método v1.2 homologado.**
3. Antes da geração final, verificar se a versão do Método efetivamente fixada **autoriza a fórmula e as exclusões**. Se não for possível comprovar, gerar somente `PROTOTIPO_NAO_HOMOLOGADO` com ressalva visível; não suprimir índices calculáveis, mas não chamá-los oficiais.
4. Se a consolidação contiver índices homologados, comparar índice geral e **cada uma das dez dimensões**, com tolerância limitada ao arredondamento de exibição. Divergências devem constar na validação e **bloquear a publicação como HOMOLOGADO**. Não escolher silenciosamente um dos valores.
5. O Prompt 04 e o Prompt 05 devem reconciliar pontos, denominadores e índices sobre o **mesmo snapshot** de consolidação e o mesmo Método. A planilha não é fonte de dados do painel.
6. Mapear `criterio_pendente` do contrato de integração v1.2 a uma contagem **explicitamente comprovada** na consolidação; não presumir equivalência com qualquer outro campo chamado `pendentes`. Manter `nv` e `na` distintos. Preservar os dez nomes e códigos de dimensões da fonte, sem inventar rótulos.
7. A soma `atendido + parcial + nao_atendido + nv + na + criterio_pendente` deve igualar `total` em cada dimensão, e a soma das dez dimensões deve igualar o total geral. Em caso de inconsistência, bloquear a geração final e registrar a diferença.
8. O HTML final deve **abrir com os dados embutidos**, sem depender de carregamento manual de JSON. A funcionalidade de importação pode permanecer para testes, mas não substitui a geração do painel preenchido. Não transportar dados reais para o template canônico.
9. As sínteses devem provir da consolidação auditada; não produzir recomendações, pontos fortes ou lacunas inventadas. Na ausência de base, indicar `SEM_BASE_DOCUMENTAL`.
10. Registrar no manifesto os Git blob SHAs do template v1.1 e do contrato, o snapshot da consolidação, fórmula efetivamente aplicada, arredondamento, comparação de índices, resultado dos testes e ressalvas metodológicas.

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


## 12. Publicação web e verificação em navegador (GitHub Pages)

**Obrigação:** após gerar, auditar e persistir o HTML versionado, disponibilizar uma **URL de visualização renderizada** no GitHub Pages, além do link de código-fonte GitHub e do HTML para download. Um endereço `github.com/.../blob/...` ou `raw.githubusercontent.com/...` **não é prova de visualização no navegador**.

1. Consultar o registro de versões e a configuração de publicação web mantida pelo Coordenador/Prompt 0-H. Confirmar que GitHub Pages está habilitado para o repositório, com origem, branch, diretório e URL base reais; **não presumir** que `https://mglienke.github.io/CIVITASProntidaoNormativa/` esteja ativo.
2. Publicar o HTML **no caminho versionado da avaliação**, preservando o arquivo original do template e as versões anteriores. Se a configuração Pages publicar `main` na raiz, a URL candidata é `{base_url_pages}/avaliacoes/{ID}/06-paineis/{arquivo_html}`; somente tratá-la como URL pública depois de verificação HTTP e renderização. Para publicação por GitHub Actions, aguardar a implantação correspondente e registrar o commit publicado.
3. Verificar que a URL pública responde com sucesso, serve HTML (não página 404, tela de código ou download) e exibe a entidade, a faixa de estado, os indicadores e as dez dimensões. Inspecionar JavaScript/DOM, radar, tabela e console em navegador real quando disponível. Não equiparar sucesso HTTP a teste visual completo.
4. Registrar no manifesto e em `painel-vNNN-validacao.json`: `publicacao_web` com `provedor`, `configuracao_verificada`, `origem`, `branch`, `base_url`, `url_visualizacao`, `commit`, `git_blob_sha_html`, `http_status`, `content_type`, `teste_renderizacao`, `estado` e `falhas`. Valores não observados são `null`/`NAO_VERIFICADO`, nunca dados presumidos.
5. Estados distintos: `HTML_GERADO`, `GITHUB_ARQUIVO_PUBLICADO`, `PAGES_CONFIGURADO`, `PAGES_IMPLANTADO`, `URL_HTTP_VALIDADA`, `RENDERIZACAO_VALIDADA`. Não usar `PUBLICACAO_WEB_CONCLUIDA` sem implantação e URL verificadas.
6. Se Pages não estiver configurado ou a conexão não permitir habilitá-lo, registrar `PENDENTE_CONFIGURACAO_GITHUB_PAGES` e fornecer instruções exatas para habilitação; **não bloquear a geração local, a auditoria e a publicação versionada do HTML**, mas não declarar a entrega web concluída.
7. Protótipos parciais podem ser publicados na web **somente com identificação visível de PROTÓTIPO NÃO HOMOLOGADO**, sem índices artificiais e com ressalvas. Publicação web não é homologação metodológica ou visual.
8. Após publicar, fornecer prioritariamente o link renderizado validado; em separado, link de código e download HTML. Se o link renderizado não foi confirmado, não apresentá-lo como funcional.

**Teste de aceitação adicional:** o usuário deve conseguir abrir a URL pública em Chrome/Edge e visualizar o painel renderizado, sem acessar editor de código, sem importar JSON manualmente e sem dependências externas. A verificação é registrada e reprodutível.
