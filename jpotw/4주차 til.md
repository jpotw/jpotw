

# 2023-12-26


### 함수 속의 함수

파이썬에서는 함수 안에 다른 함수를 넣는 게 가능하다. 생각보다 많이 쓰인다고 함

```
def add(n1,n2):
	return n1 + n2

...(생략)

def calculator(n1,n2,func):
	return func(n1,n2)
```

이렇게 할 경우

```
calculator(n1,n2,add)
```

를 통해 함수 안의 함수를 호출 가능하다.
*호출할 때는 괄호()를 쓰지 않는다*



### enumerate

순서가 있는(iterable) 객체(예: 리스트, 튜플, 문자열 등)에서 해당 항목의 인덱스나 값을 가져올 때 유용하다고 함


```
for index, value in enumerate(iterable):
```
이런 식으로 쓴다고 함.


그럼 딕셔너리도 ㄱㄴ?


ChatGPT:
```
딕셔너리의 경우, 기본적으로 `enumerate` 함수를 사용하면 딕셔너리의 키(key)만 반환됩니다.

딕셔너리의 키와 값을 함께 순회하려면 `enumerate`를 사용하는 것이 아니라 `.items()` 메서드를 사용해야 합니다. `.items()` 메서드는 딕셔너리의 키-값 쌍을 반환하므로 `enumerate`를 사용할 필요가 없습니다.
```

암튼 enumerate은 딕셔너리에 쓰기 부적합하다고 한다. 대신 dict.items()를 쓰면 된다고 함

```
dict = {"roddy": "rich", "post": "malone", "kanye": "west"}

for value in dict.items():
    print(value)
```

result:
```
('roddy', 'rich')
('post', 'malone')
('kanye', 'west')
```

굳



### 객체를 그냥 Print하면 안 되고 attribute이나 method를 불러와야 댐

```
object = Class()
```
에서 그냥 print하면
```
print(object)
```

```
<turtle.Turtle object at 0x00000274438524D0>
```
이런 식의 외계어가 나옴. 여기서 왼쪽은 이게 객체 라는 뜻이고 오른쪽은 컴퓨터에 저장된 값을 말함. 근데 우리가 궁금한 건 이게 아니라 그 객체 안에 저장된 정보들이다. 

이 정보들을 불러오기 위해서는 **attribute이나 method를 불러와야 한다.**

Ex.
```
object = Class()
print(object.x)
```
또는
```
object = Class()
print(object.x())
```

이렇게 하는 거임 ㅇㅋ?



# 광교

1. 회원가입하는 법
2. 채팅 히스토리 불러오는 법
treehouse
todo
- 도메인 bookshorts.store로 바꿈
- flask rest api 강의 수강
- bookshorts 코드 구상

# 2023-12-27

## Flask 공부

## 템플릿

```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run()
```


`app = Flask(__name__)` means you're telling your Python program to create a web application using Flask, and the name of this application should be based on the name of your Python script.

### debug = True

디버깅 모드를 실행하는거, 코드 실행 과정에 대해 더 상세한 정보를 듣게 됨

### request.args.get은 무슨 뜻인가

1. **`request`**: 클라이언트의 정보들을 담은 Flask의 global object.
    
2. **`request.args`**: query parameter 부분
    
3. **`request.args.get`**: key를 넣으면 corresponding한 value를 출력함


뒤에 꼭 ?(query)를 붙여야 함

ex.
```
http://127.0.0.1:5000/?name=Jerry
```



### URL Argument

좀 더 깔끔하게 코딩을 하기 위해 request 대신 url argument를 쓸 수 있다.

쓰는 법:

```
from flask import Flask

app = Flask(__name__)

@app.route('/<a>')
def (b):
	return '{}'.form(c)
```

라고 할 때 a는 URL의 variable 파트다. a에서 사용자가 기입한 str을 b로 넘겨준다. c는 b를 str의 포맷으로 변경을 하는데 위 케이스의 경우 어차피 b가 str이기 때문에 큰 상관이 없으나, 만약 a가 <int:a>와 같은 경우라면, 유용하게 쓸 수 있다.



# 2023-12-28

