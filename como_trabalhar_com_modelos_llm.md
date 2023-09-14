# Como trabalhar com modelos LLM

## Como funcionam os modelos LLM (large language models)

[Modelos LLM][Large language models Blog Post] são funções que mapeiam texto por texto. Dada uma sequência de entrada de texto, um modelo LLM prevê o texto que deve vir em seguida.

A magia dos modelos LLM é que, ao serem treinados para minimizar esse erro de previsão sobre grandes quantidades de texto, os modelos acabam aprendendo conceitos úteis para essas previsões. Por exemplo, eles aprendem:

* como soletrar
* como funciona a gramática
* como parafrasear
* como responder a perguntas
* como manter uma conversa
* como escrever em muitas línguas
* como programar
* etc.

Nenhum desses recursos é explicitamente programado – todos eles surgem como resultado do treinamento.

O GPT-3 capacita [centenas de produtos de software][GPT3 Apps Blog Post], incluindo aplicativos de produtividade, aplicativos educacionais, jogos e entre outros, e com a chegada do GPT-4, [podemos ver ainda mais recursos][GPT4].

## Como controlar um modelo LLM

De todoss os tipos de inputs para um modelo LLM, de longe a mais influente é o prompt de texto.

LLM podem ser direcionados a produzir saídas de algumas maneiras:

* **Instrução**: Dizer ao modelo o que você quer
* **Completude**: Induzir o modelo a completar o início do que você deseja
* **Demonstração**: Mostrar ao modelo o que você deseja, com:
  * Alguns exemplos no prompt
  * Centenas ou milhares de exemplos em um conjunto de dados de treinamento de ajuste fino

Um exemplo de cada um é mostrado abaixo.

### Prompts de instrução

Instrução acompanhada de modelos (por exemplo, 'text-davinci-003' ou qualquer modelo que comece com 'text-') são especialmente projetados para seguir instruções. Escreva sua instrução no topo do prompt (ou na parte inferior, ou ambos), e o modelo fará o possível para seguir a instrução e, em seguida, parar. As instruções podem ser detalhadas, então não tenha medo de escrever um parágrafo detalhando explicitamente a saída desejada.

Exemplo de prompt de instrução:

```text
Extrair o nome do autor da citação abaixo.

“Alguns humanos teorizam que espécies inteligentes se extinguem antes de poderem se expandir para o espaço exterior. Se eles estiverem corretos, então o silêncio do céu noturno é o silêncio do cemitério.”
― Ted Chiang, Exalação
```

Saída:

```text
Ted Chiang
```

### Exemplo de prompt de completude

Os prompts de estilo de completude aproveitam o fato de que os modelos LLM tentam escrever texto que eles consideram mais provável de vir a seguir. Para direcionar o modelo, tente começar um padrão ou frase que será completada pela saída que você deseja. Em relação às instruções diretas, esse modo de direcionar o modelo LLM pode exigir mais cuidado e experimentação. Além disso, os modelos não necessariamente saberão onde parar, então você frequentemente precisará de sequências de parada ou pós-processamento para interromper o texto gerado além da saída desejada.

Exemplo de prompt de completude:

```text
“Alguns humanos teorizam que espécies inteligentes se extinguem antes de poderem se expandir para o espaço exterior. Se eles estiverem corretos, então o silêncio do céu noturno é o silêncio do cemitério.”
― Ted Chiang, Exalação

O autor desta citação é
```

Saída:

```text
 Ted Chiang
```

### Exemplo de prompt de demonstração (few-shot learning)

Semelhante aos prompts de estilo de completude, as demonstrações podem mostrar ao modelo o que você deseja que ele faça. Essa abordagem é às vezes chamada de few-shot learning (aprendizado com poucos exemplos), pois o modelo aprende a partir de alguns exemplos fornecidos no prompt.

Exemplo de prompt de demonstração:

```text
Citação:
“Quando a mente racional é forçada a enfrentar o impossível repetidamente, não tem escolha senão se adaptar.”
― N.K. Jemisin, A Quinta Estação
Autor: N.K. Jemisin

Citação:
“Alguns humanos teorizam que espécies inteligentes se extinguem antes de poderem se expandir para o espaço exterior. Se eles estiverem corretos, então o silêncio do céu noturno é o silêncio do cemitério.”
― Ted Chiang, Exalação
Autor:
```

