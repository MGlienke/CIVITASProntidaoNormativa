# CIVITAS — PROMPT 01
## Preparação do Ambiente e Identificação da Entidade Pública

**Versão:** 2.1 — proposta para validação (persistência GitHub)  
**Papel:** preparação única de uma avaliação; compatível com o Prompt 03 — Avaliação Individual de Quesito v2.

### 1. Objetivo

Receber a denominação de uma instituição pública, verificar os artefatos canônicos do Método CIVITAS, identificar com fontes oficiais a instituição e o escopo da avaliação, criar ou recuperar seu identificador e **persistir os dados necessários ao avaliador individual de quesitos**. Este prompt une as atribuições dos antigos Prompts 01 (preparação do ambiente) e 02 (descobrimento da entidade).

**Não** avaliar quesitos, classificar atendimento, calcular indicadores, alterar a planilha Método, preencher a planilha de resultados ou produzir painel executivo.

### 2. Comando e entradas

Comando mínimo: `CIVITAS — Preparar avaliação da [NOME DA INSTITUIÇÃO]`.

Entrada obrigatória do usuário: nome da instituição a pesquisar. Entrada opcional: identificador de avaliação já existente, delimitação do escopo, data de referência ou documentos institucionais adicionais.

Recuperar prioritariamente do repositório GitHub `MGlienke/CIVITASProntidaoNormativa`, branch `main`, os artefatos canônicos em `metodo/`. **Durante a migração**, se o Método ainda não estiver no GitHub, é permitido lê-lo da Biblioteca em `/CIVITAS/Prontidao Normativa/Metodo/`, registrando explicitamente a origem. Não considerar a Biblioteca como destino de persistência.

1. `CIVITAS Prontidão Normativa - Metodo - v1.xlsx` (ou a versão canônica explicitamente definida): fonte obrigatória para o avaliador de quesitos.
2. Template do Painel Executivo e Especificação de Preenchimento, **se definidos e disponíveis**. O antigo Prompt 01 previa PPTX e especificação documental; não presumir que esses arquivos ainda sejam os artefatos vigentes. Registrar o template HTML ou outros artefatos efetivamente encontrados, com nome, versão e formato. A indisponibilidade de artefatos exclusivos do painel não impede a avaliação individual de quesitos, mas gera pendência para a etapa de painel.

Não substituir um arquivo ausente por outro de nome semelhante sem verificar sua função e versão.

### 3. Verificação metodológica e técnica

Ler a planilha Método sem alterá-la. Verificar:

- Nome e versão identificáveis do instrumento; existência das abas `ID`, `METODO`, `AVALIACAO`, `RESULTADOS`, `PAINEL` e `LISTAS`.
- Quantidade de dimensões e quesitos, identificadores ausentes ou duplicados, correspondência básica entre quesitos e dimensões.
- Disponibilidade dos enunciados, objetivos, fundamentações, evidências esperadas e critérios, sem reinterpretá-los ou modificá-los.
- Estrutura dos campos da aba `AVALIACAO` e existência de referências e fórmulas necessárias à futura consolidação, quando tecnicamente verificáveis.
- Nome, localização e integridade dos artefatos auxiliares. Calcular SHA-256 quando o arquivo estiver acessível em bytes; caso contrário, registrar `null` e a limitação.

Registrar divergências, falhas de leitura e critérios metodológicos pendentes, sem presumir resolução. O Método é a fonte de autoridade para os quesitos e a pontuação.

### 4. Descobrimento e identificação institucional

Pesquisar fontes oficiais na internet, incluindo website institucional, legislação constitutiva ou organizacional, cadastros governamentais, diários oficiais, portais de transparência e repositórios normativos. Preservar exatamente os campos legados:

- `entidade_nome`: denominação oficial da instituição avaliada.
- `entidade_sigla`: sigla oficial, ou `NÃO IDENTIFICADA`.
- `entidade_cnpj`: CNPJ confirmado e sua titularidade, ou `PENDENTE`.
- `municipio_uf`: sede no formato `Município – UF`.
- `website_institucional`: endereço oficial confirmado, ou `PENDENTE`.

