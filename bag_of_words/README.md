# Bag of Words e TF-IDF

Notebook: [`bag_of_words.ipynb`](bag_of_words.ipynb)

## A ideia

Bag of Words representa um documento pela **contagem das palavras que ele contém**, descartando a
ordem. O corpus inteiro define um vocabulário de $V$ termos, e cada documento vira um vetor de
dimensão $V$.

TF-IDF ajusta esses pesos: um termo vale mais quando é frequente **no documento** e raro **no
corpus**.

$$\mathrm{tfidf}(t, d) = \mathrm{tf}(t, d) \times \left[\ln\!\left(\frac{1 + n}{1 + \mathrm{df}(t)}\right) + 1\right]$$

Cada linha é normalizada em L2, para que o tamanho do documento não domine.

## Corpus

[B2W-Reviews01](https://huggingface.co/datasets/vladjr/B2W-Reviews01) — 132.373 avaliações de
produtos em português, com nota de 1 a 5. O notebook descarta a nota 3, converte o resto em
positivo/negativo e usa uma amostra balanceada de 6.000 avaliações.

## Roteiro

1. Carregar e preparar o corpus
2. Pré-processamento passo a passo: minúsculas, remoção de acentos, tokenização por regex,
   *stopwords* (preservando as negações)
3. Vocabulário e matriz de contagem em NumPy puro, com corte por `min_df`
4. TF-IDF na mão, com as fórmulas abertas
5. Conferência contra `CountVectorizer` e `TfidfVectorizer` — `assert` de igualdade
6. Classificação com `MultinomialNB` e `LogisticRegression`
7. Variações de `ngram_range`, `min_df`, `max_df`; termos de maior peso por classe
8. Onde o método falha (negação, ironia, OOV) e exercícios

## Resultado esperado

Acurácia em torno de 92% com unigramas e 93% acrescentando bigramas. A matriz de treino é ~0,4%
não-zero, o que explica por que o scikit-learn usa representação esparsa.
