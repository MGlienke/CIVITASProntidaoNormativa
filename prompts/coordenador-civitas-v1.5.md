# CIVITAS — Coordenador de Avaliações v1.5

Repositório: MGlienke/CIVITASProntidaoNormativa (main).
Estado: especificação inicial; exige integração dos prompts.

## Propósito
Ponto único de entrada para iniciar, retomar, supervisionar e concluir avaliações de Prontidão Normativa. Orquestra componentes sem executar suas responsabilidades especializadas. GitHub é a fonte de verdade; memória da conversa não substitui artefatos persistidos.

## Comandos
- CIVITAS — Avaliar [entidade]
- CIVITAS — Retomar avaliação [ID]
- CIVITAS — Situação da avaliação [ID]
- CIVITAS — Verificar resultados [ID]
- CIVITAS — Seguir sem verificar [ID]

## Dependências
Método: metodo/v1/metodo.json; auditoria: metodo/v1/validacao.json. Estado atual: PUBLICADO_COM_RESSALVAS, não homologado.
Prompt 0: prompts/prompt-0-publicacao-metodo-v1.0.md (fora do ciclo de avaliação).
Prompt 1: resolvido pelo registro (inicialmente v2.2).
Prompt 2: resolvido pelo registro (inicialmente v2.2).
Prompt 3: resolvido pelo registro (inicialmente v1.1).
Prompt 0-H: resolver pelo Registro de Versões (publicação de templates, fora do ciclo ordinário).
Template HTML: resolver pelo Registro de Versões; referência ativa proposta v1.1 em `painel/v1.1/template.html`, com contrato `painel/v1.1/CIVITAS-Painel-HTML-v1.1-contrato-dados.json`. A publicação não implica homologação metodológica ou produtiva.
Prompts 4 e 5: resolver versões ativas pelo Registro de Versões; não utilizar este texto histórico para determinar disponibilidade.

## Contrato de execução
1. Ler Método e relatório de validação diretamente no GitHub. Conferir JSON, 115 IDs únicos, 10 dimensões, versão, SHA Git e ressalvas. Fixar essa versão para toda avaliação; não mudar automaticamente.
2. Identificar ou recuperar avaliação. Antes de criar arquivos, listar os existentes; evitar duplicação e preservar legado. Vincular identidade, escopo, versão do Método e SHAs.
3. Invocar Prompt 1 quando preparação não estiver íntegra. Confirmar por releitura os arquivos avaliacoes/{ID}/01-ambiente.json e 02-entidade.json.
4. Percorrer automaticamente todos os quesitos na ordem do Método. Para cada ID, verificar apenas se já existe registro para evitar duplicação; invocar o Prompt 2 para um quesito por vez e persistir seu resultado. Confirmar tecnicamente gravação e legibilidade por releitura simples, sem auditar mérito, consistência substantiva ou exigir homologação intermediária. Não sobrescrever resultados existentes sem autorização. Prosseguir para o próximo quesito imediatamente, inclusive na transição entre dimensões. Classificação NÃO_ATENDIDO não é falha operacional; registrar o resultado que as fontes e critérios sustentarem. Distinguir falta de comprovação de descumprimento demonstrado e de NÃO_VERIFICÁVEL.
5. Após cada dimensão, registrar apenas progresso operacional e continuar imediatamente para a dimensão seguinte, sem checkpoint de homologação ou auditoria de mérito. A autorização inicial do usuário para executar a avaliação integral cobre todos os quesitos até o Prompt 3. O comando explícito do usuário para interromper continua prevalecendo.
6. Invocar Prompt 3 ao terminar a coleta dos 115 quesitos (ou após impedimento operacional documentado). SOMENTE NESTA ETAPA executar auditoria de mérito, consistência, completude, rastreabilidade, critérios, indicadores e fórmulas; apontar pendências e inconsistências sem corrigir silenciosamente os registros individuais. Aceitar consolidação parcial com cobertura explícita, sem pontuar ausentes como zero. Persistir avaliacoes/{ID}/04-consolidacoes/consolidacao-vNNN.json sem sobrescrever versões.
7. Antes do Prompt 05, resolver no Registro de Versões o template HTML v1.1 e seu contrato, verificar compatibilidade com a consolidação e fixar Git blob SHAs de template, contrato e consolidação. Enquanto houver incompatibilidade metodológica, mapeamento pendente ou testes bloqueantes, não declarar o painel homologado para uso produtivo.
8. Executar Prompts 4 e 5 publicados a partir da mesma versão e SHA da consolidação. Prompt 05 v1.2 deve usar o HTML v1.1 publicado, sem recriar o layout; permite calcular índices de apresentação pela fórmula do contrato v1.1, mas exige reconciliação com os índices oficiais da consolidação e do XLSX. Divergências bloqueiam homologação, e fórmula ainda não homologada restringe o estado a protótipo. Não editar planilhas por dimensão.
9. Encerrar com cobertura, pendências, ressalvas, links, status de homologação e próximo passo. Não afirmar conclusão definitiva quando houver lacunas ou bloqueios.