Saída:

```text
 Ted Chiang
```

### Exemplo de prompt de ajuste fino (Fine-tuned)

Com exemplos de treinamento suficientes, você pode [fazer um ajuste fino][Documentacao de Ajuste Fino] num modelo personalizado. Nesse caso, as instruções se tornam desnecessárias, pois o modelo pode aprender a tarefa a partir dos dados de treinamento fornecidos. No entanto, pode ser útil incluir separadores (por exemplo, -> ou ### ou qualquer sequência de caracteres que não apareça comumente em suas entradas) para indicar ao modelo quando o prompt terminou e a saída deve começar. Sem os separadores, há o risco de o modelo continuar elaborando sobre o texto de entrada em vez de começar a responder o que você deseja ver.

Exemplo de prompt de ajuste fino (para um modelo que foi treinado personalizadamente em pares de completude de prompt semelhantes):

```text
“Alguns humanos teorizam que espécies inteligentes se extinguem antes de poderem se expandir para o espaço exterior. Se eles estiverem corretos, então o silêncio do céu noturno é o silêncio do cemitério.”
― Ted Chiang, Exalação

###


```

Saída:

```text
 Ted Chiang
```

## Capacidade de código

Modelos LLM não são muito bons apenas para texto - eles também podem ser ótimos para código de programação. A OpenAI tem um modelo especializado em código chamado [Codex].

Codex alimenta atualmente [mais de 70 produtos][Postagem de Blog Codex Apps], incluindo:

* [GitHub Copilot] (completa automaticamente o código no VS Code e em outras IDEs)
* [Pygma](https://pygma.app/) (transforma designs do Figma em código)
* [Replit](https://replit.com/) (tem um botão 'Explicar código' e outras funcionalidades)
* [Warp](https://www.warp.dev/) (um terminal inteligente com pesquisa de comandos de IA)
* [Machinet](https://machinet.net/) (escreve modelos de teste de unidade em Java)

Observe que, ao contrário dos modelos de texto que seguem instruções (por exemplo, text-davinci-002), o Codex não é treinado para seguir instruções. Portanto, é preciso escrever bons prompts e exige mais cuidado.

### Mais conselhos sobre prompts

Para obter mais exemplos de prompts, visite [OpenAI Exemplos][OpenAI Exemplos].

Em geral, um bom prompt de entrada é a melhor forma de melhorar as saídas do modelo. Você pode experimentar truques como:

* **Dar instruções mais explícitas.** Por exemplo, se você deseja que a saída seja uma lista separada por vírgulas, peça para retornar uma lista separada por vírgulas. Se você quiser que ele diga "Eu não sei" quando não souber a resposta, diga 'Diga "Eu não sei" se você não souber a resposta.'
* **Fornecer melhores exemplos.** Se você estiver demonstrando exemplos em seu prompt, certifique-se de que seus exemplos sejam diversos e de alta qualidade.
* **Pedir ao modelo para responder como se fosse um especialista.** Pedir explicitamente ao modelo para produzir uma saída de alta qualidade ou uma saída como se fosse escrita por um especialista pode induzir o modelo a dar respostas de maior qualidade que ele considera que um especialista escreveria. Por exemplo, "A seguinte resposta está correta, de alta qualidade e escrita por um especialista."
* **Pedir ao modelo para escrever os passos da série explicando seu raciocínio.** Por exemplo, inicie sua resposta com algo como "[Vamos pensar passo a passo](https://arxiv.org/pdf/2205.11916v1.pdf)." Pedir ao modelo para dar uma explicação de seu raciocínio antes de sua resposta final pode aumentar a probabilidade de que sua resposta final seja consistente e correta.


[Documentacao de Ajuste Fino]: https://beta.openai.com/docs/guides/fine-tuning
[Postagem de Blog Codex Apps]: https://openai.com/blog/codex-apps/
[Large language models Blog Post]: https://openai.com/blog/better-language-models/
[GitHub Copilot]: https://copilot.github.com/
[Codex]: https://openai.com/blog/openai-codex/
[GPT3 Apps Blog Post]: https://openai.com/blog/gpt-3-apps/
[GPT4]: https://openai.com/gpt-4
[OpenAI Exemplos]: https://beta.openai.com/examples
