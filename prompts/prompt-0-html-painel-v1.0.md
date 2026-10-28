# CIVITAS — PROMPT 0-H — Preparação e Publicação do Template HTML do Painel Executivo

**Versão do prompt:** 1.0 — proposta para validação
**Fonte inicial na Biblioteca:** `CIVITAS-Painel-HTML-v0.3.html`
**Repositório:** `MGlienke/CIVITASProntidaoNormativa`, branch `main`
**Componente consumidor:** Prompt 05 — Geração do Painel Executivo.

## 1. Objetivo
Ler uma versão explicitamente escolhida do template HTML do Painel Executivo CIVITAS na Biblioteca, preservar o arquivo original, analisar HTML/CSS/JavaScript e seus contratos de entrada, preparar uma variante parametrizada e publicar no GitHub artefatos versionados, verificáveis e reutilizáveis. O template é o **padrão visual**, não a fonte dos indicadores; o JSON consolidado pelo Prompt 03 é a fonte dos resultados.

**Não** avaliar entidades, recalcular índices, substituir decisões visuais, modificar a versão original na Biblioteca, inventar dados ou homologar alterações sem autorização.

## 2. Comandos
- `CIVITAS — Preparar template HTML do Painel v0.3`: ler, auditar e gerar artefatos de preparação; **não publicar** sem comando de publicação.
- `CIVITAS — Publicar template HTML do Painel v0.3`: preparar, auditar, solicitar autorização quando houver divergências bloqueantes ou decisões visuais e publicar a versão autorizada.
- `CIVITAS — Auditar template HTML do Painel v0.3`: verificar sem modificar.
- `CIVITAS — Atualizar template HTML do Painel v0.4`: usar o novo arquivo explicitamente escolhido na Biblioteca; criar nova versão sem sobrescrever anteriores.

A versão deriva da fonte escolhida e da decisão do usuário. Se houver versões concorrentes, confirmar qual é a vigente antes de publicar.

## 3. Fonte de referência e inspeção obrigatória
1. Recuperar os **bytes completos** do HTML selecionado da Biblioteca, não apenas snippets, texto resumido ou conteúdo do chat.
2. Registrar nome, caminho/ID da Biblioteca, tamanho, versão, SHA-256 dos bytes originais, data de processamento e eventual especificação visual complementar efetivamente localizada.
3. Analisar o documento real: DOM, CSS, fontes, cores, breakpoints, impressão, imagens incorporadas em data URI, scripts, dependências externas, IDs, atributos, tabelas, SVG e campos dinâmicos.
4. Validar HTML/JS/CSS na medida suportada pelo ambiente. Para comportamento e fidelidade visual, realizar renderização real em navegador quando houver ferramenta; sem isso, registrar `VALIDACAO_VISUAL_NAO_EXECUTADA`.
5. Preservar logos e imagens embutidas. Nunca reconstruir manualmente uma imagem base64, nem substituir por aproximação.

### Observações já identificadas na fonte v0.3
- Contém cartões de indicadores, régua de prontidão, radar SVG de dez dimensões, detalhamento por dimensão, sínteses e nota metodológica.
- Há carregamento local de JSON por `input type=file` e função `render()`.
- O script incorpora dados demonstrativos de Porto Alegre.
- O script calcula totais, índices, aplicados e cores; **não presumir** equivalência dessas regras com o Método canônico.
- O nome do arquivo indica `v0.3`, mas a barra superior contém o texto `Template HTML v0.2`. Registrar discrepância e solicitar decisão para correção na versão preparada.
- Há regras CSS para impressão em A3 paisagem; preservar.
Essas observações orientam a auditoria, mas **não substituem** inspeção integral da fonte.

## 4. Separação entre apresentação e dados
O HTML preparado deve ser um **renderizador** de resultados já consolidados. Não pode reavaliar quesitos nem redefinir denominadores, cobertura, pontuação ou qualificação.

Fonte dos indicadores: `avaliacoes/{ID}/04-consolidacoes/consolidacao-vNNN.json` (versão e SHA fixados).
Fonte do Método: `metodo/v1/metodo.json`, ou a versão vinculada à avaliação.
O Prompt 05 fará a adaptação **determinística** da consolidação para o contrato do painel, sem modificar índices.

Se a fonte HTML tiver cálculos internos, classificá-los:
- `APRESENTACAO`: formatação, arredondamento visual autorizado, posicionamento de elementos, renderização do radar.
- `DERIVACAO_SEMANTICA`: totais, índices, cobertura, NA/NV, faixas e qualificação. Esses valores devem vir do consolidado ou de mapeamento explicitamente homologado; **não recalcular** por conta própria.
- `INCOMPATIVEL`: regra divergente ou impossível de validar; registrar bloqueio ou ressalva e propor ajuste específico, sem aplicá-lo silenciosamente.

Não preencher dimensões sem índice com 0 para o radar. Diferenciar índice ausente de zero real; representar ausência de forma visualmente explícita e sem criar polígono enganoso. Diferenciar NA, não verificável, pendente e não respondido de acordo com as categorias efetivas do Método.