## 한 것
- Flask Documentation → 안 읽힘
- Flask Basics 강의 → 너무 빠르다. 뭔소린지 잘 모르겠다.
	- 강의를 들어도 모르는 부분은 어떡하지?
		예시:
		```
		@app.route('/builder')
		def builder():
			return render_template(
			'builder.html',
			**saves=get_saved_data()** -> 어떤 역할이고 어느 맥락에서 튀어나온건지 설명 X,
			d
			options=DEFAULTS -> 무슨 소리임..?
			)
		```
	
- bookshorts에 회원가입 기능을 추가했다. (구글 복붙) (/register) github에 올렸다.
- 이제 로그인 기능을 구현해야되는데 대충 다음과 같은 프로세스일듯:

```
# 로그인 페이지
@app.route("/loginpage")

# 이미 db에 로그인이 돼 있는 경우 서비스 이용
if input in db:
	redirect('/index.html')


else:
	redirect('/register.html)
```

- 이 기능을 구현하면 내가 질문했던 내용을 확인할 수 있게 만들어야됨


	render_template을 이용하면 temlpate을 python 으로 불러올 수 있음.
```
render_template(ex.html, ex=ex)
```

요런 식으로

html 파일들은 templates이라는 이름의 파일에 저장하고 css, js 파일들은 static이라는 이름의 파일 안에 저장한다.




url_for('file_name')


request.form 을 통해 form 의 내용(ImmutableMultiDict)을 불러올 수 있음


dict key and value 둘다 갖고 오기 위해서는 request.form.items() → 이려면 튜플 형태의 dict로 반환함


**Redirect:** 원래 request된 url와 다른 주소로 보낼때 씀
- **Use cases:**

- **Ex:** `return redirect(url_for('home'))` sends the user to the homepage.

**URL_for:** 
- `endpoint_name`: The name of the route you want to generate a URL for (defined using `@app.route()`).
- `argument1`, `argument2`, etc.: Values for any variable parts of the URL (specified as `<variable>` in the route definition).
```
redirect(url_for('endpoint_name', arg1=value1, arg2=value2))
```

**JSON dumps:** python data structure를 json으로 변환
**JSON loads**: Str → Python(dictionary)

## post, get method 차이점

추가로 POST, GET은 둘다 **정보를 서버로 전달하기위한** 기능입니다.

차이점은 
**GET** : 저장된 정보를 단순히 요청할때 사용되며, url에 정보의 이름이 노출 = 한마디로 데이터에 수정이 있는경우 (게시글 작성, 회원가입 정보 받기)

**POST** : url에 쿼리가 노출되지 않으며, 요청시 데이터 양에 제한이 없음. = 단순히 정보를 보기위한 용도 (게시글 읽기, 회원가입창 보기)



항상 무언가를 불러올 때 /를 쓰는 걸 기억할 것





# 2023-12-29

- 09:27 start - Do it, 점프투 Flask 읽어보려고 함


## 이번주 주말까지 목표: bookshorts 회원가입/로그인 구현 & 유저 db저장 & deploy

기능 단위로 쪼개보기
### 1. 회원가입/ 로그인 구현:
```
# 로그인 페이지
@app.route("/loginpage")

# 이미 db에 로그인이 돼 있는 경우 서비스 이용
if input in db:
	redirect('/index.html')


else:
	redirect('/register.html)
```

### 2. 유저 db 저장
#### 유저 식별(by cookie?)

#### 유저 질문, 챗봇 답변의 내용을 db에 저장
SQLAlchemy를 통해 QandA DB 생성
	QandA에는 무엇이 들어가야 하는가?
	-현재시간(datetime.now())
	- 책 제목
	- 저자명
	- ~~질문 (생략 가능 → 어차피 "요약해줘"로 공통임)~~
	- 답변

- 히스토리 인터페이스 추가
- 히스토리 인터페이스를 클릭(button)하면 db에 저장된 메세지 내용을 확인할 수 있도록 함

### 3. deploy
- amazon aws lambda 사용



## 배운 점

- db를 생성할 때 primary key로 설정하면 구분이 가능해진다. id처럼 **배타성이 필요한 것들**에는 primary key=True로 설정해줄 것
```
class db:
	id = db.Column(..., primary_key = True)
```

- **db에 저장되는 흐름.** 
	- 1. 우선 html form에 맞게 python 객체들을 설정(Ex. input이 4개라면 각각 4개의 객체를 설정)
	- 2. 이후 models.py(db 템플릿 잇는곳)의 class를 불러온다. 그 class에 파이썬 객체를 집어넣는다.
	- 3. db.session.add() 하고 commit()까지 해주면 db에 저장완
