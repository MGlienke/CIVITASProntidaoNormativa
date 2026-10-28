# CIVITAS — PROMPT 02 — Avaliação Individual de Quesito

**Versão:** 2.2 — GitHub (proposta para validação)  
**Origem:** antigo Prompt 03 v2.0, renumerado como Prompt 02.  
**Repositório canônico:** `MGlienke/CIVITASProntidaoNormativa`, branch `main`.

## 1. Objetivo e limites
Avaliar **exatamente um quesito** da Avaliação de Prontidão Normativa CIVITAS, identificado por `ID_QUESITO`, no contexto institucional preparado pelo **Prompt 01 v2.2**. Investigar fontes oficiais, confrontar evidências com os critérios literais do Método, classificar, auditar e persistir **um JSON autossuficiente no GitHub**.

**Não** avaliar outros quesitos, modificar planilhas Excel, atualizar arquivos de controle globais, consolidar dimensões, calcular índices ou gerar painel. Uma execução não depende de resultados de outros quesitos.

## 2. Comando e entradas
Comando: `CIVITAS — Avaliar quesito [ID_QUESITO] da avaliação [ID_AVALIACAO]`.  
Exemplo: `CIVITAS — Avaliar quesito PN-03.01 da avaliação 5208707-001`.

Antes de avaliar, recuperar **diretamente do GitHub**:
- `avaliacoes/{ID_AVALIACAO}/01-ambiente.json`;
- `avaliacoes/{ID_AVALIACAO}/02-entidade.json`.

Conferir sintaxe, `schema_version`, identidade e coincidência do `id_avaliacao`, escopo, datas, referência metodológica, pendências impeditivas e coerência entre os dois arquivos. O primeiro é obrigatório nesta versão para identificar a fonte do Método; o segundo contém a identidade e o escopo institucionais. Se estiverem ausentes, incompatíveis ou não puderem ser lidos, interromper sem classificar.

Recuperar o **Método JSON canônico** diretamente do GitHub: `metodo/v1/metodo.json` e `metodo/v1/validacao.json`, ou versão explicitamente fixada em `01-ambiente.json` pelo Coordenador. Conferir versão e Git blob SHA; se o ambiente não tiver referência verificável, registrar `MIGRACAO_METODO_PENDENTE` e interromper sem classificação. **Não há fallback para XLSX ou Biblioteca.**

Localizar `ID_QUESITO` por correspondência exata e única em `metodo.quesitos[].id_quesito`; conferir dimensão por prefixo `PN-xx` e ordem em `metodo.dimensoes`, bloco, enunciado, objetivo, fundamentações, relações, evidências, objeto evidenciado, regra NA, situação metodológica e critérios literais `criterios["0"]`, `criterios["1"]`, `criterios["2"]`. O JSON usa `dimensao` como **nome descritivo** no quesito, enquanto `metodo.dimensoes[].codigo` usa `PN-01` etc.; o caminho operacional `D01` deriva do prefixo, não de suposto campo `D01` no Método.

Registrar que o Método v1 está `PUBLICADO_COM_RESSALVAS` e que fórmulas de resultados `NAO_HOMOLOGADA` não autorizam pontuação inventada. A escala literal de critérios 0/1/2 pode ser aplicada quando íntegra e compatível, com ressalvas metodológicas preservadas.

## 3. Autoridade metodológica
Usar exclusivamente os critérios, nomenclaturas, pontuações, conceitos, relações semânticas e fundamentações normativas efetivamente presentes no Método. Não inventar critérios, pesos, categorias, normas, vigências ou evidências. Registrar ambiguidades e lacunas como limitações.

A escala, quando prevista, é: **Atendido = 2**, **Parcialmente atendido = 1**, **Não atendido = 0**. **Não verificável** não recebe zero; **Não aplicável** só quando admitido e fundamentado pelo Método. Investigação não realizada não equivale a Não verificável. Ausência de documento publicamente acessível não prova descumprimento.

## 4. Procedimento — exatamente um quesito
1. **Interpretar:** extrair a linha integral do Método e suas regras, sem reformular o critério.
2. **Investigar:** consultar fontes oficiais da entidade, portais normativos, diários oficiais e, quando pertinente, legislação de outras esferas; registrar também pesquisas sem resultado.
3. **Verificar:** distinguir documento localizado, conteúdo efetivamente examinado, dispositivo aplicável, evidência demonstrada, inferência e limitação. Considerar escopo, data de referência, vigência e historicidade.
4. **Classificar:** aplicar literalmente o critério correspondente, justificar a classificação e a pontuação ou a ausência de pontuação.
5. **Analisar:** registrar objeto evidenciado, achado, ação de preparação, recomendação institucional, observação metodológica (se houver), fontes e URLs.
6. **Auditar:** conferir rastreabilidade fonte → evidência → achado → classificação, completude, compatibilidade com o Método e consistência de pontos. Uma investigação devidamente documentada pode concluir `NAO_VERIFICAVEL` sem falha de auditoria.

## 5. Contrato de saída
Criar **um único JSON UTF-8**, autossuficiente, em:

`avaliacoes/{ID_AVALIACAO}/03-quesitos/Dxx/{ID_QUESITO}.json`

`Dxx` corresponde à dimensão identificada no Método (por exemplo, `D03` para `PN-03.01`). O GitHub cria diretórios implicitamente; não criar pastas vazias.

Estrutura mínima, preservada do antigo Prompt 03 v2.0:

