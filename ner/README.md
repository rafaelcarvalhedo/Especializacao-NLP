# Reconhecimento de Entidades Nomeadas (NER)

Notebook: [`ner.ipynb`](ner.ipynb)

## A ideia

NER localiza menções a entidades no texto e as classifica. Formalmente é **classificação por
token** no esquema BIO — `B-` inicia a entidade, `I-` continua, `O` fica fora — o que permite
representar entidades de vários tokens sem ambiguidade.

A avaliação, porém, é feita em **span exato**: o modelo só acerta quando o tipo e as duas
fronteiras coincidem com a anotação. Prever `LOC:Rancho Alegre` onde o gold diz
`LOC:Rancho Alegre d' Oeste` conta como falso positivo *e* falso negativo.

## Modelo e corpus

- **Modelo**: `pt_core_news_sm` (spaCy, treinado em WikiNER). Rótulos `PER`, `ORG`, `LOC`, `MISC`.
- **Gold**: [WikiNEuRal](https://huggingface.co/datasets/Babelscape/wikineural) split `test_pt` —
  10.160 frases anotadas com os **mesmos quatro rótulos**, o que permite comparar sem mapeamento.

O notebook constrói o `Doc` a partir da tokenização do corpus (`Doc(nlp.vocab, words=...)`) em vez
de passar o texto remontado. Sem isso, os índices de token dos dois lados não batem e a avaliação
mede desalinhamento em vez de qualidade.

## Roteiro

1. Anatomia de um `Doc`: tokens, spans, `ent_iob_`, visualização com `displacy`
2. O corpus anotado e a conversão BIO → spans (com os casos de borda testados por `assert`)
3. Previsão com tokenização alinhada ao gold
4. Precisão, revocação e F1 por rótulo, calculados na mão, conferidos contra o `Scorer` do spaCy
5. Análise de erros separada em **fronteira**, **tipo** e **inventada**
6. `EntityRuler` para entidades de formato fixo — CNPJ, CPF, número de lei
7. Limitações e exercícios

## Resultado esperado

Em 2.000 frases: F1 micro ~0,91. Por rótulo, `LOC` lidera (~0,95) e `MISC` fica bem atrás (~0,72),
em parte por desacordo de anotação — o rótulo mistura obras, eventos e nacionalidades.