- flask db init 을 사용하면 db 관리 초기파일들을 migrations라는 디렉터리에 자동 생성
	- cf) db로할 수 있는 건 flask db라고 치면 나오는데 보통은 **flask db migrate**(모델 새로 생성/변경 시 사용), flask db upgrade(모델 변경 내용을 실제 db에 적용시 사용) 두가지를 주로 사용한다고 함.	
- SQLALCHEMY_DATABASE_URL 아니고 URI임,..
- 걍 import models하니까 안 됨. from bookshorts import models 하니까 됨.. ㅇㅇ
- 모든 model Class 는 (db.Model)을 가져야 한다 안 그러면 db에 저장되지 않는다.
```
- @bp.route('/summarize/', methods=['POST'])
```
- bp.route 설정할 때 요런식으로 ㄱㄱ
- get = url에 입력하면 접속하는 방식 post = 버튼/내장함수를 통해 이동 가능(만약 post 방식만 허용할 경우, url 입력하면 405오류가 뜬다.)
- init에서 app.register_blueprint()할 경우, 하나만 가능하더라. 두 개 하면 2 arguments are allowed but you wrote 3 이런식으로 나옴. 그래서 main_views.py에 bp를 하나만 설정하고 거기에 다 때려박으셈
- from models import Class 했더니 갑자기 No module found in models가 뜸 → 확인해봤더니 상대경로로 설정해야 됨 (나중에 공부할 kw : **절대경로 & 상대경로**) → 암튼 그래서 '..models'로 바꿔줬더니 잘 되더라..
- flask run을 해서 나온 메인 페이지에 제목과 저자를 쓰고 버튼을 누르면 /summarize로 이동하는 동시에 db에 저장돼야 하는데, internal error가 뜨고 있음. 가정:  answer가 not null인데 안 넣어져서..인듯하다.
- 함수 중에 kwargs가 있는 함수는 딕셔너리형태('x=y')로 변수에 값을 저장해줄 수 있다. 
	- 예를 들어 return render_template('summarize.html', book_id=bookinfo.id) 에서는 book_id를 summarize.html에서 쓸 수 있음.
- db에 데이터가 저장됐는지 확인하는 쉬운 방법: flask shell 킨 후 ModelName.query.all() 입력하면 입력된 데이터들이 쭉 나옴.
```
@bp.route('/summarize', methods=['POST'])
def handle_summarize():
    bookinfo = BookInfo()
    bookinfo.title = request.form['title']
    bookinfo.author = request.form['author']
    bookinfo.create_date=datetime.utcnow()
    # 데이터베이스에 저장
    db.session.add(bookinfo)
    db.session.commit()

    # 요약 페이지로 리디렉션
    return render_template('summarize.html', book_id=bookinfo.id)
```
이 상태였는데도 두 가지 문제가 있었는데:
1. db에 저장이 안 됐음
2. summarize.html이 안 나왔음.

- 해결: 결국 한 가지 문제에서 기인했는데 model을 설정할 때 nullable=False라 해놓고(입력값이 없으면 저장이 안 된다.) 입력하지 않은 상태에서 add, commit을 할 경우 오류가 뜬다. 나의 경우 answer가 아직 나오지 않은 상태에서 answer를 nullable=False라고 해서 계속 오류가 떴었음. nullable=True로 하니 문제 해결.

## 궁금한 점
Terminal:
```
PS C:\bookshortsv2> set FLASK_APP=__init__.py
PS C:\bookshortsv2> flask run
Usage: flask run [OPTIONS]
Try 'flask run --help' for help.

Error: Could not locate a Flask application. Use the 'flask --app' option, 'FLASK_APP' environment variable, or a 'wsgi.py' or 'app.py' file in the current directory.  
```
- 왜 flask 환경변수를 설정해도 계속 app.py 나 wsgi.py를 찾으려 하는거임. 
- 해결:
	1. 우선 Windows Powershell 환경에서는 다음과 같이 환경변수를 설정해준다: 
	   ```
	   $ env:FLASK_APP="내가 지정하고 싶은 파일or폴더"
	   ```

	2. 그럼에도 계속 안 됐는데(Error: Failed to find Flask application or factory in module 'bookshorts'. Use 'bookshorts:name' to specify one.) 이는 app이 제대로 정의됐을 때 flask가 run할 수 있기 때문이다. 나의 경우, def create_app() function에서 return app을 빼먹었음. 무조건 create_app이 정의돼 있어야 함.


