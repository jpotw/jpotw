# TIL 

## 2023-12-06


IMG/IMG_0079.jpeg

**한 것**

- 카카오톡 요약봇 만드는 중
- 하이라이트 친 부분까지 함 약 5시간 동안 한 거..
- 겁나 삽질했다.
- 에러는 안 뜨는데 무한루프 돌고 있어서 내일 뜯어봐야댈듯


**배운 점**

- 무조건 큰그림부터

- 앞으로 코딩 짤 때:

1. 큰그림부터 그린다. (세세한 건 일단 무시, 최대한 ai 시켜)
2. h1,h2,h3태그로 아웃라인을 잡아준다 (이래야 헛짓거리 안 함)
3. 각 기능을 구현한다. 일단 되는 것 대로 한다. done is better than perfect
4. 그 다음에 순서대로 막히는 거 해결한다. 안 되면 어떻게든 되게하라. google을 검색하든 개발자한테 물어보든..



## 2023-12-07

**한 것**
- 오늘도 한 5~6시간 정도 했다.
- 근데 무한 에러 -> 고치기 -> 에러 -> 고치기 여서 진도는 별로 못 나감.
- 사실 이렇게까지 오래 걸릴 , 시간을 투자할 가치가 있는 플젝인지도 모르겠는데 실력이 부족한 걸 어떡해~
- 다음에는 gpt4v 갖고 블로그 댓글 자동화 만들어볼 생각


### 헤맸던 함수 복습:

<uploaded_file을 받는다>
def make_new_file(uploaded_document):
    <현재 날짜와 타겟 날짜를 설정한다. 타겟 날짜는 하루 전>
    current_date = datetime.datetime.now()
    target_date = current_date - datetime.timedelta(days=1)
    <uploaded_document 문서를 이제 읽어줘야 한다. .read().decode 함수로 읽어주자>
    file = uploaded_document.read().decode("utf-8", errors="ignore")

    <날짜와 시간의 포맷을 정한다.>
    date_pattern = re.compile(r'\d{4}년 \d{1,2}월 \d{1,2}일')

    <여기서 겁나 헤맸는데, 06일 이런 식으로 출력 안 하고 싶으면 직접 한땀한땀 설정해줘야 한다.. 연도는 상관 X>
    target_date_str = f"%Y년 {target_date.month}월 {target_date.day}일"

    
    if target_date_str in file:
        st.write(f"{target_date_str}의 기록을 찾았습니다.")
        start_index = file.index(target_date_str)
        new_content = file[start_index:]
        st.write("수정되었습니다.")
        <이제 다음 함수에서 쓸 수정된 txt파일을 빼주자>
        return new_content
    else:
        st.write(f"{target_date_str}의 기록을 찾을 수 없습니다.")
        while target_date_str not in file:
            target_date_str -= datetime.timedelta(days=1)
        st.write(f"{target_date_str}의 기록을 찾았습니다.")
        start_index = file.index(target_date_str)
        new_content = file[start_index:]
        st.write("수정되었습니다.")
        <이제 다음 함수에서 쓸 수정된 txt파일을 빼주자>
        return new_content
   

### 꿀팁:
Ctrl / 안 되면 Microsoft 입력기로 바꿔주면 됨 


**배운 점**
오늘도 삽질 겁나 한 것 같다.
확실히 뭔가 뻘짓하고 있는 것 같으면 계속 붙잡지 말고 큰그림 다시 그리고 뭐가 막히는지 정확히 정의하고 구글링/chatgpt한테 물어보면 좀 풀리는 것 같다.
걍 chatgpt가 잘 짜주겠지, 하고 뇌빼고 물어보면 뇌 뺀 대답이 나온다. 근데 내가 꽤 깊이 생각한 질문에 대해서는 필요한 대답을 해줌.
설령 헛소리를 해도 헛소리인지 내가 알 수 있어서 다시 설명하면 잘 대답하는듯.

내일까진 미숙하더라도 완성해보고 싶다.



## 2023-12-08

**한 것**
- 3~4시간 정도
- 결론: 오늘은 포기. 내일 다시 도전해본다. 랭체인 강의 딱대


- 내가 구현하고 싶은 방향: 굉장히 심플하다.
1. 카카오톡 파일 업로드 
2. 자동으로 txt 자르고 핵심 내용 요약 및 링크를 제공한다.
3. 이후 추가 질문이 있으면 txt파일을 바탕으로 답변을 준다.


그럼 이걸 어떻게 구현하냐?

1. 카카오톡 파일 업로드 -> streamlit 업로더를 이용해 해결한다. (clear)


2. 자동으로 txt 자르고 핵심 내용 요약 및 링크를 제공한다. 
2.1. txt 파일 자르기 -> datetime을 이용해 일주일 전 텍스트를 확인할 수 있는 함수를 만든다. (clear)
2.2. 프롬프트에 맞춰서 핵심 내용 요약하기 -> 사실 그냥 요약이면 별 문제가 없는데 txt파일 기반 요약이라 복잡해진다. langchain에서 요약 기능은 크게 세 가지가 있는데 세 번째건 쳐다도 안 봤다. 첫 번째는 stuff 방식, 두 번째는 map reduce 방식. 근데 map reduce 방식이 훨 빡세 보이고 코드도 겁나 길어서 stuff 방식으로 해봄. 요약 제대로 안 됐음. 그래서 map reduce 방식 하다가 대가리 깨져서 포기. 


->걍 *3번이랑 퉁쳐서 해결해야겠다*고 판단 내림


3. 이후 추가 질문이 있으면 txt파일을 바탕으로 답변을 준다. -> RetrieverQA 함수로 구현
이미 streamlit에 Chat with my pdf 같은 거 만든 사람들 코드를 볼 수 있음. 그래서 그대로 복붙한 다음에 세부사항만 바꿨는데,
아니 토큰제한이 4096까지밖에 안 되는 거임;; 근데 토큰 4000이면 생각보다 너무 짧아서 당황스러웠다.


안 그래도 openai api 쓰는 거여서 돈 내면서 하는 건데 토큰 제한도 짜다? 이럴거면 gpt4나 앤트로픽 쓰지 ㅋㅋㅋ

---

그래서 ver4 branch를 새로 파서 구현한 것:
1. 카카오톡 내보내기를 통해 만든 txt파일을 업로드
2. 다운로드 버튼 클릭시, 일주일 전 내용까지 컷한 후 다운받게 함
3. URL 추출하기 버튼을 만들었음. https://로 시작하는 모든 링크를 추출할 수 있게 함. 하이퍼링크 기능을 넣어두어 바로 이동가능
4. 채팅방 이름과 채팅방에서 주로 하는 이야기를 기입할 수 있는 칸 만듦 -> 이걸 통해 자동으로 프롬프트 생성 (근데 이건 쓸데없이 복잡한 것 같아 걍 뺄까 고민중)
5. claude ai로 갈 수 있는 링크를 만듦.


아 뭔가 아쉽다. 
코딩 웨이렇게 어려움