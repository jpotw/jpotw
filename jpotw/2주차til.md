# 2023-12-11

git pull request 어떻게 하는지 몰랐는데 이제 대충 알았다.

우선 
1. 내 로컬 프로그램에 내가 Fork해둔 파일을 git clone {fork 링크} 으로 갖고 온 후,
2. 내 입맛대로 변경한 다음
3. add, commit, push 해주면 된다.
4. 그 다음에 Fork해둔 Repository에 들어가서 Pull Request 누르면 merge하겠냐고 나옴.
5. 크게 바꾼 거 아니면 바로 conflict 없이 바로 보낼 수 있음.
끝

cf. 미국에서 버블 노코드 툴이 뜬다고 하는데 버블이나 배울까..


### *args 와 **kwargs에 대해 ARABOZA

1. *args
*args는 함수의 인자다.
python 함수에 몇 개의 인풋이 들어올지 모를 때 사용할 수 있는 것이다. 
type = 튜플로 반환한다.

예시 코드:
```
def input_printer(*args):
    for arg in args:
        print(arg)
```


2. **kwargs
**kwargs도 함수의 인자다.
딕셔너리 형태로 {key:value}로 구성돼있다.

예시코드
```
def input_printer(**kwargs):
    for key, value in kwargs.items():
        print({"0 is 1"}.format(key, value))

input_printer(myname=jp)
```

여기서

**.items()**는 딕셔너리의 키와 값을 튜플로 묶어 반환하는 메서드입니다. 따라서 kwargs.items()는 kwargs 매개변수에 전달된 키워드 인자의 이름과 값을 튜플로 묶어 반환합니다.

**{0}과 {1}**은 format() 메서드의 인수로 사용됩니다. format() 메서드는 문자열을 형식 지정하는 메서드입니다. {0}은 key 변수의 값을 나타내고, {1}은 value 변수의 값을 나타냅니다.




# 2023-12-12


**Langchain에서 Huggingface Embeddings를 만들기 위한 환경 세팅(안 끝남; 나중에 수정해야 됨)** → 2024-01-14에 수정

```
pip install sentence_transformers # HuggingFace Embedding 사용 위해 필요
```


- .env 파일에
```
HUGGINGFACEHUB_API_TOKEN = “hf-xxx”
```
huggingface 공홈을 들어가면 찾을 수 잇다


(Chroma Vectorstore 기준)
- main.py에
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



**궁금한 것들 모음**
- init이 뭐임 → initialize. 걍 기본값이라 생각하면 댐
- self가 뭐임 → 나중에 클래스 꺼내 쓸 때의 객체
- @은 왜 붙는거임
- async def는 뭐임 → 이건 아직도 모르게는데?


# 2023-12-14

### 한 것
- GPT4V를 어떻게 활용할지에 대해 생각해보고 있음.

### 리뷰

### Async 와 Await 

- **Async functions** let you write code that does multiple things at once, like a pro chef.
- **Await** lets you pause your code while waiting for something to happen, like getting more flour for your cake.
- Together, they make your code more efficient and responsive, just like a well-oiled kitchen.

- In the example you provided, the `await puppeteer.launch` makes the code wait for the browser to launch before assigning it to the `browser` variable. This ensures that the code doesn't try to perform any operations on the browser before it's ready.
**Think of it like this:**

- Async is the oven that allows you to bake multiple things at once.
- Await is like checking on each dish individually to see if it's done before taking it out.
- The oven cooks all the dishes simultaneously, but you wouldn't take them out until each one is ready.

보통 같이 쓰임



**3시 20분~ : 30+분 삽질한 이유 복기**

```
PS C:\ai_projects\blog_agent> npm run main
Debugger attached.
npm ERR! Missing script: "main"
npm ERR!
npm ERR! To see a list of scripts, run:
npm ERR!   npm run

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\parkj\AppData\Local\npm-cache\_logs\2023-12-14T06_53_38_214Z-debug-0.log
Waiting for the debugger to disconnect...
```

이와 비슷한 에러가 계속 떴다.

package.json 문제인데 

scripts 안의 파일의 경우 
```
npm run xxx.js
```
이런 식으로 돌리면 된다.

하지만 scripts 밖에 있는 파일은 **node xxx.js** 라고 돌려줘야 함.


# 2023-12-15




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


### Protocol Error

```
node screenshot.js
```
를 하면 어제까지는 멀쩡하게 됐는데 

```
C:\ai_projects\blog_agent\node_modules\puppeteer-core\lib\cjs\puppeteer\common\CallbackRegistry.js:96
    #error = new Errors_js_1.ProtocolError();
             ^

ProtocolError: Protocol error (Page.navigate): Invalid parameters Failed to deserialize params.url - BINDINGS: mandatory field missing at position 50
```

계-속 이 에러 뜸. 왜 이러는지 모르겟음.



	with: 코드 블럭이 끝나면 자동으로 닫힘

    f.read(): 전체 파일을 읽고 메모리에 저장

    base64.b64encode(): 데이터를 base64로 암호화해서 메모리가 더 쉽고 정확하게 할 수 있도록 도움.

    decode 다시 base64string을 python string으로 변환