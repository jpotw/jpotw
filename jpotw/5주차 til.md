# 이번주에 한 것
- bookshorts 회원가입, 로그인, 히스토리 확인하는 거 다 만듦
- Flask 공부 조금
- thegigabrain 에서 영감을 받아 dcGPT에 대한 아이디어 구상
	- 이를 위해 RAG(검색증강생성 기술), Langchain 학습중


# 2024-01-02
- 일단 질문, answer 리스트부터 만들자 → 끝
- 현재 문제: 이전에 만들어둔 javascript 백엔드 어떻게 구현한 건지 까먹음(사실상 복붙이라) → 이 기회에 js 버리고 flask로 전부 백엔드 만들려고 하는데 감을 못 잡겠음
	- 만들고 싶은 것: summarize.html파일을 바탕으로 chatgpt API를 사용해 답변을 하고, 답변 내용을 answer라는 이름으로 db에 저장하는 것


## Database 삭제하는 법 : 
	  flask shell에 들어가서 model, db import 한 후,
	  
		BookInfo.query.delete()
		db.session.commit()


## html에서 FLASK로 POST 방식으로 보내기

```
<form method="post" action="/summarize">            
                <div class="chat-message assistant-welcome">
                <p class="assistant">요약을 원하실 경우, '요약하기'를 눌러주세요</p>
                </div>
        </div>
        <div class="chat-input">
            <input type="text" placeholder="요약하기">
            <button type="submit" class="button-container button" style="text-align: center;">보내기</button>
            </form>
```


Vercel쓰고 싶은데 어떻게 써야하는지 모르겠네


# 2023-01-03

## 한계점에 대한 아이디어
- db = aws lambda(python script로 가능한가?)
- 로딩 기능 추가 - 문구: 로딩 중입니다. 잠시만 기다려주세요 
- 버튼 누르고 오래 걸리는 이유: book info, content를 다음 html 페이지에 보내려면 한 번에 처리해야 돼서. 안 그러면 html form -> python script -> html -> python 식으로 처리해야됨 (내가 했을 때는 잘 안 되긴 했음)




# 2023-01-04
QA(서버 배포시 고려할 사항들, 개발자 양식장 목적, 아이디에이션)


# 2023-01-05
- 로그인한 사람만 책 요약할 수 있게 하려고 했음.
- flask-login import하고 @app.route아래에 @login_required 하면 된다고 해서 했음
- 근데 
```
from flask_login import LoginManager
from .models import Fcuser


db = SQLAlchemy()
migrate = Migrate()
login_manager = LoginManager()

  

def create_app():
    #login_manager

    login_manager.init_app(app)

  (생략)
  
  
  

@login_manager.user_loader

def load_user(user_id):

    return Fcuser.query.get(int(user_id))
```

이런 식으로 하니까 models에서는 init을 불러오고 init에서는 models를 불러와 circular import 오류가 뜸.
어떻게 해결해야될지 잘 모르겠어서 일단 패쓰




# 2024-01-06
## batch 파일 만드는 법 알아냄
 
- 우선 venvs 파일을 C:/ directory에 생성함 (m venv venvs)
- 환경변수에 들어가 PATH를 새로 만든다(C:/venvs)
- batch 파일을 만들어준다: 
```
@echo off
cd c:/bookshortsv2
set FLASK_APP=bookshorts
set FLASK_DEBUG=true
c:/venvs/myenv/scripts/activate

```
- .cmd로 저장한다.
- 명령 프롬프트에 파일 제목(ex. myproject (cmd 생략)) 입력하면 자동세팅완
- 나 근데 powershell 쓰는데 이건 오류가 계속 뜸.


