# Exemplos de Comparação de Texto

O [OpenAI API embeddings endpoint](https://beta.openai.com/docs/guides/embeddings) (conexão com dispisitivos terminais) pode ser usado para medir a relação ou similaridade entre trechos de texto.

Aproveitando a compreensão do texto pelo GPT-3, essas incorporações [alcançou o estado da arte](https://arxiv.org/abs/2201.10005) sobre os parâmetros de referência na aprendizagem não supervisionada e em transferir configurações de aprendizagem. 

As incorporações podem ser usadas para pesquisa semântica, recomendações, análise de clusters, detecção de plágio e muito mais.

Para obter mais informações, leia os anúncios de postagens no blog da OpenAI:

* [Apresentando Incorporações de Texto e Código (jan 2022)](https://openai.com/blog/introducing-text-and-code-embeddings/)
* [Novo e Melhorado Modelo de Incorporação (dez 2022)](https://openai.com/blog/new-and-improved-embedding-model/)

Para comparação com outros modelos de incorporação, consulte o [Parâmentros de referência para incorporação de textos massivos](https://huggingface.co/spaces/mteb/leaderboard) (Massive Text Embedding Benchmark - MTEB) Tabela de classificação

## Pesquisa Semântica

As incorporações podem ser usadas para pesquisa, seja por si só ou como parte de um sistema maior.

A maneira mais simples de usar incorporações para pesquisa é a seguinte:

* Antes da pesquisa (pré-produção):
  * Divida o corpus do seu texto em pedaços menores que o limite de tokens (8.191 tokens para 'text-embedding-ada-002')
  * Incorpore cada pedaço de texto
  * Armazene essas incorporações em seu próprio banco de dados ou em um provedor de pesquisa por vetor como [Pinecone](https://www.pinecone.io), [Weaviate](https://weaviate.io) ou [Qdrant](https://qdrant.tech)
* No momento da pesquisa (em produção):
  * Incorpore a consulta de pesquisa
  * Encontre as incorporações mais próximas em seu banco de dados
  * Retorne os principais resultados

Um exemplo de como usar incorporações para pesquisa pode ser visto através deste notebook [Semantic_text_search_using_embeddings.ipynb](examples/Semantic_text_search_using_embeddings.ipynb).

Em sistemas de pesquisa mais avançados, a [similaridade por cosseno](https://medium.com/@raivitor/agrupando-frases-usando-similaridade-por-cosseno-c9d7a55be95b) das incorporações pode ser usada como um recurso entre muitos para classificar os resultados da pesquisa.

## Resposta a Perguntas

A melhor maneira de obter respostas confiáveis do GPT-3 é fornecer a ele documentos de origem nos quais ele possa localizar respostas corretas. Usando o procedimento de pesquisa semântica acima, você pode pesquisar de forma econômica através de um corpus de documentos em busca de informações relevantes e, em seguida, fornecer essas informações ao GPT-3 por meio do prompt para responder a uma pergunta. Isso é demonstrado neste notebook [Question_answering_using_embeddings.ipynb](examples/Question_answering_using_embeddings.ipynb).

## Recomendações

As recomendações são bastante semelhantes à pesquisa, exceto que, em vez de uma consulta de texto livre, as entradas são itens em um conjunto.

Um exemplo de como usar incorporações para recomendações é mostrado neste notebook [Recommendation_using_embeddings.ipynb](examples/Recommendation_using_embeddings.ipynb).

Assim como na pesquisa, esses scores de similaridade por cosseno podem ser usadas sozinhas para classificar itens ou como recursos em algoritmos de classificação maiores.

## Personalização de Incorporações

Embora os pesos do modelo de incorporação da OpenAI não possam passar por ajustes finos (fine-tuned), você ainda pode usar dados de treinamento para personalizar incorporações para sua aplicação.

Em [Customizing_embeddings.ipynb](examples/Customizing_embeddings.ipynb), é fornecido um método de exemplo para personalizar suas incorporações usando dados de treinamento. A ideia do método é treinar uma matriz personalizada para multiplicar vetores de incorporação, a fim de obter novas incorporações personalizadas. Com bons dados de treinamento, essa matriz personalizada ajudará a enfatizar as características relevantes para seus rótulos de treinamento. Você pode considerar a multiplicação da matriz como (a) uma modificação das incorporações ou (b) uma modificação da função de distância usada para medir as distâncias entre incorporações.
