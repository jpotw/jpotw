---
sticker: lucide//code-2
---
[[jocoding.langchain]]


왜 랭체인?

한줄: Provide interchangable tools to automate each step of a text generation pipline
- loading data, parsing, storing, querying it, passing it to large language models
- integrates with lots of services
- easy to swap out providers (e.g. chatgpt → llama)



Two big goals of langchain:

1. Provide tools to automate each step of a text generation pipeline
2. make it easy to connect tools together

by **CHAIN**

Python Class provided by LangChain


# 왜 체인임? - 랭체인 안에서 벌어지는 것

랭체인이라는 말을 들으면 일단 화가 난다.

뭐가 그리 이름이 복잡하냐

나는 그랬다.


그래서 바보도 이해할 수 있는 랭체인 이 있었으면 좋겠다 생각했고 아래는 가능한 한 가~장 쉽게 이해할 수 있게 했다.

- 데이터 인식
- 에이전트 기능


input - prompt template - Model(LM) e.g. OpenAI - output

*Inputs*: Dictionary that must contain values for each variable the PromptTemplate requires
예를 들어, prompt template가

	write a {language} snippet that will {task}

라면,

input 에는 

	dict = [language:"python", task:"print numbers 1 to 10"]

이런 식으로

**CHAIN ELEMENTS**
- *prompt template*
- *language model* Ex. Bard, Claude, ChatGPT, etc.


*outputs*: inputs, 그리고 chain을 통해 만들어진 데이터로 이루어진 Dictionary

	code:"for i in range(1,11): print(i)"


# a


4. load text

5. chunck/split

6. embedding: takes a string and change it into a ray of numbers

7. store vectors in database

8. chat with doc using database (relevancy)

## PromptTemplate

```
prompt = PromptTemplate(
	template =""
	input_variables =[]
)

chain = LLMChain(
)

result = chain(
input_variables
)
```



# 2024-01-07~

# 모두의 AI 시리즈

[[모두의AI_Langchain이 뭘까]]?

[[모두의AI_LLM 초간단 설명]]
파인튜닝하거나 오픈소스 모델을 이용할 때 GPU 서버가 대체 얼마나 필요한 거임??

[[모두의AI_Langchain PromptTemplate]]

[[모두의AI_Vectorstores]]

[[모두의AI_Retriever]]


# PromptTemplate

```
# 랭체인 설치
!pip install langchain

# 변수를 지정하지 않을때
from langchain import PromptTemplate

no_input_prompt = PromptTemplate(input_variables=[], template="Tell me a joke.")
print(no_input_prompt.format())

# 하나의 변수 지정
from langchain import PromptTemplate

one_input_prompt = PromptTemplate(input_variables=["blogPostTopic"], template="Create a blog post about“{blogPostTopic}”")
print(one_input_prompt.format(blogPostTopic="tomato benefits"))


# 변수 2개 지정
multiple_input_prompt = PromptTemplate(
 input_variables=["blogPostTopic", "tone"],
 template="Create a blog post about “{blogPostTopic}” . Write it in a “{tone}” "
)
print(multiple_input_prompt.format(blogPostTopic="사과 효능", tone="funny"))


# 변수 3개 지정
multiple_input_prompt = PromptTemplate(
 input_variables=["blogPostTopic", "tone", "keywords"],
 template="Create a blog post about “{blogPostTopic}” . Write it in a “{tone}” tone. include the following keywords: “{keywords}”."
)
print(multiple_input_prompt.format(blogPostTopic="사과 효능", tone="funny", keywords = "건강"))

```





## Summarize Chain

```python
chain = load_summarize_chain(llm, 
                             map_prompt=prompt, 
                             combine_prompt=combine_prompt, 
                             chain_type='map_reduce', 
                             verbose=False)
```

map reduce 방식을 쓸 경우 map_prompt에 text를 assign 한 다음 combine_prompt에서 한 번에 처리함