## Django Flask 차이점
[언제 Django를, 언제 Flask를 사용해야 할까? | 블로그 | 딩그르르 (dingrr.com)](https://dingrr.com/blog/post/%EC%96%B8%EC%A0%9C-django%EB%A5%BC-%EC%96%B8%EC%A0%9C-flask%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C)

요약: 장고가 더 쉬움. 근데 장고는 새 기능을 만들 때 더 빡셈. 
- 자유도를 더 높이고 싶다 → 플라스크
- 이미 있는 기능으로 빨리 만들고 싶다 → 장고


the gigabrain 팀한테 직접 메세지로 어케 만들었는지 물어봤는데 아직 답은 안 옴. ㄲㅂ

# 2024-01-07

## How to make dcGPT

- 유저의 질문 → llm 분석 → 검색할 단어를 고른다 → '정확성' 순서 대로 상위 5~10개 포스팅 & 댓글을 크롤링한다 → llm 요약 → 문서와 함께 보여준다


## Semantic Search

[Semantic Search 기술과 활용법 : 네이버 포스트 (naver.com)](https://post.naver.com/viewer/postView.naver?volumeNo=34273567)

[My experience on starting with fine tuning LLMs with custom data : LocalLLaMA (reddit.com)](https://www.reddit.com/r/LocalLLaMA/comments/14vnfh2/my_experience_on_starting_with_fine_tuning_llms/)
[xlang-ai/instructor-embedding: [ACL 2023] One Embedder, Any Task: Instruction-Finetuned Text Embeddings (github.com)](https://github.com/xlang-ai/instructor-embedding)



## RAG 101


RAG은 무엇인가?

**RAG는 대규모 언어 모델(LLM)에서 질문에 대한 답변이나 텍스트를 생성하기 전에 광범위한 문서 집합에서 관련 정보를 검색하고, 이를 이용하여 응답을 생성하는 방법**입니다. 이는 LLM의 기존 문제점인 지식의 시대에 뒤떨어짐, 특정 영역에 대한 지식 부족, 그리고 응답의 투명성 부족을 해결하는 데 중점을 두고 있습니다.


그러니까 뭐냐고 → 검증생

**검색(R) 증강(A) 생성(G)**

외부 데이터를 **검색**하고 컨텍스트를 **구성/이해** 후 자연어 형태로 **생성**

아래 그림이 가장 잘 표현한 거라고 함


![](https://i.imgur.com/GkmAxzy.png)


- 문서, 각종 자료들을 쪼개고 임베딩/인덱싱한다.
	- 임베딩은 벡터화한다는 거고 인덱싱이 뭐지? chunck들을 label화한다는 건가
	- → 검색해보니 색인(책으로 치면 목차)을 만드는 걸 의미함. 만약 raw data를 그대로 db에 때려박으면 자료를 검색하는 데 시간이 너무 오래 걸림. 따라서 목차를 미리 만들어 둔 후, query에 맞는 정보만 더 깊이 들어가기 위해 indexing을 함 (출처: [[Database] DB 인덱싱(Indexing)이란? (velog.io)](https://velog.io/@bsjp400/Database-DB-%EC%9D%B8%EB%8D%B1%EC%8B%B1Indexing%EC%9D%B4%EB%9E%80))
- 사용자의 검색(query)도 같은 모델로 임베딩함
- 사용자의 검색 벡터 데이터에 가장 가까운 자료들을 찾는다
- llm에게 프롬프트를 준다: 가장 가까운 자료들 중 검색 데이터에 맞는 내용에 답해라(자연어로 생성"해줘")
- 답변하면 끝~


ChatPDF니 the gigabrain이니 다 이거 쓰는 거인듯

> 과격하게 표현해 보자면, 무지성으로 많은 데이터를 넣은 것보다 **더 좋은 정보의 knowledge를 구축하고 검색과 생성에 초점을 맞춘다면 LLM을 fine-tuning하는 것보다 효과적인 옵션이 RAG가 될 수 있습니다.**

라고 함.

(출처: [캐글에서 살펴본 RAG 트렌드 살펴보기 (1) (brunch.co.kr)](https://brunch.co.kr/@hotorch/20))




---

<iframe width="560" height="315" src="https://www.youtube.com/embed/TRjq7t2Ms5I?si=DsO6fUScOUgpm8nb" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>



```timestamp-url 
 https://youtu.be/TRjq7t2Ms5I?si=UkxhKld4tb73nPGf
 ```


```timestamp 
 03:27
 ```
Naive RAG의 단점:
- 퀄리티가 너무 낮음
- 할루시네이션 이슈 큼

```timestamp 
 05:36
 ```
Evaluation 기준




---
RAG VS Finetuning
[RAG or 파인튜닝? 선택 전 던져야할 몇가지 질문들 (brunch.co.kr)](https://brunch.co.kr/@delight412/620#comments)

1. 외부 데이터소스에 대한 엑세스가 필요한가?
Y: RAG
N: Finetuning

2. 특정 말투(뉘앙스)나 도메인 전문성을 보여주어야 하는가?
Y: Finetuning
N: RAG

3. 환각/새로운 정보에 취약한 정도
RAG이 덜 취약함
왜?
응답시 검색된 증거에 근거를 두기 때문

4. 얼마나 많은 라벨링 학습 데이터를 사용할 수 있는가?
많이: Finetuning
적음: RAG (학습 데이터로부터 독립적)

5. 투명성 정도(할루시네이션과 이어짐)
RAG: 검색 구성요소를 통해 정보의 출처, 얻는 프로세스를 확인할 수 있음
Finetuning: ㅁㄹ


아니 근데 RAG랑 파인튜닝을 같이 할 수는 없는거?
검색은 외부 검색, 말투는 내가 학습한 대로 이렇게 할 수 잇잖아
라고 물어봄(**댓글 달았으니까 나중에 확인 ㄱㄱ**)



---
### 그래서 어떻게 만드는거임

```timestamp-url 
 https://youtu.be/LhnCsygAvzY?si=bJx3uGZmPbBWMval
 ```

```timestamp 
 18:01
 ```
벡터 DB 만들기

Pinecone 많이 쓰시는 거 같네
```timestamp 
 19:25
 ```
Pinecone API Key


아니 근데 이건 Naive RAG인듯

---

### Advanced RAG using Langchian

```timestamp-url 
 https://youtu.be/KQjZ68mToWo?si=tGUAF50_ZITfthlJ
 ```

```timestamp 
 05:58
 ```
Parent DocumentSplitter 가 뭐임
- The `ParentDocumentRetriever` strikes that balance by splitting and storing small chunks of data. During retrieval, it first fetches the small chunks but then looks up the parent ids for those chunks and returns those larger documents. ... This can either be the whole raw document OR a larger chunk. (출처: [Parent Document Retriever | 🦜️🔗 Langchain](https://python.langchain.com/docs/modules/data_connection/retrievers/parent_document_retriever))
- 그러니까 chunk가 작아야지 임베딩이 잘 되는데 너무 작으면 전체 맥락을 소실할 수 있어서 먼저 작은 chunk로 만든 후 → query와 관련된 작은 chunk를 불러와 그 chunk의 맥락을 살필 수 있도록 늘리는 거임

```timestamp 
 08:30
 ```
MultiQueryRetriever: user query 바탕으로 여러가지 query를 만들어서 다각도로 접근 → 답변 기능 향상?

```timestamp 
 10:33
 ```
Contextual Compression:
Contextual Compression Retriever가 Base Retriever(벡터 db 저장된 곳)에 query를 날림
가장 유사한 문서 알려줌
그리고 contextual compression으로 문서 압축


---


## 일단 working하는지 아는 방법:

최소한으로 사이클 돌린다:

1. 나름 참고할 만한 글들이 있는 미주갤같은 곳을 크롤링 함 → 데이터셋은 포스팅+댓글 100개 정도 (이미 만든 dc crawler 이용)
2. 크롤링한 내용을 chunk, indexing해서 ChromaDB or PineconeDB 에 저장
3. Langchain 라이브러리 이용해서 Advanced RAG 적용 (Embedding, Retrieving all GPT3.5-turbo)
4. 답변 퀄 확인

바로 ㄱㄱ


VSCode 들어가니 프로그래밍 갤러리 개념글 9.7~11.14일거 시험삼아 크롤링 한 거 찾음
1 → 클리어



뭐 다운할때마다 requirements.txt에 적어주는 습관 들이기


terminal:
```
pip install --upgrade langchain_core
```

2까진 했는데 RAG가 아니라 일반적인 ChatGPT 대답이 나옴.

뭔가 시스템 문제라기 보다는 ChromaDB가 한국어 인식을 잘 못하는건가 .. 저번에 카톡 요약기 만들때도 그렇고


1시간 뒤...

일단 3 반 까진 함

```
from langchain.vectorstores.chroma import Chroma
from dotenv import load_dotenv
from langchain.chains import RetrievalQA
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
import os

  
load_dotenv()
openai_api_key=os.getenv("OPENAI_API_KEY")
chat=ChatOpenAI()
embeddings=OpenAIEmbeddings()
db=Chroma(
    persist_directory="emb",
    embedding_function=embeddings

)
retriever=db.as_retriever() #해당 데이터베이스를 retriever로 설정
docs = retriever.get_relevant_documents("프로그래머의 관점이 뭐야?")
  

print(docs)
```

retriever.get_relevant_documents가 젤 쉬운 버전인데 가장 관련성 있는 문서를 가져온다.

```
(venvs) PS C:\kakao_extract> python question.py

 Document(page_content='프로그래머의 관점.이 말 공감가는사람 없니?컴퓨터를 아예 모르는 사람이 프로그램이나 기계에 대해 작 동방식을 추측하고 질문하고 검색하는것과컴퓨터를 아는 사람이 작동방식이 이러이러할것같고 그래서 너가 얘기한 아이디어는 (가능/불가능)할것같아.한번 검색해볼게.(무 슨 키워드로 검색해야하는지 알고 궁금증이 무엇인지 정확히 암.)랑은 차이가 있지전자의 경우는주관식답변을 원한다.나 모르는데 알려줘.내가 뭘 모르는지도 몰라.(답변이 하나 나오면) 대충 받아들이라면 받아들이겠는데 납득은 잘 안가 -> 추가검색을 계속함후자의 경우는구체적인 선택지의 답변을 원한다.~~~는 있니/없니?~~~는 하니/안하니?~~~크기는 얼마니?~~~무슨 컵?~~~하루 몇섹?(퍽 퍽)프로그래머의 관점이 생각보다 정말 강력해.그리고 컴구 운체는 프로그래머의 관점을 비약적으로 상승시켜줘.내가 만든 이 데이터는 어느 공간에 있으며어느 공간으로 이동하고락은 어떻게 어느 단위로 걸릴까?데이터베이스와 파일시스템을 안전하게 복구하는 방법은?Raid, 복제, 로깅시스템 내가 파일/db에 기록하는 데이터들은 얼마만큼 캐쉬가 이뤄지고각 복제본들은 어떻게 동기화 될 까?I/O의 정확한 메카니즘이 어떻게 될까?도커나 클라우드환경에서 내가 주는 입력이 들어가는 정확한 루트가 어떻게 될 까?자원을 분배함에 있어CPU, 메모리, 네트워크 등등어떤 정책과 분할이 최적일까?이런 고민들도 중요하지만,더 중요한건내가 상상조차 못해서 예시들지 못한 경우야.다른 기술을 보면서도 어 이거 ~~하겠네!!아 이거 ~~해서 이렇게 하는게 최선이었네!!이런것들을 이해하고무엇이 기술적으로 가능 한지그리고 무엇이 기술적으로 힘든지프로그래머의 관점이 생김.운체는 근데 내가보기엔네트워크랑 db의 선수과목으로서의 느낌이 강함.파일시스템의 연장선이 db고인터럽 트와 io의 연장선이 네트워크임.운체 스킵하고넷웍 디비 한사람들도 난 괜찮다고 생각함.충분히 깊이가 있지.어느 정도의 깊이가 제일 좋다!!라고 주장하려는건 아닌데어느 깊이 어느수준이던지간에프로그래머의 관점을 키우는건 중요함이 프로그래머의 관점이란체급이야개별적인 경험들을 엮어줄 수 있는 체급', metadata={'source': 'contents.txt'}), Document(page_content='일단 취업이 먼저라면, 혹은 반대로 여유가 너무 넘쳐서 취미로 프로그래밍하는 경우라면cs스킵후 부족한거 채워나가기이게 정답인거 같 음.다만 취업하려고해도 진짜 듣보가아니라면 cs를 다들 물어보니까리더가 되려면 cs 진지빨고하기중요한 판단이나 업무계획은 윗사람한테 맡기려면 cs핵심만 하기가 좋을 듯\n"\n2564207,프로그래머의 관점을 얻자,"', metadata={'source': 'contents.txt'}), Document(page_content='"\n2564208,리더가 되려면 cs필수. 그게 아니라면 핵심만,"\n일단 취업이 먼저라면, 혹은 반대로 여유가 너무 넘쳐서 취미로 프로그래밍하는 경우라면cs스킵후 부족한거 채워나가기이게 정답인거 같음.다만 취업하려고해도 진짜 듣 보가아니라면 cs를 다들 물어보니까리더가 되려면 cs 진지빨고하기중요한 판단이나 업무계획은 윗사람한테 맡기려면 cs핵심만 하기가 좋을듯\n"\n2564207,프로그래머의 관 점을 얻자,"', metadata={'source': 'contents.txt'}))
```

이렇게 원하는 문서를 가져왔음

다음에는 
```
print(docs[0].page_content)
```
이렇게 하면 답변만 깔끔하게 나올듯

```
db.as_retriever((search_kwargs={"k":3}))
```
- k 수 조정으로 관련 문서 갖고 옴
- threshold로 관련성 정도 조정

근데 일단 relevant document는 불러왔는데 이걸 바탕으로 LLM한테 먹이는 건 어케 하는거임???



### 자꾸 이 에러 뜸 → 스택오버플로우같은 데 검색 ㄱㄱ
```
LangChainDeprecationWarning: The function `__call__` was deprecated in LangChain 0.1.0 and will be removed in 0.2.0. Use invoke instead
```

```
retriever=db.as_retriever(search_kwargs={"k":3}) #해당 데이터베이스를 retriever로 설정

# docs = retriever.get_relevant_documents("프로그래머는 뭐하는 사람이야?")

prompt = hub.pull("rlm/rag-prompt", api_url="https://api.hub.langchain.com")
qa_chain = RetrievalQA.from_chain_type(
    llm=chat,
    retriever=retriever,
    # chain_type_kwargs={"prompt": prompt}
)


question = "프로그래머는 뭐하는 사람이야?"
result = qa_chain({"query": question})

print(result["result"])
```

답변:
```
프로그래머는 컴퓨터 소프트웨어를 개발하고 구현하는 사람입니다. 프로그래머는 프로그래밍 언어를 사용하여 컴퓨터 프로그램을 작성하고, 소프트웨어의 기능을 설계하고 개선하기도 합니다. 또한 버그를 수정하고 소프트웨어를 테스트하여 안정성과 성능을 개선하는 일도 담당합니다. 프로그래머는 현실의 문제를 해결하기 위해 컴퓨터를 사용하는데, 다양한 분야에서 필요로 하는 소프트웨어를 개발하는 역할을 수행합니다.
```

### 이거 RAG 제대로 안 되는 거 같은데 이유를 모르겠네

chain_type_kwargs 주석 처리 안 하고 돌리니까 답변이 더 안 좋은데??:
```
프로그래머는 컴퓨터 프로그램을 작성하고 개발하는 사람입니다.
```


담주에 langchain 더 공부 ㄱㄱ