- What is app.register_blueprint function ?
- 왜 from .views import main_views에서 .이 붙음? 
	- → 이게 상대경로라는 건데 .의 경우 현재 디렉토리(폴더) 안에 있는 파일을 의미함. 이 이후로 .이 하나 늘수록 부모 디렉토리(더 상위의 폴더)를 의미하는 거임. 그래서 이론상 .. ... .... 등이 나올 수 있음.
- 헤맸던 부분: 기존 html 파일에서 버튼을 클릭하면 다른 페이지가 나오는 동시에 입력 내용이 db에 저장되게 하고 싶었다. JS의 Ajax만 가능한 줄 알았는데, post만 allow하는 방식으로 하면 가능.. ㅇㅇ


## 예쁘게 말고 되게,최소 기능 구현부터 집중, 안 되면 쪼갤 것, 할 수 있는 건 분명히 있다
→ 내가 이런 명언을 햇다니ㅜ

# 2023-12-30

- 어떤 route로 갔을떄 return render_template 또는 redirect하기 전까지는 그냥 유지되는건가...??
	- 그러니까 
- return rendertemplate을 쓰면서 설정하는 값들은 html 에서 {{ }} 안에서 사용 가능하다
- password authentication을 위해 SECRET_KEY를 config.py에 설정해준다.
- book_list 관련해서 return render template 썼는데도 오류 뜬 이유: for book in booklist이니까 book을 써야

# 2023-12-31
- 다른 views를 등록하려면 init 파일에 app.register_blueprint()를 꼭 추가해주자.
- url_for 기능을 이용해   route를 redirect하면 나중에 코드를 수정해도 redirect부분들을 굳이 건드릴 필요 X
- js에서 .env 쓰는 방법
```
terminal:
npm install dotenv


require('dotenv').config();

const dbPassword = process.env.DATABASE_PASSWORD;
const apiKey = process.env.API_KEY;

```

- db에 password 저장하기 전에 password hash를 걸어놔야 함. 안 그러면심각한 보안이슈


## 회원가입 → DB 저장 → 로그인 / DB 비교 → 통과

- 이 기능을 구현하는 데 정~말 오래 걸림. 
- html파일에 forms를 덧붙여서 하다가 쓸데없이 복잡해지는 거 같아서 flask WTF로 하려다가 또 html 방법을 찾아서 일단 기록:

1. **우선 회원가입 기능을 만들어준다**. (views/register_views.py)

```
bp = Blueprint('register', __name__, url_prefix='/')

@bp.route('/', methods=['GET', 'POST'])

def register():
    if request.method == 'GET':
        return render_template("register.html")
    else: # Post Method 로 받을 경우
        userid = request.form.get('userid') # html 파일에서 name이 userid인 내용을 가져온다.
        username = request.form.get('username')
        password = request.form.get('password')
        re_password = request.form.get('re_password')
        print(password) # 들어오나 확인해볼 수 있다.

        if not (userid and username and password and re_password) :
            return "모두 입력해주세요"

        elif password != re_password:
            return "비밀번호를 확인해주세요"

        else: #모두 입력이 정상적으로 되었다면 밑에명령실행(DB에 입력됨) (python 객체 -> Database)
            fcuser = Fcuser()
            fcuser.password = generate_password_hash(password)  #pw을 hash한 상태로 암호화해야 됨     
            fcuser.userid = userid
            fcuser.username = username      
            db.session.add(fcuser)
            db.session.commit()

            return "회원가입 완료"
```

2. **회원가입 template을 만들어준다**: templates/register.html

```
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>회원가입</title>
    // css 생략 //

</head>
<body>
    <div class = "container">
        <div class="row mt-5">
            <h1>회원가입</h1>
        </div>
        <div class="row mt-5">
        <div class="col-12">
            <form method="POST" >
                <div class="form-group">
                    <label for="userid">아이디</label>
                    <input type="text" class="form-control" id="userid" placeholder="아이디" name="userid"/>
                </div>
                <div class="form-group">
                    <label for="username">사용자 이름</label>
                    <input type="text" class="form-control" id="username" placeholder="사용자 이름" name="username"/>
                </div>
                <div class="form-group">
                    <label for="password">비밀번호</label>
                    <input type="password" class="form-control" id="password" placeholder="비밀번호" name="password"/>
                </div>
                <div class="form-group">
                    <label for="re_password">비밀번호확인</label>
                    <input type="password" class="form-control" id="re_password" placeholder="비밀번호확인" name="re_password"/>
                </div>
                <button type="submit" class="btn btn-primary">등록</button>
            </form>
        </div>
        </div>
</body>
</html>
```