- 와 드디어 **Created a chunk of size 1738, which is longer than the specified 200** 지옥에서 벗어남
	- 왜 무한오류였냐, 정확한 이유는 아직 모르겠으나 **seperator**를 정해주지 않으면 잘 못 자르는 것 같음 (얘가 임의적으로 자르기 시작함)

```python
text_splitter = CharacterTextSplitter(
             separator="\n", ## 이거!!
             chunk_size=200,
             chunk_overlap=20,
             length_function = len,
             is_separator_regex = False,
        )
```



## Huggingface 을 이용해 embed.py를 만들어보자


```python
from langchain.embeddings.huggingface import HuggingFaceEmbeddings
from langchain.document_loaders.text import TextLoader
from langchain.text_splitter import CharacterTextSplitter
from langchain.vectorstores.chroma import Chroma 


load_dotenv()
inference_api_key = os.getenv("HUGGINGFACEHUB_API_TOKEN")

model_name="jhgan/ko-sbert-nli" # 한국어 임베딩용 모델이다. 원하는 모델 갖고 와서 쓸 수 있다.
model_kwargs= {'device': 'cpu'}
encode_kwargs={'normalize_embeddings': True}

# Embedding 정의
hf = HuggingFaceEmbeddings(
    model_name=model_name,
    model_kwargs=model_kwargs,
    encode_kwargs=encode_kwargs
) # inference_api_key가 자동으로 hf에 들어가는 건가 ?? 이건 나도 왜 이런지 모르겠네 (jhgan/ko-sbert-nli가 public model이라 그런듯)


# Text Split 정의 by CharacterTextSplitter
text_splitter = CharacterTextSplitter(
    separator="\n",
    chunk_size = 600,
    chunk_overlap  = 0,
    length_function = len,
    is_separator_regex = False,
)

# 텍스트를 불러오고 스플릿한다. 이때 txt의 위치를 잘 assign해주자
loader = TextLoader('.txt', encoding = 'UTF-8')
docs = loader.load_and_split(text_splitter=text_splitter)

# Embedding을 하고 Chroma DB에 값 저장, persist_directory를 설정해주면 한 번만 돌려도 db 계속 갖다 쓸 수 있음
db = Chroma.from_documents(
    docs,
    embedding=hf,
    persist_directory= "hf"
)
```


**databse duplicate(복제) 문제** → qa process와 load - embed process를 구분짓는다!


## Retreival

Langchain에서는 DB를 이용해 Retriever를 쓴다. (RAG)


Retriever: Str을 받고 그와 가장 관련된 문서들을 return하는 기능을 갖춘 데이터베이스.

문제는 database마다 세세한 세부설정이 다르다.

그래서 각 데이터베이스가 통일된 구조를 갖게 하는 기능이 바로

```
retriever=db.as_retriever()
```
임.

이러면 db라고 assign한 데이터베이스가 retriever가 돼서 RetrievalQA 함수를 쓸 수 있음.



**RetrievalQA 함수의 기능**

```
chain = RetrievalQA.from_chain_type(
    llm=chat,
    retriever=retriever,
    chain_type="stuff" # stuff가 제일 단순함
)


result = chain.run("what is an interesting fact about language?")
```


## Langchain Summarization 정복하기 : stuff, map reduce, map_rerank





랭체인 디버깅
```
import langchain
langchain.debug=True
```




## 자주 발생하는 오류들(이거 보면 해결할 수도 있음)

### ImportError: cannot import name 'OpenAI' from 'openai'

```
from openai import OpenAI
```

를 했는데 자꾸

```
ImportError: cannot import name 'OpenAI' from 'openai'
```

이렇게 뜸.


결론은 버전이 잘못 되었거나 제대로 다운이 안 돼서 그렇다고 한다.

해결한 방법:

```
pip uninstall openai
pip install openai
```

하니까 다시 됨..