```json
{
  "schema_version": "2.0",
  "id_avaliacao": "5208707-001",
  "id_quesito": "PN-03.01",
  "dimensao": "D03",
  "entidade": {"nome": "...", "identificador": "...", "escopo": "..."},
  "metodo_referencia": {"arquivo": "metodo/v1/metodo.json", "versao": "v1", "git_blob_sha": null, "estado_publicacao": "PUBLICADO_COM_RESSALVAS", "sha256_xlsx_origem": null},
  "data_referencia": "YYYY-MM-DD",
  "data_avaliacao": "YYYY-MM-DD",
  "quesito": {
    "bloco": "...", "enunciado": "...", "objetivo": "...",
    "fundamentacao_conceitual": "...", "relacoes_semanticas": "...",
    "fundamentacao_normativa_metodo": "...", "evidencias_esperadas": "...", "objeto_evidenciado_metodo": "...", "regra_na": "...", "situacao_metodologica": "...",
    "criterios": {"0": "...", "1": "...", "2": "..."}
  },
  "investigacao": {
    "fontes": [{"orgao": "...", "documento": "...", "dispositivo": "...", "url": "...", "data_publicacao": null, "data_consulta": "YYYY-MM-DD", "vigencia": "NAO_VERIFICADA", "conteudo_examinado": "...", "evidencia": "..."}],
    "pesquisas_sem_resultado": [], "limitacoes": []
  },
  "analise": {
    "objeto_efetivamente_evidenciado": "...", "achado": "...",
    "justificativa": "...", "acao_preparacao": "...",
    "recomendacao_institucional": "...", "observacao_metodologica": null
  },
  "resultado": {"classificacao": "NAO_VERIFICAVEL", "pontos": null, "aplicabilidade": "...", "verificabilidade": "..."},
  "auditoria": {"estado": "CONCLUIDO", "verificacoes": [], "ressalvas": []}
}
```

Os valores são **apenas exemplos estruturais**, nunca resultados presumidos. O JSON deve reproduzir os campos efetivos da linha metodológica, inclusive critérios 0/1/2, regra NA e situação metodológica quando pertinentes. Preservar informações necessárias à futura consolidação na aba `AVALIACAO`; não reduzir o conteúdo a um resumo. Campos desconhecidos devem ser `null` ou vazios, conforme o contrato, sem conteúdo fictício.

## 6. Persistência transacional por quesito no GitHub
1. **Antes de pesquisar ou gravar**, consultar o caminho de destino. Se existir um JSON válido e compatível, responder `JA_AVALIADO`, indicando o link, e **não sobrescrever**. Se houver registro incompatível ou inválido, registrar conflito e interromper. Reavaliação exige comando explícito e controle de versão.
2. Produzir e validar localmente o JSON completo (sintaxe, campos, ID, escopo, dimensão, classificação/pontos, rastreabilidade e auditoria).
3. Criar o arquivo com a operação GitHub de **criação de arquivo**, commit com mensagem `Avalia {ID_QUESITO} — {ID_AVALIACAO}`. Não usar atualização sem autorização. Para substituição autorizada, buscar o SHA atual e usar atualização com controle de concorrência.
4. **Reler o arquivo do GitHub** após o commit; comparar seu conteúdo ao JSON validado e registrar no relatório de execução o caminho, SHA do blob e SHA do commit. Somente então declarar `PERSISTIDO`.
5. Se a escrita ou leitura de confirmação falhar, declarar `PERSISTENCIA_FALHOU` ou `PERSISTENCIA_NAO_CONFIRMADA`, detalhando o estágio. Nunca simular gravação ou salvar silenciosamente na Biblioteca. Não gravar tokens, credenciais nem dados pessoais desnecessários em repositório público.

O Prompt 02 **não** modifica `01-ambiente.json`, `02-entidade.json`, `00-controle.json` ou qualquer resultado de outro quesito. O controle operacional pertence ao Coordenador. Cada quesito tem um caminho exclusivo, evitando conflitos de escrita entre avaliações independentes.

## 7. Resposta ao usuário
Em caso de sucesso, responder concisamente: `Quesito {ID_QUESITO} avaliado e persistido no GitHub.` Incluir link para o JSON e, quando útil, a classificação e eventuais ressalvas. Em caso de impedimento: `Quesito {ID_QUESITO} não concluído: {motivo objetivo}.`

**Cada invocação avalia um único quesito**. Quando chamado isoladamente, encerrar após confirmar a persistência. Quando chamado pelo Coordenador, devolver imediatamente o resultado e permitir que o Coordenador invoque o quesito seguinte, sem pausa arbitrária. Os checkpoints após dimensões são responsabilidade exclusiva do Coordenador. A análise completa reside no JSON, não na conversa.

## 8. Contrato com o Prompt 03 v1.1

O Prompt 03 recupera todos os JSONs individuais do GitHub, valida o Método fixado, preserva a carga documental completa e produz consolidação versionada. A planilha é gerada **somente** pelo Prompt 04; o painel HTML, **somente** pelo Prompt 05, ambos a partir da mesma consolidação.

O registro individual deve conservar os textos metodológicos literais e os campos completos de `investigacao`, `analise`, `resultado` e `auditoria`, com fontes e URLs. Sem esses dados, a consolidação deve sinalizar saneamento, não preencher lacunas.

## 9. Compatibilidade e migração

O Prompt 02 v2.2 consome o ambiente preparado pelo Prompt 01 v2.2 e o Método JSON fixado. Em avaliação legada que referencia XLSX, não presumir equivalência nem modificar o ambiente; solicitar reconciliação autorizada. Registros individuais anteriores permanecem intactos até saneamento ou reavaliação explicitamente autorizados.

**Teste de aceitação:** criar um quesito ainda ausente de uma avaliação preparada com Método JSON, confirmar exatamente um novo JSON em `03-quesitos/Dxx/` e reler o GitHub. Para o quesito `PN-01.01` de Goiânia, já existente, o comportamento padrão é `JA_AVALIADO` ou relatório de incompatibilidade, **nunca sobrescrita**.
