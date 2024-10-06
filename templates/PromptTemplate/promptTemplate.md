# Trabalhando com Templates no LangChain

Quando se trabalha com grandes modelos de linguagem (LLMs), um dos desafios é garantir que os prompts sejam claros, consistentes e adequados ao contexto. O **LangChain** facilita essa tarefa com o uso de **PromptTemplate**, que permite criar templates de prompts dinâmicos e personalizados, fornecendo um método estruturado para interagir com os modelos.

## O que é um Prompt e por que ele é tão importante?

Um **prompt** é, em essência, o ponto de partida na comunicação com um modelo de linguagem. Ele serve como a instrução que passamos ao modelo para que ele entenda a tarefa que desejamos que seja realizada, seja uma pergunta, uma solicitação de análise ou qualquer outro tipo de comando. O prompt é a peça-chave que define o que esperamos do modelo, estabelecendo a tarefa e o contexto.

### Por que o prompt é tão importante?

- **Clareza e Direcionamento**: A forma como o prompt é formulado determina diretamente a qualidade da resposta. Um prompt mal estruturado pode gerar respostas vagas ou fora do contexto. Por outro lado, um prompt claro e específico ajuda o modelo a fornecer respostas mais precisas e alinhadas com o esperado.
- **Controle do Contexto**: O prompt permite que o desenvolvedor defina o contexto em que o modelo deve operar. Isso significa que, além de formular a questão, podemos fornecer orientações, regras ou princípios para que o modelo atue de acordo com as diretrizes estabelecidas. Por exemplo, podemos especificar que o modelo considere os princípios **SOLID** ao avaliar um código.
- **Personalização da Resposta**: Usando variáveis e placeholders, como o **PromptTemplate**, os prompts podem ser adaptados dinamicamente para diferentes cenários, garantindo consistência na estrutura, mas permitindo personalização com base nas variáveis fornecidas, como linguagem de programação ou tecnologia utilizada.
- **Eficiência**: Um prompt bem definido economiza tempo ao garantir que o modelo tenha as informações necessárias e o contexto certo desde o início. Isso evita retrabalho e facilita a criação de fluxos de trabalho automatizados, especialmente quando se utiliza ferramentas como o **LangChain**, que geram prompts programaticamente.

## O que é o PromptTemplate?

O **PromptTemplate** é uma classe no LangChain que permite criar prompts dinâmicos com placeholders para variáveis. Isso significa que você pode configurar prompts que sejam preenchidos com diferentes valores conforme a necessidade, mantendo a estrutura e a coerência.

### Vantagens do uso de PromptTemplate

- **Flexibilidade**: Em vez de escrever o prompt de cada vez, você cria um modelo que pode ser reutilizado com diferentes variáveis, economizando tempo.
- **Consistência**: Ao usar um template, você garante que todos os prompts seguem a mesma estrutura e padrão, facilitando o controle de qualidade.
- **Personalização**: É possível adaptar os prompts com base em diferentes inputs, ajustando o comportamento do modelo para diferentes cenários.

## Exemplo prático: Criando um template de prompt

Vamos criar um exemplo simples onde configuramos um **PromptTemplate** que gera uma pergunta para um modelo de linguagem altamente experiente. O exemplo utiliza Python e LangChain para criar um prompt personalizado que simula uma situação onde o modelo avalia um trecho de código e sugere melhorias com base nos princípios **SOLID**.

### Código:

```python
from langchain import PromptTemplate
from langchain_openai import ChatOpenAI
import os
from dotenv import load_dotenv

load_dotenv(dotenv_path="../.env")
OPENAI_MODEL_NAME = os.getenv('OPENAI_MODEL_NAME')
llm = ChatOpenAI(temperature=0, model=OPENAI_MODEL_NAME)

# Template de Prompt
template_text = '''Você é um programador altamente experiente, com mais de 20 anos de carreira em 
desenvolvimento de software. Seu conhecimento abrange profundamente as melhores práticas de engenharia 
de software, incluindo os princípios SOLID, Clean Architecture, TDD, e design patterns. Sua experiência 
inclui linguagens como {programming_language}, e você tem um rigoroso compromisso com a qualidade do código, 
mantendo-o sustentável, legível e de fácil manutenção. Considere também as tecnologias e frameworks que você domina, 
como {technologies}, ao elaborar a solução. Por favor, explique a solução em detalhes, considerando as seguintes diretrizes: 
Explique como sua solução mantém conformidade com os princípios SOLID.

{question}
'''

# Configurando o PromptTemplate
prompt = PromptTemplate(input_variables=["programming_language", "technologies", "question"], template=template_text)

# Exemplo de pergunta
question = '''
function calculate(a: number, b: number, op: string) {
  if (op === "add") return a + b;
  else if (op === "subtract") return a - b;
  else if (op === "multiply") return a * b;
  else if (op === "divide") return b !== 0 ? a / b : NaN;
  else return NaN;
}
const result = calculate(5, 3, "add");
console.log("Result:", result);

Como poderia melhorar a qualidade e a legibilidade desse código acima?
'''

# Gerando a mensagem final
message = prompt.format(programming_language="typescript", technologies="NestJS", question=question)

# Chamando o modelo para processar o prompt
result = llm.invoke(message)
print(result)

```

## Explicando o exemplo

Neste exemplo, criamos um **PromptTemplate** com três variáveis de entrada:

- **programming_language**: A linguagem de programação relevante para a questão (no caso, TypeScript).
- **technologies**: As tecnologias utilizadas no contexto da solução (por exemplo, NestJS).
- **question**: O trecho de código e a pergunta que queremos fazer ao modelo de linguagem.

O **PromptTemplate** utiliza essas variáveis para gerar um prompt final que será enviado ao modelo de linguagem. O LLM então responde com base na estrutura do prompt, fornecendo uma solução conforme os princípios **SOLID** e outras boas práticas de engenharia de software.

## Benefícios para Desenvolvedores

O uso de **PromptTemplate** no LangChain oferece aos desenvolvedores uma maneira poderosa e eficiente de criar prompts que podem ser usados em diversos cenários. Isso permite a criação de fluxos de trabalho mais flexíveis e automatizados, ajudando a aumentar a produtividade e a qualidade do código gerado pelos LLMs.

## Conclusão

Com o **PromptTemplate**, os desenvolvedores podem criar prompts personalizados e reutilizáveis, garantindo que a interação com modelos de linguagem seja eficiente e escalável. Esse é apenas um dos muitos recursos que o LangChain oferece para facilitar o desenvolvimento de soluções baseadas em LLMs.

No próximo post, vamos explorar como combinar essas operações e criar fluxos de trabalho sequenciais com o conceito de **“chains”**. Fique ligado!