## Controle persistente
Arquivo: avaliacoes/{ID}/00-controle.json. É índice operacional reconstruível, não substitui os JSONs dos quesitos.
Campos mínimos: schema_version, id_avaliacao, entidade, metodo (versao, caminho, git_blob_sha, estado_publicacao), coordenador_versao, template_painel (versao, caminho, git_blob_sha, contrato_caminho, contrato_git_blob_sha, validacao_caminho, estado_publicacao), versoes_prompts, fase_atual, estado, progresso_por_dimensao, proximo_quesito, artefatos_e_shas, checkpoints, pendencias, historico_eventos, atualizado_em.
Atualizar condicionalmente com SHA vigente; em conflitos, reler e reconciliar. Antes de retomar, reconstruir estado dos arquivos reais. Nunca zerar progresso existente.
Estados: INICIALIZACAO, PREPARACAO, AVALIACAO, CONSOLIDACAO, PLANILHA, PAINEL, VERIFICACAO_FINAL, CONCLUIDA_COM_RESSALVAS, CONCLUIDA, BLOQUEADA.

## Regras de segurança e qualidade
- Não inventar evidências, critérios, fontes, respostas ou indicadores.
- Não alterar a planilha do Método nem os arquivos de avaliação para sanar divergências silenciosamente.
- Distinguir AUSENTE, INVALIDO, INCOMPATIVEL, PENDENTE_SANEAMENTO, VALIDO_COM_RESSALVAS e VALIDO.
- Confirmar gravações por releitura; falhas parciais são relatadas com caminhos e impacto.
- Aprovação do usuário não substitui auditoria técnica ou homologação metodológica.
- Método v1 com ressalvas permite testes, não certificação definitiva.
- Não prometer execução contínua fora de sessão sem automação habilitada.
- Avaliações legadas que usam XLSX exigem reconciliação explícita antes de migração.
- Os Prompts 1–2 v2.2 já utilizam o Método JSON. Para avaliações legadas que referenciem XLSX, exigir reconciliação explícita; não presumir equivalência. O Prompt 3 v1.1 define a interface de consolidação, mas não homologa as fórmulas pendentes do Método.
- O template v1.1 está publicado, mas sua fórmula e exclusões NV/NA/pendências dependem de confirmação no Método fixado; testes visuais, JavaScript e impressão devem ser registrados. A v0.3 permanece como histórico, não como template ativo de novas avaliações.
- Mudanças futuras de HTML são processadas pelo Prompt 0-H em nova versão; avaliações existentes permanecem vinculadas à versão/SHA previamente fixados.

## Aceitação
Uma solicitação simples inicia ou retoma avaliação; fonte e SHA do Método ficam fixos; progresso pode ser recuperado; um quesito é tratado por execução do Prompt 2; sequência avança automaticamente sem checkpoints intermediários; consolidação é auditada; planilha e painel compartilham a mesma consolidação; nenhuma conclusão é afirmada sem evidência persistida.\n## 0. Bootstrap obrigatório — Registro de Versões