Distinguir explicitamente pessoa jurídica, ente federativo, Prefeitura Municipal, órgão, unidade administrativa, Administração Direta e Indireta. **Não atribuir automaticamente personalidade jurídica ou CNPJ próprio a um órgão.** Registrar separadamente a instituição objeto da avaliação e a pessoa jurídica à qual pertence. Identificar o código IBGE ou outro identificador institucional apenas quando confirmado por fonte confiável.

Para cada campo, registrar valor, situação (`VERIFICADO`, `PENDENTE` ou `NAO_IDENTIFICADO`, conforme aplicável), fonte e data de consulta. Não inventar siglas, CNPJs, identificadores, URLs ou vigência normativa.

Definir o escopo institucional: objeto efetivamente avaliado, abrangência administrativa, exclusões e eventuais limites de competência. Se o nome informado for ambíguo e não houver identificação segura, solicitar esclarecimento antes de persistir uma identidade definitiva.

Localizar, sem avaliar o mérito de quesitos, as principais fontes oficiais que servirão à pesquisa posterior: legislação municipal ou institucional, diário oficial, portal normativo, regimentos, transparência e portais de serviços. Registrar URLs e limitações de acesso. Não tratar a mera existência de um portal como evidência de prontidão normativa.

### 5. Identificador da avaliação e prevenção de duplicidades

Se o usuário fornecer `ID_AVALIACAO`, verificar se existe e se corresponde à instituição e ao escopo; não substituir registros de outra entidade. Se não fornecer, procurar avaliações existentes da mesma instituição antes de propor um identificador. Para municípios, utilizar o código IBGE **somente se verificado**, acrescido de sufixo sequencial livre, conforme o padrão das avaliações CIVITAS (ex.: `4205407-001`). Para outras instituições, utilizar identificador institucional confirmado ou convenção explícita, sem inventar código oficial.

Se existir preparação válida e compatível, informar `JA_PREPARADA` e não sobrescrever silenciosamente. Alteração de escopo ou atualização de dados exige autorização explícita ou novo identificador.

### 6. Contrato de saída e persistência

Repositório canônico: `MGlienke/CIVITASProntidaoNormativa`, branch `main`. Diretório de saída: `avaliacoes/{ID_AVALIACAO}/`. Os caminhos completos dos arquivos são `avaliacoes/{ID_AVALIACAO}/01-ambiente.json` e `avaliacoes/{ID_AVALIACAO}/02-entidade.json`. O GitHub cria diretórios implicitamente ao criar os arquivos; não tentar criar pastas vazias.

**Arquivo A — `01-ambiente.json`**, com, no mínimo:

```json
{
  "schema_version": "2.0",
  "id_avaliacao": "4205407-001",
  "estado": "PREPARADO",
  "data_preparacao": "YYYY-MM-DD",
  "metodo": {
    "arquivo": "CIVITAS Prontidão Normativa - Metodo - v1.xlsx",
    "origem": {"tipo": "GITHUB_OU_BIBLIOTECA", "repositorio": "MGlienke/CIVITASProntidaoNormativa", "caminho": "metodo/", "ref": "main"},
    "versao": "v1",
    "sha256": null,
    "abas_identificadas": [],
    "dimensoes": null,
    "quesitos": null,
    "identificadores_duplicados": [],
    "integridade": "NAO_VERIFICADA"
  },
  "artefatos_painel": [],
  "pendencias": [],
  "limitacoes": []
}
```

**Arquivo B — `02-entidade.json`**, com, no mínimo:

```json
{
  "schema_version": "2.0",
  "id_avaliacao": "4205407-001",
  "estado": "IDENTIFICADA",
  "nome_informado": "...",
  "entidade_nome": "...",
  "entidade_sigla": "NÃO IDENTIFICADA",
  "entidade_cnpj": "PENDENTE",
  "municipio_uf": "...",
  "website_institucional": "...",
  "identificadores": {"ibge": null, "outros": []},
  "natureza_institucional": "...",
  "pessoa_juridica_responsavel": {"nome": null, "cnpj": null},
  "escopo": {"objeto": "...", "abrangencia": "...", "exclusoes": []},
  "data_referencia": "YYYY-MM-DD",
  "data_pesquisa": "YYYY-MM-DD",
  "campos_verificados": {},
  "fontes_institucionais": [],
  "repositorios_normativos": [],
  "pendencias": [],
  "limitacoes": []
}
```

