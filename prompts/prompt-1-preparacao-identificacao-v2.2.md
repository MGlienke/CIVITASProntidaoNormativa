# CIVITAS — PROMPT 01
## Preparação do Ambiente e Identificação da Entidade Pública

**Versão:** 2.2 — proposta para validação (persistência GitHub)  
**Papel:** preparação única de uma avaliação; compatível com o Prompt 02 — Avaliação Individual de Quesito v2.2.

### 1. Objetivo

Receber a denominação de uma instituição pública, verificar os artefatos canônicos do Método CIVITAS, identificar com fontes oficiais a instituição e o escopo da avaliação, criar ou recuperar seu identificador e **persistir os dados necessários ao avaliador individual de quesitos**. Este prompt une as atribuições dos antigos Prompts 01 (preparação do ambiente) e 02 (descobrimento da entidade).

**Não** avaliar quesitos, classificar atendimento, calcular indicadores, alterar a planilha Método, preencher a planilha de resultados ou produzir painel executivo.

### 2. Comando e entradas

Comando mínimo: `CIVITAS — Preparar avaliação da [NOME DA INSTITUIÇÃO]`.

Entrada obrigatória do usuário: nome da instituição a pesquisar. Entrada opcional: identificador de avaliação já existente, delimitação do escopo, data de referência ou documentos institucionais adicionais.

Recuperar **obrigatoriamente** do GitHub `MGlienke/CIVITASProntidaoNormativa`:
1. `metodo/v1/metodo.json` e `metodo/v1/validacao.json`, ou a versão explicitamente selecionada pelo Coordenador.
2. `painel/v0.3/template.html`, `painel/v0.3/contrato-dados.json` e `painel/v0.3/validacao.json` apenas para registrar dependências futuras, sem bloquear a avaliação de quesitos.

O Método canônico **já está publicado em JSON**. Não usar XLSX da Biblioteca como fallback, não reconstruir o Método por memória e não mudar a versão automaticamente. O XLSX de origem é apenas proveniência do Prompt 0, nunca a fonte operacional do Prompt 01.

### 3. Verificação metodológica e técnica

Ler o JSON integralmente, confirmar `tipo_artefato=CIVITAS_METODO`, `versao_metodo`, `estado_publicacao`, sintaxe, SHA de blob GitHub, 115 IDs únicos e dez dimensões; conferir cada `id_quesito` com sua dimensão e as chaves `enunciado`, `objetivo`, `fundamentacao_conceitual`, `relacoes_semanticas`, `fundamentacao_normativa_metodo`, `evidencias_esperadas`, `objeto_evidenciado_metodo`, `regra_na`, `situacao_metodologica` e `criterios` (0/1/2).

Registrar `PUBLICADO_COM_RESSALVAS` do Método e ressalvas da validação; não afirmar homologação. `proveniencia.sha256_xlsx` é hash do arquivo Excel de origem e **não** substitui `git_blob_sha` do JSON. Não interpretar fórmulas `NAO_HOMOLOGADA` como regras aprovadas.

Registrar as referências do template HTML v0.3 e sua condição `PUBLICADO_COM_RESSALVAS`; a falta de homologação do painel não impede preparação e avaliação.

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
    "arquivo": "metodo/v1/metodo.json",
    "versao": "v1",
    "git_blob_sha": null,
    "estado_publicacao": "PUBLICADO_COM_RESSALVAS",
    "validacao_arquivo": "metodo/v1/validacao.json",
    "validacao_git_blob_sha": null,
    "sha256_xlsx_origem": null,
    "dimensoes": 10,
    "quesitos": 115,
    "identificadores_duplicados": [],
    "integridade": "VERIFICADA_COM_RESSALVAS"
  },
  "artefatos_painel": [{"versao": "v0.3", "template": "painel/v0.3/template.html", "contrato": "painel/v0.3/contrato-dados.json", "validacao": "painel/v0.3/validacao.json", "estado": "PUBLICADO_COM_RESSALVAS"}],
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

Se houver impedimento, informar a pendência específica e o estado da gravação. Quando invocado isoladamente, não iniciar automaticamente avaliação de quesitos. Quando invocado pelo Coordenador, devolver controle para que ele continue a sequência sem nova confirmação técnica. O próximo comando será, por exemplo:

`CIVITAS — Avaliar quesito PN-03.01 da avaliação 4205407-001`

### 9. Compatibilidade e responsabilidades

O Prompt 01 v2.2 prepara `01-ambiente.json` e `02-entidade.json` para o Prompt 02 v2.2 e Prompt 03 v1.1. O Método é exclusivamente o JSON canônico fixado; a planilha Excel não é consultada durante a avaliação. O Coordenador v1.1 é responsável pela continuidade automática do fluxo e pelos checkpoints de dimensão.

**Avaliações legadas:** se o `01-ambiente.json` existente referir apenas o XLSX, não sobrescrever nem supor equivalência. Comparar identidade, escopo, versão e proveniência, registrar `MIGRACAO_METODO_PENDENTE` e solicitar autorização para uma atualização controlada, com SHA de concorrência e trilha de auditoria. Sem reconciliação, não declarar a avaliação preparada para o novo fluxo.

### 10. Teste de aceitação

Preparar nova entidade e conferir que o ambiente referencia `metodo/v1/metodo.json` com Git blob SHA real, 115 quesitos e dez dimensões. Verificar que os arquivos são relidos do GitHub e que nenhuma planilha XLSX foi aberta ou modificada. Em avaliação legada, detectar referência XLSX e não migrar silenciosamente.