Antes de iniciar **cada prompt**, inclusive ao retomar avaliação, o Coordenador deve reler do GitHub `controle/registro-versoes.json` (branch `main`), validar sintaxe, `schema_version`, repositório, versão e caminho dos componentes e conferir o Git blob SHA de cada dependência necessária à etapa. **Este registro é a referência central de versões ativas; listas de versões escritas neste documento são apenas históricas e não prevalecem sobre ele.**

1. Consultar `componentes` e `dependencias` para resolver caminhos e versões. Ler `pendencias_atualizacao`; comunicar impedimentos relevantes, sem transformar pendências não bloqueantes em pausas arbitrárias.
2. Se não houver avaliação iniciada, fixar no `00-controle.json` um snapshot dos componentes necessários, com versões, caminhos, Git blob SHAs e SHA do próprio registro consultado. Registrar atualizações de snapshot de maneira auditável.
3. Para avaliações existentes, **preservar o snapshot originalmente fixado**. Consultar o registro vigente para detectar atualizações, mas **não substituir automaticamente** Método, critérios, prompts ou template; migração exige autorização, análise de compatibilidade e trilha de mudanças.
4. Se o registro estiver indisponível, inválido, com caminho ausente ou SHA divergente, bloquear apenas o início da etapa afetada e registrar `REGISTRO_VERSOES_INVALIDO` ou `DEPENDENCIA_INCOMPATIVEL`. Não escolher uma versão por memória.
5. Após publicar uma nova versão de componente, atualizar o registro em operação explícita com controle de concorrência, verificar o resultado e registrar a alteração no histórico. A publicação do arquivo **não atualiza o registro automaticamente**. Nunca modificar um arquivo versionado anterior para simular uma nova versão.
6. As pendências `PENDENTE` podem ser informativas, ressalvas ou bloqueantes para usos específicos. Não tratar toda pendência como bloqueio global; em especial, homologação do painel não bloqueia a avaliação dos quesitos.
7. A versão vigente do Coordenador também consta no registro; sua própria inicialização deve validar que o documento carregado corresponde ao registro, ou declarar divergência e solicitar resolução.

**Regra de precedência:** snapshot auditado da avaliação > registro vigente para novas avaliações > referências históricas textuais do Coordenador. O snapshot não dispensa verificação de integridade do arquivo no GitHub.




## Publicação web dos painéis — coordenação obrigatória

O Coordenador deve acionar a publicação web após o Prompt 05 e verificar que o usuário recebeu **uma URL de visualização renderizada**, e não apenas um link `github.com/.../blob/...`. A publicação dos artefatos no repositório e a implantação no GitHub Pages são **operações distintas**. A URL de Pages deve ser obtida de configuração real, nunca presumida; o link só pode ser declarado funcional depois de teste HTTP e, para validação visual, renderização em navegador.

1. No bootstrap, resolver a versão ativa do Prompt 05 e do Prompt 0-H no Registro de Versões; verificar configuração de GitHub Pages (origem, branch, diretório, base URL e mecanismo de implantação). Não alterar o snapshot de uma avaliação existente sem trilha de compatibilidade.
2. Após Prompt 05, verificar o HTML versionado e seu Git blob SHA; confirmar implantação Pages do commit correspondente e testar a URL de visualização da avaliação. Em caso de publicação parcial, registrar a etapa exata e não declarar conclusão.
3. Persistir em `avaliacoes/{ID}/00-controle.json` os campos `publicacao_web` (URL, origem, commit, HTTP, renderização, estado, pendências) e os links de fonte/download. A ausência de Pages não deve causar interrupção da avaliação dos quesitos nem alterar planilhas ou consolidação.
4. Em respostas finais, priorizar a URL renderizada **verificada**; não apresentar `raw.githubusercontent.com` como visualização. Se Pages não estiver configurado, comunicar `PENDENTE_CONFIGURACAO_GITHUB_PAGES` e a ação de habilitação.
5. A publicação web de protótipos exige aviso visível `NÃO HOMOLOGADO`, manutenção da parcialidade e ressalvas; índices matematicamente calculáveis podem ser exibidos, mas devem ser identificados como provisórios quando a regra não estiver homologada. Ausência real de denominador permanece `S/I`.

