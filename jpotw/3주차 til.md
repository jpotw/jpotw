# **이번 주 한 거**

- 카카오톡이 AI 요약 기능을 발표함 → 망햇다 → 인줄 알았는데 써보니까 수준이 ... 그만 알아보자
- gpt4v를 이용한 크롤링 ver.1 만듦. 딱히 쓸일은 없어보인다.
- - 떠먹여주기식 강의 듣는중(udemy)..ㅂㄷㅂㄷ
- 카톡 요약기 다 만들고 이제 뭘 해야될지 고민중
- 토이 프로젝트?
	- **가정1: 앞으로 정보는 더욱더 많아질 것이다 + 사람들은 요약된 것(TL;DR)을 좋아한다. → 요약 서비스는 점점 필요해진다.**
		- 요약 서비스의 핵심: 얼마나 정확한가? 얼마나 핵심적인가? 얼마나 간단한가? 
		- 근데 이미 요약 서비스는 많다.
		- 그럼에도 요약 관련 서비스를 만들어보고 싶은데 ...당장은 차별화된 아이디어가 안 떠오름. 
		  
	- **가정2: 사람들은 나 자신에 대해 알고 싶어한다.** 
		- (Mbti 테스트가 유행하는 것도 비슷한 맥락) 
		- 나의 데이터를 넣으면 분석 및 그걸 바탕으로 어떤 컨텐츠를 만들어주는 서비스?? → 엄청 어려울 거 같긴 함.
		- 근데 내가 이것저것 기입하면 불편하니까 이미 있는 데이터로. 
		- 데이터는 어디있는데? **카카오톡, 일기, 블로그 등**
		- 이 데이터를 기반으로 뭘 할 수 있는데? 아직 모르겟음..
		  
- 강의 들으면서 기본기 쌓기?
	- Udemy python 기초 강의 듣기?
- or..??
# 2023-12-20

## What I've Learned
- Fork한 파일이 javascript를 쓰길래 이것저것 알아야 했음.

### subprocess.run function에 대하여

구조: 
```
subprocess.run([command, first_argument, second_argument, ..., nth_argument])
```

- command: 터미널에서 처음에 적는거 ex. python, node etc.
- arg: They are typically options, flags, or other parameters that the command needs to execute properly.


### Javascript에서 .env 불러오는 법

terminal
```
npm install dotenv
```

js file
```
require('dotenv').config(); 
// Now you can access the variables using process.env 
const dbUser = process.env.DATABASE_USER; 
const dbPassword = process.env.DATABASE_PASSWORD;
```



### git remote origin 바꾸는 법

현재 위치 확인
```
git remote -v
```

바꾸고 싶을 때 
```
git remote set-url origin https:// ...
```


### 모듈 vs 클래스
- 모듈은 약간 관련된 도구 모음 느낌
- 클래스는 특정 객체의 청사진 (암튼 도구 모음 같은 느낌은 아님)


# 2023-12-22

### 배운 점

- 클래스에 대해 배움(앤젤라 선생님 감사합니다 ㅎㅎㅎ)

class에서 가장 중요한 건 attribute와 method인데

attribute은 초기 설정이고 method는 클래스 안에서 정의된 함수다.

attribute은
```
__init__
```
이라는 특별한 함수 안에서 정의되는데,

```
class Class:

	def __init__(self, a, b):
		self.j=a
		self.p=b
```

대충 이런 식으로 쓰인다.

여기서 self란, 나중에 이 클래스를 받는 객체를 말함.

더 자세한 내용은 개발 블로그에 적음 https://jpotw.tistory.com/entry/%EB%B0%94%EB%B3%B4%EB%93%A4%EC%9D%84-%EC%9C%84%ED%95%9C-def-init-%EC%84%A4%EB%AA%85



# 2023-12-24

### qj 선생님의 assert 복습


**assert**이라는 키워드를 통해 내가 쓴 코드가 말이 되는지 확인할 수 있음.
만약 assert에서 False가 나오면 에러(AssertionError)가 뜸.

```
assert isinstance(object, 예상 데이터 타입)
```

이렇게 찍어서 아무런 에러 메세지가 없다면 object = 내가 쓴 데이터 타입이 맞다.

Ex.
```
def input_printer(**kwargs):

    assert isinstance(kwargs, dict)

    assert kwargs == {"myname": "jp"}

  

#assert는 False가 들어가면 에러가 발생 -> 아무런 문제 없이 찍힐 경우 True라는 뜻

input_printer(myname="jp")
```
아무런 변화 없음

근데
```
def input_printer(**kwargs):

    assert isinstance(kwargs, tuple)

    assert kwargs == {"myname": "jp"}

  

#assert는 False가 들어가면 에러가 발생 -> 아무런 문제 없이 찍힐 경우 True라는 뜻

input_printer(myname="jp")
```

Traceback (most recent call last):
...
AssertionError

에러가 뜬다.