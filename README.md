# nlp-perfumery-reviews-case-study
# Análise de Sentimento: O "Cheiro" do Problema na Categoria Perfumaria

Este projeto é uma investigação de ponta-a-ponta para diagnosticar a performance da categoria "Perfumaria" de um e-commerce. A análise parte de uma suspeita de baixo desempenho e utiliza **Processamento de Linguagem Natural (NLP)** para analisar os reviews de clientes.

A grande descoberta foi que a insatisfação dos clientes não estava ligada ao *produto* (qualidade, fragrância), mas sim à *logística* (atrasos na entrega).

## Objetivos

A investigação foi guiada por duas perguntas principais:
1.  Qual é o sentimento geral (positivo, negativo ou neutro) dos clientes em relação à categoria Perfumaria?
2.  Quais são os principais pontos de reclamação (gaps) que estão prejudicando a performance da categoria?

##  Ferramentas Utilizadas

* **Python 3**
* **Pandas:** Para manipulação, merges complexos e tratamento dos dados.
* **LeIA (SentimentIntensityAnalyzer):** Biblioteca de NLP para análise de sentimento adaptada para o português.
* **NLTK (Natural Language Toolkit):** Para tratamento de texto e remoção de *stopwords*.
* **Scikit-learn (`CountVectorizer`):** Para extração de N-gramas (bigramas e trigramas).
* **Matplotlib & Seaborn:** Para visualização dos dados e resultados.
* **Jupyter Notebook / Google Colab:** Como ambiente de análise.

---

##  O Processo de Análise

A análise foi dividida em 5 etapas, desde a coleta dos dados brutos até a recomendação estratégica.

### 1. O Desafio dos Dados (ETL)

O primeiro desafio foi técnico. Os dados estavam em três datasets separados: `Produtos`, `Pedidos` e `Reviews`.
* Não havia uma ligação direta entre `Produtos` (onde estava a categoria "perfumaria") e `Reviews` (onde estava o feedback).
* **Solução:** Foi necessário um **merge em duas etapas**:
    1.  Primeiro, unimos `Produtos` + `Pedidos` (via `product_id`) para "etiquetar" todas as ordens que continham perfumes.
    2.  Segundo, uni o resultado anterior + `Reviews` (via `order_id`) para finalmente conectar os produtos de perfumaria aos seus respectivos feedbacks.

### 2. Limpeza e Preparação (O Problema da "Voz Duplicada")

Durante a limpeza de dados, além de tratar valores nulos, um problema crítico foi identificado: **reviews duplicados**.
* **Causa:** Quando um cliente comprava múltiplos itens no mesmo pedido e deixava *um único review*, o sistema copiava esse review para *todos* os itens do pedido.
* **Risco:** Isso inflava artificialmente a contagem de reviews, fazendo uma única reclamação ter peso 2x, 3x ou 4x.
* **Solução:** Os dados foram tratados para manter apenas uma ocorrência por `review_id`, garantindo que a voz de cada cliente fosse contada apenas uma vez.

### 3. Análise 1: O Veredito Geral (Sentimento)

Com os dados limpos, usei a biblioteca `leia` para classificar cada comentário. O resultado confirmou a suspeita inicial de performance "morna":
* **~55% Positivos**
* **~20% Negativos**
* **~20% Neutros**

Quase 40% dos clientes não estavam ativamente satisfeitos. Isso provou que havia um problema, mas ainda não sabíamos *qual*.

### 4. Análise 2: A Causa-Raiz (NLP e N-gramas)

Para encontrar o "gap", focamos apenas nos comentários **negativos**.
* O texto foi limpo (stopwords, pontuação, etc.).
* Utilizei`CountVectorizer` do Scikit-learn para extrair não apenas palavras soltas, but **bigramas** (ex: "entrega atrasada") e **trigramas** (ex: "não recebi produto"). Isso é crucial para entender o *contexto* da reclamação.

### 5. A Grande Descoberta: O Vilão era a Logística

A hipótese era encontrar reclamações sobre os *produtos*: "cheiro fraco", "embalagem quebrada", "fixação ruim".

A realidade foi completamente diferente.

Ao plotar os N-gramas mais frequentes, ficou claro que a esmagadora maioria das reclamações era sobre **LOGÍSTICA**.

Os principais termos foram:
* `entrega atrasada`
* `não recebi` / `nao recebi produto`
* `prazo de entrega`
* `demora para entregar`
* `produto nao chegou`

---

##  Conclusão e Recomendação

O "cheiro" do problema não vinha dos frascos de perfume. A percepção negativa da categoria "Perfumaria" não estava sendo causada pelo *produto*, mas pela *experiência de compra*.

**Recomendação Estratégica:** A ação de maior impacto para melhorar a performance da categoria não é rever o catálogo de produtos ou fornecedores. A urgência está em uma **auditoria e otimização dos processos logísticos** (prazo, rastreio, transportadora) associados a esta categoria.
