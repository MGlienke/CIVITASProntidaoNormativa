# CIVITAS — PROMPT 02 — Avaliação Individual de Quesito

**Versão:** 2.1 — GitHub (proposta para validação)  
**Origem:** antigo Prompt 03 v2.0, renumerado como Prompt 02.  
**Repositório canônico:** `MGlienke/CIVITASProntidaoNormativa`, branch `main`.

## 1. Objetivo e limites
Avaliar **exatamente um quesito** da Avaliação de Prontidão Normativa CIVITAS, identificado por `ID_QUESITO`, no contexto institucional preparado pelo **Prompt 01 v2.1**. Investigar fontes oficiais, confrontar evidências com os critérios literais do Método, classificar, auditar e persistir **um JSON autossuficiente no GitHub**.

**Não** avaliar outros quesitos, modificar planilhas Excel, atualizar arquivos de controle globais, consolidar dimensões, calcular índices ou gerar painel. Uma execução não depende de resultados de outros quesitos.

## 2. Comando e entradas
Comando: `CIVITAS — Avaliar quesito [ID_QUESITO] da avaliação [ID_AVALIACAO]`.  
Exemplo: `CIVITAS — Avaliar quesito PN-03.01 da avaliação 5208707-001`.

Antes de avaliar, recuperar **diretamente do GitHub**:
- `avaliacoes/{ID_AVALIACAO}/01-ambiente.json`;
- `avaliacoes/{ID_AVALIACAO}/02-entidade.json`.

Conferir sintaxe, `schema_version`, identidade e coincidência do `id_avaliacao`, escopo, datas, referência metodológica, pendências impeditivas e coerência entre os dois arquivos. O primeiro é obrigatório nesta versão para identificar a fonte do Método; o segundo contém a identidade e o escopo institucionais. Se estiverem ausentes, incompatíveis ou não puderem ser lidos, interromper sem classificar.

Recuperar o **Método canônico**: prioritariamente de `metodo/` no mesmo repositório, conforme a versão referida em `01-ambiente.json`. **Transição autorizada:** enquanto o Método ainda não estiver publicado no GitHub, ler `CIVITAS Prontidão Normativa - Metodo - v1.xlsx` na Biblioteca `/CIVITAS/Prontidao Normativa/Metodo/`, registrando a origem e a limitação de integridade. Se a fonte ou versão não corresponder ao contexto, não substituir silenciosamente. Não executar com memória, resumos ou exemplos em lugar da linha efetiva do Método.

Localizar `ID_QUESITO` por correspondência exata e **única** na aba `METODO`. Conferir a dimensão derivada do ID, bloco, enunciado, objetivo, fundamentações, relações, evidências e critérios. Examinar `LISTAS`, `AVALIACAO` e outras abas apenas quando necessário. Se o ID não existir ou for duplicado, interromper.

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
  "metodo_referencia": {"arquivo": "...", "versao": "...", "sha256": null, "origem": "..."},
  "data_referencia": "YYYY-MM-DD",
  "data_avaliacao": "YYYY-MM-DD",
  "quesito": {
    "bloco": "...", "enunciado": "...", "objetivo": "...",
    "fundamentacao_conceitual": "...", "relacoes_semanticas": "...",
    "fundamentacao_normativa_metodo": "...", "evidencias_esperadas": "...",
    "criterios": "..."
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

O Prompt 02 **não** modifica `01-ambiente.json`, `02-entidade.json`, `03-controle.json` ou qualquer resultado de outro quesito. Cada quesito tem um caminho exclusivo, evitando conflitos de escrita entre avaliações independentes.

## 7. Resposta ao usuário
Em caso de sucesso, responder concisamente: `Quesito {ID_QUESITO} avaliado e persistido no GitHub.` Incluir link para o JSON e, quando útil, a classificação e eventuais ressalvas. Em caso de impedimento: `Quesito {ID_QUESITO} não concluído: {motivo objetivo}.`

**Não iniciar automaticamente o quesito seguinte**: cada invocação avalia um único quesito. A análise completa reside no JSON, não na conversa.

## 8. Contrato de consolidação futura
O futuro consolidador deve recuperar os JSONs individuais do **GitHub**, validar cobertura de IDs, compatibilidade de método e escopo, duplicidades, classificação/pontuação e rastreabilidade. Só então preencher a planilha de resultados **uma única vez**, sem refazer pesquisas ou classificações. O componente de painel consumirá os resultados consolidados. O Prompt 02 não atualiza contadores, planilhas ou painéis.

## 9. Compatibilidade e migração
Este **Prompt 02 v2.1** substitui o antigo **Prompt 03 v2.0**. Consome `01-ambiente.json` e `02-entidade.json` do **Prompt 01 v2.1**, já persistidos em `avaliacoes/5208707-001/`. O Método pode continuar temporariamente na Biblioteca, mas os resultados novos são gravados **exclusivamente no GitHub**. Ao migrar o Método para `metodo/`, atualizar sua referência no contexto institucional de forma controlada, sem mudar a versão metodológica silenciosamente.

**Teste de aceitação sugerido:** `CIVITAS — Avaliar quesito PN-01.01 da avaliação 5208707-001`. Verificar que somente `avaliacoes/5208707-001/03-quesitos/D01/PN-01.01.json` foi criado e confirmado por releitura.
