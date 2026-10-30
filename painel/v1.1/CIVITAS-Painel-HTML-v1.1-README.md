# CIVITAS — Painel Executivo HTML v1.1

Template reutilizável para avaliações de prontidão normativa.

## Arquivos
- `template.html`: painel HTML independente, com logomarca incorporada.
- `contrato-dados.json`: estrutura de entrada e regras de validação.
- `exemplo-dados.json`: estrutura neutra para testar o carregamento.

## Utilização
1. Abra `template.html` em um navegador.
2. Clique em **Carregar JSON** e selecione o JSON consolidado de uma avaliação.
3. Confira indicadores, radar, dimensões e síntese.
4. Utilize **Imprimir / PDF** para produzir a versão de apresentação.

## Regras implementadas
- Atende = 2; Parcialmente = 1; Não atende = 0.
- Índice = (2×Atende + Parcialmente) / [2×(Atende + Parcialmente + Não atende)] × 100.
- NV, NA e pendências não entram no denominador.
- Índices são calculados no navegador, não copiados de campos potencialmente desatualizados.
- Denominador zero: índice ausente.
- A fórmula deve ser validada contra a metodologia oficial antes da homologação definitiva.

## Evolução em relação ao arquivo recebido
- Retirados os dados reais de Porto Alegre do template.
- Identificação da versão ajustada para v1.1.
- Removidos nomes de dimensões fixos e potencialmente incorretos.
- Corrigida a contagem de quesitos aplicados para excluir pendências.
- Recalculados pontos, máximos e índices a partir das contagens.
- Adicionada validação das dez dimensões e da consistência dos totais na importação JSON.
- Preservados layout, marca incorporada, radar, cartões, síntese e impressão.

## Publicação proposta no repositório
`painel/v1.1/template.html`, `painel/v1.1/contrato-dados.json`,
`painel/v1.1/exemplo-dados.json` e `painel/v1.1/README.md`.

**Situação:** pacote preparado para revisão e publicação; não publicado automaticamente.
