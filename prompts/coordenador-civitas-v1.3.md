# CIVITAS — Coordenador de Avaliações v1.3

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
Prompt 0-H: prompts/prompt-0-html-painel-v1.0.md (publicação de templates, fora do ciclo ordinário).
Template HTML canônico: painel/v0.3/template.html; original: painel/v0.3/template-original.html; contrato: painel/v0.3/contrato-dados.json; auditoria: painel/v0.3/validacao.json. Estado PUBLICADO_COM_RESSALVAS, não homologado para uso produtivo.
Prompts 4 e 5: resolver versões ativas pelo Registro de Versões; não utilizar este texto histórico para determinar disponibilidade.

## Contrato de execução
1. Ler Método e relatório de validação diretamente no GitHub. Conferir JSON, 115 IDs únicos, 10 dimensões, versão, SHA Git e ressalvas. Fixar essa versão para toda avaliação; não mudar automaticamente.
2. Identificar ou recuperar avaliação. Antes de criar arquivos, listar os existentes; evitar duplicação e preservar legado. Vincular identidade, escopo, versão do Método e SHAs.
3. Invocar Prompt 1 quando preparação não estiver íntegra. Confirmar por releitura os arquivos avaliacoes/{ID}/01-ambiente.json e 02-entidade.json.
4. Percorrer todos os quesitos na ordem do Método. Para cada ID, conferir existência, integridade e compatibilidade do arquivo avaliacoes/{ID}/03-quesitos/Dxx/PN-xx.yy.json. Invocar Prompt 2 para **um único quesito** ausente por execução; reler arquivo e validar. Não sobrescrever resultados existentes ou corrigir conteúdo sem autorização.
5. Após cada dimensão, auditar progresso e perguntar explicitamente: **Verificar resultados** ou **Seguir sem verificar**. A segunda opção aprova operacionalmente com ressalvas e autoriza começar imediatamente a próxima dimensão. Checkpoints técnicos internos não interrompem o fluxo. Falta de resposta não implica autorização.
6. Invocar Prompt 3 após a sequência possível. Aceitar consolidação parcial com cobertura explícita, sem pontuar ausentes como zero. Conferir inventário, justificativas, fontes, indicadores, fórmulas, cargas documentais completas e persistência de avaliacoes/{ID}/04-consolidacoes/consolidacao-vNNN.json. Nunca sobrescrever versões.
7. Antes do Prompt 05, ler o contrato e validação do template HTML v0.3, verificar a compatibilidade do consolidado com o contrato de dados e fixar os Git blob SHA do template, contrato e consolidação. Enquanto o contrato tiver mapeamentos pendentes ou ressalvas bloqueantes, não declarar o painel apto a uso produtivo.
8. Somente quando publicados, executar Prompts 4 e 5 a partir da mesma versão e SHA da consolidação. Prompt 05 deve utilizar o HTML parametrizado publicado, sem recriar o layout e sem recálculos independentes; não editar planilhas por dimensão.
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
- O template v0.3 está publicado com ressalvas: mapeamento do contrato pendente, cores provisórias e testes visuais/JavaScript não executados. O Coordenador deve manter tais ressalvas explícitas até saneamento e testes.
- Mudanças futuras de HTML são processadas pelo Prompt 0-H em nova versão; avaliações existentes permanecem vinculadas à versão/SHA previamente fixados.

## Aceitação
Uma solicitação simples inicia ou retoma avaliação; fonte e SHA do Método ficam fixos; progresso pode ser recuperado; um quesito é tratado por execução do Prompt 2; sequência avança por dimensão; checkpoints oferecem as duas opções; consolidação é auditada; planilha e painel compartilham a mesma consolidação; nenhuma conclusão é afirmada sem evidência persistida.## 0. Bootstrap obrigatório — Registro de Versões

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
5. A publicação web de protótipos exige aviso visível `NÃO HOMOLOGADO` e manutenção da parcialidade, das ressalvas e dos índices nulos.

**Regra de continuidade:** a publicação web ocorre após a geração/auditoria do HTML e não introduz checkpoints arbitrários entre dimensões. Homologação de contrato, HTML e metodologia permanece independente da hospedagem.
