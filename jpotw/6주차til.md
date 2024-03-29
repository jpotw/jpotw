
# 6주차 TIL

1. bookshorts의 기능을 고도화 - 각종 디버깅, 로그인 회원가입 기능 스무스 - 지금은 또 login_required 기능 추가했더니 잘 안 돌아가긴 함
	1. 그 과정에서 Flask-WTF의 flow에 대해서 파보았다. [Flask-WTF 프로세스 쉽게 설명한 글 (tistory.com)](https://jpotw.tistory.com/entry/Flask-WTF-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EC%89%BD%EA%B2%8C-%EC%84%A4%EB%AA%85%ED%95%9C-%EA%B8%80)
2. insight_extract RAG 1차 완 디자인은 구림
	1. 그 과정에서 RAG에 대해 좀 공부할 수 있었음
3. inganjipyo 리서치함. 플레이스토어 일일 다운로드 지수를 확인하는 것은 생각보다 어려움. 유료까지 가면 불가능하진 않은듯.
4. 101 For Dummies 만듦. 일주일에 한 번씩은 정독하자 (세세한 거 보지말고 그냥 이런 게 있다 식으로 눈대중)


# 2024-01-08

혹시 PromptTemplate을 제대로 설정 안 해줘서 그런 게 아닐까?
PromptTemplate을 수정해보고 결과가 어떤지 확인해보자


일단 Huggingface Embeddings 로 한국어 특화 임베딩을 하자

Solar 10 7b가 요즘 가장 핫한 한국어 모델이라 함
[LDCC/LDCC-SOLAR-10.7B · Hugging Face](https://huggingface.co/LDCC/LDCC-SOLAR-10.7B)

확인하는 법: [Open Ko-LLM Leaderboard - a Hugging Face Space by upstage](https://huggingface.co/spaces/upstage/open-ko-llm-leaderboard)
ko-llm 리더보드 들어가서 보면 됨


```
load_dotenv()

inference_api_key = os.getenv("HUGGINGFACEHUB_API_TOKEN")

model_name="LDCC/LDCC-SOLAR-10.7B"
model_kwargs= {'device': 'cpu'}
encode_kwargs={'normalize_embeddings': True}

hf = HuggingFaceEmbeddings(
    model_name=model_name,
    model_kwargs=model_kwargs,
    encode_kwargs=encode_kwargs
)

db = Chroma.from_documents(
docs,
embeddings=hf,
persistent_derectory = "원하는 이름" # 이래야지 임베딩 생성 중복 방지 ㄱㄴ
)
```

유튜브 참조함


## 랭체인 최신문서 찾음

docs에 써 있는대로 import했는데 
ModuleNotFoundError: No module named 'langchain_community'
뜸

구글링 했는데 자꾸 No module named 'langchain' 해결하는 법만 뜸.

구글에 Langchain Huggingface Embeddings 치니 최신 공식 문서 떠서 그대로 썼더니 해결됨

이게 최신 공식 문서: 여기 Ctrl+F 치면 다 나옴. 랭체인 docs보다 이게 더 좋은거 같은데???
얘네는 업데이트를 하도 많이 해서 최신 문서를 찾는 게 더 좋음
[langchain_community 0.0.9 — 🦜🔗 LangChain 0.1.0](https://api.python.langchain.com/en/latest/community_api_reference.html#module-langchain_community.document_loaders)



## 허깅 페이스는 처음 다운 받을 때 이런다
```
PS C:\kakao_extract> python 1.py
Downloading .gitattributes: 100%|█| 1.52k/1.52k [00:00<00:0
Downloading README.md: 100%|█| 1.02k/1.02k [00:00<00:00, 1.
Downloading config.json: 100%|████| 641/641 [00:00<?, ?B/s]
Downloading generation_config.json: 100%|█| 133/133 [00:00<
Downloading (…)of-00005.safetensors:   1%| | 41.9M/4.96G [0
```


## 구글 콜랩은 신이다

걍 용량 큰거 있으면 무조건 콜랩으로 돌리자 
망할 허깅페이스가 다운이 4시간이 넘게 걸린 이유: 컴퓨터가 구데기여서

GPU는 신이다! 구글 colab은 신이다!

인 줄 알았는데

colab 왜 변수등록하고 아래에 쓰면 정의되지 않았다고 함.. 



일단 해야 될 거:
RAG 좀 제대로 돌려보기..ㅇㅇ



# 2024-01-09

## Flask-RESTful document

>The main building block provided by Flask-RESTful are resources. Resources are built on top of [Flask pluggable views](http://flask.pocoo.org/docs/views/), giving you easy access to multiple HTTP methods just by defining methods on your resource.

"Resources"가 기본, resources 는 method(Class 안의 함수)로 쉽게 정의할 수 있다고 함.


```
from flask import Flask
from flask_restful import Resource, Api

app = Flask(__name__)
api = Api(app) # 이게 뭐임

todos= {}

class ToDoSimple(Resources):
	def get(self, todo_id):
        return {todo_id: todos[todo_id]}

    def put(self, todo_id):
        todos[todo_id] = request.form['data']
        return {todo_id: todos[todo_id]}

api.add_resource(TodoSimple, '/<string:todo_id>') # 이것도 뭐임

if __name__ == '__main__':
    app.run(debug=True)
```

무조건 json 형식(python dictionary)으로 나타내야 한다


그러니까 이걸로 CRUD 쉽게 만들 수 있겠네


## requirements.txt 파일 installment 다운받는 법

```
pip install -r requirements.txt
```



```
파일이 안 돌아가면 이름을 다시 한 번 살펴보자(Flask 특히)
__init__.py
```


form function을 쓸때는 action도



## 모르겠는거 모음 → 답변 - 240110 개발상담 참고

### 1. 모르는 걸 어떻게 검색하나요?

스타일 차이일수도 있는데 하다가 막히는 거 생기면 10분 안에 못 찾으면 넘어가기? vs 될때까지 계속찾기

검색 팁 ??


### 2.

ex.
```
@bp.route('/', methods=['GET', 'POST'])
def answer():
    if request.method == 'POST':
        retriever=db.as_retriever(search_kwargs={"k":3}) #해당 데이터베이스를 retriever로 설정
        question = request.form.get('question')
        docs = retriever.get_relevant_documents(question)
        # print(type(question)) #str
    #     # 나는 여기서 막혀있다. 왜 docs가 빈칸이 나오지
        print(retriever)
        doc_content=[]
        for doc in docs:
            doc_content.append(doc.page_content)
        print(doc_content)
        return render_template('answer.html', doc_content=doc_content)
    else:
        return render_template('main.html')

```

간단한 Flask 에 Langchain 라이브러리 이용중.

원래라면 retriever.get_relevant_documents(question)을 하면 docs가 나와야댐. 근데 docs가 계속 빈칸으로 나옴:[]

인터넷에 langchain retriever.get_relevant, langchain retriever 다 쳐 봄

![](https://i.imgur.com/Lb2l7sM.png)

Langchain Docs에 나오는 그대로 했는데 계속 빈값임



```
bp = Blueprint('main', __name__, url_prefix='/')


load_dotenv()
openai_api_key=os.getenv("OPENAI_API_KEY")
chat=ChatOpenAI()
embeddings=OpenAIEmbeddings()
db=Chroma(
            persist_directory="/emb",
            embedding_function=embeddings
        )
@bp.route('/', methods=['GET', 'POST'])
def answer():
    if request.method == 'POST':
        retriever=db.as_retriever() #해당 데이터베이스를 retriever로 설정
        question = request.form.get('question')
        docs = retriever.get_relevant_documents(question, k=1)
        print(question)
        print(docs) #대체 왜 빈값임
        return render_template('answer.html')
    else:
        return render_template('main.html')
```

아니 이게 왜 안 됨



(2024-01-10)
왜 됐는지 모르겠는데

embeddings를 huggingface걸로 바꾸니까 잘 됨 ..
1. 처음에는 1.py를 이용해서 db만 huggingface로 바꿨다가 
	1. 똑같이 안 돼서 main_views.py 것도 바꿔줌
아직도 안 된 이유는 잘 모르겠음
1. persist directory를 절대경로로 설정해서 그런건가??


### 3. 도움될 만한 오픈소스 코드 찾았는데 읽을 짬이 안 되는 거 같음

대가리 깨져가면서 읽기 vs 공부나 하셈

```
request = response.get("{api end point}")
```




## REST API에 대하여 

어떤 API들은 parameters가 필요하다
API document 에서 참조


## REST 스타일 API 만드는 법

### HTTP Request Verbs
1. GET
2. POST
3. PUT/PATCH
4. DELETE



## Whipser API 세팅

무료 STT하는 법 찾아벌임


# 2024-01-10


flask templates의 html파일들은 for loop을 쓸 때 안에 {{}} 쓰지 X 

Ex.
```html
{% extends "base.html" %}
{% block title %}관련 문서 페이지{% endblock %}
{% block content %}
    <h1>관련 문서</h1>
    <ul>
        {% for answer in relevant_docs %}
            <li>
                {{ answer }}
            </li>
        {% endfor %}
    </ul>
{% endblock %}
```



## 프로그램이 시작부터 안 돌아가면 
1. 함수의 문제는 아님
2. 순서의 문제일 수 있음 (정의 안 된 걸 돌려버린다든가)
3. 코드 윗부분의 문제일 가능성이 높음



# 2024-01-11


왜 인지 모르겠지만 font를 갖고와도 인식하지 못함ㅜ

findfont: Font family 'font.ttf' not found.


그래프로 표현할 방법을 찾다가 Copilot이 matplotlib 라이브러리를 추천해줌

```python
pip install matplotlib
import matplotlib.pyplot as plt # 이게 플롯차트를 그려준다 매우 손쉽
 # 데이터 분리
    dates = [item['period'] for item in data['results'][0]['data']]
    ratios = [item['ratio'] for item in data['results'][0]['data']]

    # 그래프 생성
    plt.rc('font', family="font.ttf")
    plt.plot(dates, ratios)
    plt.xlabel('date')
    plt.ylabel('search rate')
    plt.title('Bitcoin Inganjipyo')
    plt.show()

```

대충 x와 y 데이터를 각각 분리해서 불러온다.

그다음 plt.rc, plot, xlabel, ylabel, title, show를 통해 그래프를 손쉽게 그릴 수 있음


근데 폰트가 왜 안 먹히는지 모르겠음 (그래서 일단 오류 안 뜨게 영어로 바꿔놓음)



data를 json형태로 받았다.

대충 이런식으로 나올 것이다:
```python
body = {
    "startDate": "2023-11-30",
    "endDate": "2024-01-10",
    "timeUnit": "week",
    "keywordGroups": [
        {
            "groupName": "비트코인",
            "keywords": ["비트코인", "비트코인 가격"]
        }
    ],
    "device": "pc",
    "ages": ["1", "2"],
    "gender": "f"
}
# body에서 정의한 대로 key값들이 나오고 있음을 확인할 수 있다.
```


앞으로 코드 짜고 이해 안 되면 한줄한줄 설명 듣고 그래도 이해 안 되면 키워드 적고 나중에 복습/집중탐구 ㄱㄱ

#Keyword:
**matplotlib**
**urllib** (url handling modules) [urllib — URL handling modules — Python 3.12.1 documentation](https://docs.python.org/3/library/urllib.html)
**json.loads** `json` 모듈은 JSON 형식의 데이터를 파이썬에서 사용하기 위해 필요한 모듈입니다. JSON 형식의 문자열을 파이썬 객체로 변환하거나, 파이썬 객체를 JSON 형식의 문자열로 변환하는 작업에 사용됩니다.


[[python.keywords]]





# 2024-01-14


### `AttributeError: module 'openai' has no attribute 'error'`

insight_extract 에서  AttributeError: module 'openai' has no attribute 'error' 가 계속 떴음. ChatGPT한테 물어봐도 딱히 얻은 게 없었다 근데 
구글링 하니
[[langchain + openai 에러] AttributeError: module 'openai' has no attribute 'error' (tistory.com)](https://dtbb.tistory.com/18)

고마우신 분께서 langchain version을 낮춰야 한다고 말씀해주심. 너무 최신 버전은 안정화가 안 돼서 잘 작동을 안 한 거였음. 감사함니다.



매주 한 번씩은 101 for dummies를 읽자.
나는 dummy니까..