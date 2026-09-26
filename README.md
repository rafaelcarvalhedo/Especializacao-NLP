# Especialização NLP — laboratório de métodos

Repositório de estudo: cada pasta isola uma metodologia de NLP num notebook executável de ponta a
ponta, com o método implementado **na mão** primeiro e depois conferido contra a biblioteca de
referência. Todos os corpora são em português.

## Módulos

| Pasta | Metodologia | Corpus | O que se fixa |
|---|---|---|---|
| [`bag_of_words/`](bag_of_words/) | Bag of Words e TF-IDF | [B2W-Reviews01](https://huggingface.co/datasets/vladjr/B2W-Reviews01) — 132 mil avaliações de e-commerce | Texto → vetor, pesagem de termos, classificação de sentimento |
| [`ner/`](ner/) | Reconhecimento de Entidades Nomeadas | [WikiNEuRal `test_pt`](https://huggingface.co/datasets/Babelscape/wikineural) — 10 mil frases anotadas | Esquema BIO, spans, avaliação por entidade, regras com `EntityRuler` |

Cada pasta tem o próprio `README.md` com o roteiro detalhado.

## Como rodar

O projeto usa [uv](https://docs.astral.sh/uv/). As dependências, incluindo o modelo
`pt_core_news_sm` do spaCy, estão fixadas no `pyproject.toml`.

```bash
uv sync                       # cria o .venv e instala tudo
uv run jupyter lab            # abre o Jupyter e navegue até o notebook
```

Para executar um notebook inteiro sem abrir a interface:

```bash
uv run jupyter nbconvert --to notebook --execute --inplace bag_of_words/bag_of_words.ipynb
```

A primeira execução de cada notebook baixa o corpus para `data/` (ignorado pelo git). As seguintes
usam o cache.

## Convenções

- **Implementar antes de importar**: o método vem primeiro em NumPy puro, e só depois entra a
  versão da biblioteca. As duas são comparadas com `assert` — se a implementação manual divergir,
  o notebook falha em vez de seguir em silêncio.
- **Semente fixa** (`RANDOM_STATE = 42`) em tudo que amostra ou divide dados.
- **Métricas honestas**: classes balanceadas quando se usa acurácia; precisão, revocação e F1 por
  classe no resto.
- **Erros são conteúdo**: cada notebook termina mostrando onde o método falha, não só onde acerta.
- Notebooks são versionados **com as saídas**, para dar para ler o resultado sem executar.