## 5. Pré-processamento permitido
- Preservar integralmente o arquivo original como `template-original.html`.
- Criar `template.html` como derivado rastreável: substituir **somente** o conjunto de dados demonstrativos por mecanismo de entrada parametrizado e adaptar campos, IDs e lógica de renderização quando estritamente necessário ao contrato.
- Manter composição, tipografia, cores, logotipo, componentes, SVG, impressão e responsividade, salvo alterações expressamente aprovadas.
- Escapar texto dinâmico; não injetar dados da avaliação por concatenação insegura de HTML/JavaScript.
- Garantir funcionamento offline quando o template original for autocontido, sem acrescentar dependências de CDN.
- Não trocar gráficos por imagens estáticas; o radar deve funcionar com dez dimensões e dados reais.
- Não inserir nome, brasão, imagens ou cores da entidade em substituição à identidade CIVITAS.
- Não transportar os dados demonstrativos para a versão preparada. Em ausência de dados, mostrar estado `SEM_DADOS` ou orientação neutra, nunca valores inventados.

## 6. Contrato de dados
Produzir `contrato-dados.json` com:
- `schema_version`, `versao_template`, `fonte_consolidacao`, `campos`, `regras_de_apresentacao`, `validacoes`, `categorias`, `dimensoes` e `compatibilidade`.
- Para cada campo visual: ID/seletor HTML, tipo, origem **exata** no JSON consolidado, obrigatoriedade, tratamento de null, formatação e regra de exibição.
- Mapear: entidade, município/UF, CNPJ quando pertinente, data, versão do Método, total previsto, cobertura, atendidos, parciais, não atendidos, NA, não verificáveis, pendências, índice global, dez índices dimensionais, radar, tabela, sínteses e nota metodológica.
- Se o schema do Prompt 03 não fornecer determinado campo, marcar `SEM_FONTE` e **não inventar** mapeamento.
- Registrar explicitamente a versão e SHA do consolidado usados em cada painel gerado.
- Não embutir a consolidação de uma entidade no template canônico.

## 7. Auditoria
Verificar:
1. Integridade: SHA-256 do original; arquivo completo; HTML/JS/CSS legíveis; imagens e fontes preservadas.
2. Estrutura: todos os IDs usados pelo JavaScript existem, são únicos quando exigido, e todos os componentes visuais esperados estão presentes.
3. Dados: nenhum valor demonstrativo é interpretado como dado real; cada indicador tem origem no contrato; null e ausência são tratados.
4. Método: cores/faixas, pontuação, denominadores e categorias comparados ao Método e à consolidação; registrar divergências sem presumir que o template prevalece.
5. Radar: dez dimensões, ordenação canônica, distinção de 0 e sem índice, labels e percentuais; conferir renderização.
6. Detalhamento: totais e índices exibidos correspondem exatamente aos valores consolidados, inclusive dimensões sem avaliação.
7. Identidade visual: CSS, logotipo, fontes, impressão A3 paisagem, responsividade, cores e composição; realizar comparação visual quando disponível.
8. Segurança: escapar campos, evitar scripts externos não aprovados e preservar funcionamento local.
9. Reprodutibilidade: validar JSON de contrato e relatório, comparar original byte a byte e reler os arquivos publicados.

Classificar achados como `BLOQUEANTE`, `RESSALVA` ou `INFORMATIVO`. Não declarar validação visual se não houve renderização.

## 8. Artefatos e versionamento
```text
painel/
  v0.3/
    template-original.html
    template.html
    contrato-dados.json
    validacao.json
```
`validacao.json` registra versão, proveniência, SHA-256, componentes, verificações, divergências, modificações autorizadas, diferenças entre original e derivado, testes realizados e não realizados, estado de publicação e compatibilidade com Prompt 03.

**Imutabilidade:** nova revisão visual deve ser publicada em pasta nova (`v0.4`, `v0.5`, etc.). Não sobrescrever versões anteriores sem decisão explícita e trilha de auditoria. Não atualizar painéis já gerados retroativamente.

## 9. Persistência
Na preparação, gerar arquivos locais e relatório, sem publicar.
Na publicação, exigir integridade técnica suficiente e autorização expressa para decisões que alterem comportamento ou apresentação. Se houver bloqueio, não declarar sucesso.
Criar os quatro arquivos no GitHub, reler integralmente, confirmar equivalência e registrar os SHAs/commits. Se houver publicação parcial, listar exatamente o que foi confirmado e o que falhou; não declarar conjunto concluído.

## 10. Integração com Coordenador e Prompt 05
O Coordenador fixa `versao_template`, `git_blob_sha_template`, `versao_consolidacao` e `git_blob_sha_consolidacao`.
O Prompt 05 lê `template.html` e `contrato-dados.json`, valida a consolidação e produz painel HTML versionado da avaliação. Não recria layout, não busca outra versão silenciosamente e não recalcula indicadores.
A planilha (Prompt 04) e o painel (Prompt 05) consomem **o mesmo JSON consolidado**, permitindo confronto direto dos resultados.

## 11. Teste de aceitação
- Fonte correta da Biblioteca: `CIVITAS-Painel-HTML-v0.3.html`.
- Original preservado integralmente, com SHA-256 verificável.
- Template derivado sem dados demonstrativos e com mapeamento determinístico.
- Radar, tabela, indicadores e sínteses completos.
- Ausência de índice não aparece como zero real.
- Divergências de versão interna e regras de cores/pontuação registradas.
- Compatibilidade com o JSON do Prompt 03 auditada; lacunas declaradas.
- Identidade visual e impressão preservadas, com teste visual documentado se executado.
- Publicação versionada, verificada por releitura; sem substituição de versões anteriores.

## 12. Resposta ao usuário
Informar fonte, versão, SHA-256, artefatos, mudanças necessárias, divergências, testes executados, estado `PREPARADO_NAO_PUBLICADO` ou `PUBLICADO_COM_RESSALVAS`/`HOMOLOGADO`, links e próximos ajustes. **Não confundir a publicação deste prompt com o processamento e publicação do HTML.**