1,2를 통해 회원가입html 의 input이 회원가입py를 통해 db에 저장되도록 한다.

3. **로그인 route을 설정해준다.** (views/auth_views.py)

```
// from, import 생략 //

  

bp = Blueprint('login', __name__, url_prefix='/login')

@bp.route('/', methods=['GET', 'POST'])
def login():
    if request.method == 'GET': # Get Method: url로 타이핑하면 나오는 method
        return render_template('login.html')
    if request.method == 'POST': # Post method: 버튼을 클릭하는 등 html 안의 내장된 방식(?)으로 나오는 거.
        error=None
        userid = request.form.get('userid') # `request.form.get('userid')`는 클라이언트로부터 받은 POST 요청의 form 데이터에서 'userid'라는 이름의 필드 값을 가져오는 것이라고 함
        password = request.form.get('password')
        print(userid, password)
        if not (userid and password):
            return "아이디와 비밀번호를 입력해주세요"
        else:
            fcuser = Fcuser.query.filter_by(userid=userid).first()
            if check_password_hash(fcuser.password, password):
                session['userid'] = userid
                return redirect(url_for('main.bookinfo'))
            else:
                return "아이디와 비밀번호를 확인해주세요"
```
마찬가지로 로그인 input을 받을 python 파일을 만들어준다.

4. 로그인 template 파일을 만든다: (templates/login.html)

```
{% extends "base.html" %}
{% block title %}로그인{% endblock %}
{% block content %}
</head>
<body>
    <h2>Login</h2>
    {% if error %}
        <p>{{ error }}</p>
    {% endif %}
    <form method="POST", action="{{ url_for('login.login') }}"> # button(submit)을 누르면 3번에서 만든 login route으로 post 방식으로 전송함
        <label for="userid">ID</label>
        <input type="text" id="userid" name="userid" required>
        <label for="password">Password</label>
        <input type="password" id="password" name="password" required>
        <button type="submit" class="button">로그인</button>
    </form>
        <a href="{{ url_for('main.register') }}" class="button">회원가입하기</a>
</body>
</html>
{% endblock %}
```


5. POST 방식으로 받은 data를 db 내의 data와 비교하여 일치하면 main으로 redirect한다. (3번 참조)




-  return을 Str 말고 alert으로 바꾸는 법: python script에서는 alert을 직접 쓰지는 못한다. 그래서 html template 에 javascirpt script를 추가해줘서 거기로 redirect한 후 python에서 작성한 메세지를 alert message로 print시키면 됨.
- views 안에 route가 잇는 경우, {view 이름}.{route 매소드} 식으로 불러줘야 함
- AJAX를 사용할 경우 새로고침을 하지 않고도 페이지가 로딩될 수 있다고 함.
- AJAX 방식 쓰려다 자꾸 화면에 이상한 거 떠가지고 + 어차피 새 html파일 만드는 게 더 깔끔해서 그냥 summarize.html파일 만들기로 결정
- flask 에서 redirect하는 방식은 GET 방식임. Flask return으로는 POST 방식으로 보낼 수가 없음. POST 방식 사용할 거면 AJAX나 form 방식을 이용해야 됨. url 접근을 막는 차선책은 return render_tempate.
- render_template을 통해 넘긴 값들은 "{{ }}" 안에 써줘야 한다

## base.html 사용하는 법

base.html에 이런 식으로 짜준 후
```
 { % block a %} { % endblock %}

a의 예시: css, title, content 등등
```

templates의 다른 html파일들에 사용 ㄱㄱ

- copilot 사용 꿀팁 - 페이지 왔다갔다하면서 css 업그레이드해달라고 하면 잘해줌 ㅋ
- base.html 쓸 때 띄어쓰기 유의하자 { % block title % } 이런 식으로


일단 얼추 끝났? 하루 이틀이면 완성할 수 있을듯하다.
다음에 뭐할지 생각해보자. 의사결정 소프트웨어 ??