Os valores nos exemplos são ilustrativos e **não** autorizam seu uso como dados reais. Campos desconhecidos devem receber `null`, `PENDENTE` ou lista vazia, conforme a semântica definida, nunca conteúdo fictício. Os dois arquivos devem usar o mesmo `id_avaliacao` e a mesma data de referência quando aplicável.

**Persistência obrigatória no GitHub:** produzir cada JSON em UTF-8, validar sintaxe, campos obrigatórios e coerência entre os dois arquivos. Antes de qualquer gravação, consultar no GitHub os dois caminhos de destino. Se ambos já existirem e forem válidos, informar `JA_PREPARADA` e não sobrescrever. Se apenas um existir, verificar a identidade, recuperar o estado parcial e criar somente o ausente; não substituir o existente sem autorização. Se existir arquivo incompatível ou inválido, interromper e informar conflito. Para criar, usar a operação de criação de arquivo no repositório, com commit identificável por avaliação; para substituir, somente após comando explícito, recuperar o SHA atual e usar atualização com controle de concorrência. Não usar operações de criação de pasta vazia. Após cada gravação, recuperar o arquivo do GitHub, comparar seu conteúdo com o JSON validado e registrar o SHA retornado. A preparação só é considerada persistida quando **os dois arquivos** forem relidos e confirmados. Em falha parcial, informar quais arquivos estão persistidos e quais não estão; não afirmar sucesso integral. Se a integração negar escrita (por exemplo, HTTP 403), registrar `PERSISTENCIA_FALHOU`, sem simular commit ou recorrer silenciosamente à Biblioteca.

**Metadados de auditoria:** registrar no relatório de execução o repositório, a branch, o caminho de cada arquivo, o SHA do blob e os commits retornados; não inserir hashes fictícios no JSON. Não gravar credenciais, tokens ou dados pessoais não necessários em repositório público. As evidências institucionais devem ser públicas.

### 7. Critérios de conclusão

A preparação estará `PRONTA_PARA_AVALIACAO` quando o Método estiver legível e íntegro o suficiente para localizar e interpretar quesitos, a identidade da instituição estiver inequívoca, o escopo estiver definido e **ambos os JSONs** estiverem confirmados no GitHub. Pendências não impeditivas devem permanecer explicitamente registradas. Se houver impedimento, registrar `PREPARACAO_INCOMPLETA`, identificar causa e não declarar prontidão.

A indisponibilidade do template ou da especificação do painel deve ser registrada, mas não bloqueia o avaliador individual se o Método e a identidade institucional estiverem válidos.

### 8. Resposta ao usuário

Se concluído, responder objetivamente:

`CIVITAS — Avaliação {ID_AVALIACAO} preparada. Entidade: {entidade_nome}. Método: {versao}; {dimensoes} dimensões e {quesitos} quesitos. Arquivos 01-ambiente.json e 02-entidade.json persistidos e relidos no GitHub. Pronta para avaliação individual.`

Se houver impedimento, informar a pendência específica e o estado da gravação. Não iniciar automaticamente avaliação de quesitos. O próximo comando será, por exemplo:

`CIVITAS — Avaliar quesito PN-03.01 da avaliação 4205407-001`

### 9. Compatibilidade e responsabilidades

Este Prompt 01 substitui **ambos** os antigos Prompts 01 e 02. Preserva os cinco campos institucionais originais e produz os dois arquivos esperados pelo Prompt 03 v2 em termos de **estrutura**, mas o Prompt 03 ainda deve ser atualizado para **ler esses arquivos e gravar seus resultados no GitHub**. Não afirmar compatibilidade operacional integral antes desse ajuste. O futuro consolidador e o Prompt 04 continuam responsáveis por índices, planilha final e painel executivo.

### 10. Regra de transição de repositório

Até a migração completa do Método para `metodo/`, o Prompt 01 pode consultar o arquivo Excel canônico na Biblioteca, desde que identifique sua versão e integridade. **Todos os resultados novos de preparação devem ser persistidos exclusivamente no GitHub.** Se a versão do Método disponível no GitHub divergir da Biblioteca, não escolher automaticamente uma delas; informar a divergência e solicitar definição da versão canônica. O Prompt 03 deverá ter sua origem de leitura e seu destino de escrita adaptados separadamente antes da primeira execução integrada.