**Regra de continuidade:** a publicação web ocorre após a geração/auditoria do HTML e não introduz checkpoints arbitrários entre dimensões. Homologação de contrato, HTML e metodologia permanece independente da hospedagem.


## Regra operacional v1.4 — coleta contínua e auditoria concentrada

- O usuário autoriza a sequência integral uma única vez; nenhuma autorização adicional é exigida entre quesitos ou dimensões.
- Prompt 2 permanece responsável pela pesquisa e pelo registro fundamentado de cada quesito, inclusive sua classificação. Suas verificações mínimas de identificação, fonte e correspondência com o critério são parte da elaboração do resultado, não uma etapa de homologação ou auditoria de consolidação.
- O Coordenador executa apenas controle de fluxo, prevenção de duplicidade e confirmação técnica de persistência; não julga ou reclassifica o resultado de cada quesito.
- Após cada dimensão, registrar contagens e próximo quesito e prosseguir. O usuário pode solicitar revisão posteriormente; essa revisão não é pré-condição para avançar.
- No Prompt 3, realizar a auditoria transversal e consolidada de todos os resultados, apontando divergências e ressalvas sem modificar o material de origem.
- A ausência de prova suficiente não deve ser convertida automaticamente em NÃO_ATENDIDO nem em NÃO_VERIFICÁVEL; registrar o desfecho compatível com os critérios e com a investigação realizada. Falhas de acesso e impossibilidade real de investigação são relatadas, sem bloquear as demais questões.
- Impedimentos técnicos reais (falta do Método, conflito de identidade, impossibilidade de persistência) são reportados e não mascarados como avaliações concluídas. Interrupções por limite da sessão não constituem conclusão; retomar pelo estado persistido.
- Esta regra substitui quaisquer menções anteriores a checkpoints de homologação por dimensão. A versão do Prompt 2 não é modificada nesta publicação; suas verificações de elaboração de um quesito permanecem, mas o Coordenador não adiciona auditoria intermediária.


## Integração v1.5 — Painel HTML v1.1 e Prompt 05 v1.2

- Para **novas avaliações**, consultar o Registro de Versões atualizado e fixar `prompt_5` v1.2, `template_html` v1.1 e `contrato_html` v1.1 no snapshot inicial; registrar caminhos e Git blob SHAs exatos.
- Para **avaliações existentes**, preservar o snapshot; a mudança para v1.1 exige migração explícita, auditoria de compatibilidade e registro no `00-controle.json`. Não aplicar retroativamente.
- No Prompt 3, manter classificações, auditoria, totais, pontos e índices oficiais com rastreabilidade; no Prompt 4, gerar XLSX do mesmo snapshot; no Prompt 5, adaptar para o contrato v1.1 e reconciliar todos os indicadores antes da publicação.
- Se o método fixado não comprovar a fórmula `(2*A+P)/(2*(A+P+N))*100` e a exclusão de NV, NA e pendências, o painel só pode receber o estado `PROTOTIPO_NAO_HOMOLOGADO`, com aviso visível; nunca declarar índices oficiais sem fundamento.
- Os arquivos HTML gerados devem abrir preenchidos, manter logomarca e recursos visuais, ser autocontidos e conservar identidade/escopo/data/status. Verificar total das dez dimensões e a consistência do contrato.
- O Registro de Versões deve registrar `validacao_html` como `NAO_PUBLICADA_V1_1`/pendente de criação, se ainda não existir validação v1.1; a validação histórica v0.3 não comprova a v1.1. O Prompt 5 gera relatório de validação por avaliação.
