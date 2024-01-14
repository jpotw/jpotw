



# 각종 템플릿 



##### `cmd batch file`

```cmd
@echo off
cd c:/file
set FLASK_APP=app
set FLASK_DEBUG=true
c:/venvs/myenv/scripts/activate
```

보충설명:
- 우선 venvs 파일을 C:/ directory에 생성함 (m venv venvs)
- 환경변수에 들어가 PATH를 새로 만든다(C:/venvs)
- 위에 batch 파일을 만들어준다.
- .cmd로 venvs파일 안에 저장한다.
- 명령 프롬프트에 파일 제목(ex. myproject (cmd 생략)) 입력하면 자동세팅완

##### `app/templates/base.html`

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    {% block css %}
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
    {% endblock %}
    <title>{% block title %}{% endblock %} - insight finder</title>
</head>
<body>
{% block content %}
{% endblock %}
</body>
</html>
```



### DB 관련

 db를 생성할 때 primary key로 설정하면 구분이 가능해진다. id처럼 **배타성이 필요한 것들**에는 primary key=True로 설정해줄 것
```
class db:
	id = db.Column(..., primary_key = True)
```

- **db에 저장되는 흐름.** 
	- 1. 우선 html form에 맞게 python 객체들을 설정(Ex. input이 4개라면 각각 4개의 객체를 설정)
	- 2. 이후 models.py(db 템플릿 잇는곳)의 class를 불러온다. 그 class에 파이썬 객체를 집어넣는다.
	- 3. db.session.add() 하고 commit()까지 해주면 db에 저장완

flask db init 을 사용하면 db 관리 초기파일들을 migrations라는 디렉터리에 자동 생성 

 cf) db로할 수 있는 건 flask db라고 치면 나오는데 주로 db init(처음 생성시 한 번만 사용), db migrate, db upgrade만 쓴다.



# 기타 꿀팁

기왕이면 bp, route, def, 파일명을 다 통일하는 게 좋다. 괜히 나중에 디버깅할 때 헷갈림. 

login 기능이면 login 이런 식으로 다 통일 ㄱㄱ



return을 Str 말고 alert으로 바꾸는 법: python script에서는 alert을 직접 쓰지는 못한다. 그래서 html template 에 javascirpt script를 추가해줘서 거기로 redirect한 후 python에서 작성한 메세지를 alert message로 print시키면 됨.





# 자주 나오는 디버그 오류



##### `__init__.py`

- 다른 views를 등록하려면 init 파일에 app.register_blueprint()를 꼭 추가해주자.
- 

##### `render_template을 통해 flask 값을 html로 보낼 때`

우선 jinja2 문법에 맞게 설정해줘야 한다. jinja2문법에 맞게란, python에서 보낸 값을 a라고 하면

`{{ a }}` 이런 식으로 하자. 가끔씩 하나 빈칸 안 만들어서 고생하지 말고.


그리고 꼭 보낸 이름을 제대로 쓰는지 확인!!


마지막으로 for loop을 쓰고 싶을 때는 이런 식으로 쓰셈

apple을 보냈다고 치면

```
{% for a in apple %}
	{{ a }}
{% endfor %}
```

이렇게 안에다가 {{}}을 써줄 것





**SQLALCHEMY_DATABASE_URL 아니고 URI임**

- 모든 model Class 는 (db.Model)을 가져야 한다 안 그러면 db에 저장되지 않는다.
##### `db가 문제가 생겼을 때`

Curently I'm using SQLAlchemy which is ORM and that means it's controllable with Python Grammar.

Now, there are only few commands you need to make when you interact with Database.```

###### `flask db init`
- `flask db init` 명령은 데이터베이스를 관리하는 초기 파일들을 다음처럼 migrations 디렉터리에 자동으로 생성한다. 
- 데이터베이스를 초기화하는 `flask db init` 명령은 최초 한 번만 수행하면 된다.

###### `flask db migrate`
- 모델을 새로 생성하거나 변경할 때 사용 (실행하면 작업파일이 생성된다.)

When you make a model, make sure to update it w/ `flask db migrate, flask db upgrade` !!


###### `flask db upgrade`
- 모델의 변경 내용을 실제 데이터베이스에 적용할 때 사용 (위에서 생성된 작업파일을 실행하여 데이터베이스를 변경한다.)

출처: [2-04 모델로 데이터 처리하기 - 점프 투 플라스크 (wikidocs.net)](https://wikidocs.net/81045)


##### `from .views`

새로운 views를 만들었을 때는 app.py 혹은 `__init__.py`에서 app.register_blueprint(x.bp)를 추가해주자. 


##### `SQLite 오류 방지`

```python
from flask import Flask
from flask_migrate import Migrate
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy import MetaData

import config

naming_convention = {
    "ix": 'ix_%(column_0_label)s',
    "uq": "uq_%(table_name)s_%(column_0_name)s",
    "ck": "ck_%(table_name)s_%(column_0_name)s",
    "fk": "fk_%(table_name)s_%(column_0_name)s_%(referred_table_name)s",
    "pk": "pk_%(table_name)s"
}
db = SQLAlchemy(metadata=MetaData(naming_convention=naming_convention))
migrate = Migrate()

def create_app():
    app = Flask(__name__)
    app.config.from_object(config)

    # ORM
    db.init_app(app)
    if app.config['SQLALCHEMY_DATABASE_URI'].startswith("sqlite"):
        migrate.init_app(app, db, render_as_batch=True)
    else:
        migrate.init_app(app, db)
    from . import models
    (... 생략 ...)

```



# 용어 설명

##### **`url_for()`:** 

URL to specific **function**.

why use `url_for()` instead of `render_template()`? 
- can avoid unexpected behavior caused by relative path
	- easy to change

```python
url_for(# bpname(없으면 '/').funcname)
```
##### **`HTTP Methods`**  
By default, a route only answers to `GET` requests. You can use the `methods` argument of the [`route()`](https://flask.palletsprojects.com/en/3.0.x/api/#flask.Flask.route "flask.Flask.route") decorator to handle different HTTP methods.
Ex.

```python
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()
```


or

```python
@app.get('/login')
def login_get():
    return show_the_login_form()

@app.post('/login')
def login_post():
    return do_the_login()
```


##### **`Static files`**

in `templates/base.html':
```python
url_for('static', filename='style.css')
```


##### `Request`
In Flask this information is provided by the global `request` object

```python
request.form['user']
```


##### File Uploads

make sure not to forget to set the `enctype="multipart/form-data"` attribute on your HTML form, otherwise the browser will not transmit your files at all.

 `save()` method allows you to store that file on the filesystem of the server

```python
from flask import request

@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        f = request.files['the_file']
        f.save('/var/www/uploads/uploaded_file.txt')
    ...
```



##### `Cookies`

Reading cookies:
```python
from flask import request

@app.route('/')
def index():
    username = request.cookies.get('username')
    # use cookies.get(key) instead of cookies[key] to not get a
    # KeyError if the cookie is missing.
```
Storing cookies:

```python
from flask import make_response

@app.route('/')
def index():
    resp = make_response(render_template(...))
    resp.set_cookie('username', 'the username')
    return resp
```



##### page not found 404 error handiling

```python
from flask import render_template

@app.errorhandler(404)
def page_not_found(error):
    return render_template('page_not_found.html'), 404
```


##### `Requesting API in JSON Format`!!

```python
@app.route("/me")
def me_api():
    user = get_current_user()
    return {
        "username": user.username,
        "theme": user.theme,
        "image": url_for("user_image", filename=user.image),
    }

@app.route("/users")
def users_api():
    users = get_all_users()
    return [user.to_json() for user in users]
```


##### `Sessions`

```python
from flask import session

# Set the secret key to some random bytes. Keep this really secret!
app.secret_key = b'_5#y2L"F4Q8z\n\xec]/'

@app.route('/')
def index():
    if 'username' in session:
        return f'Logged in as {session["username"]}'
    return 'You are not logged in'

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))
    return '''
        <form method="post">
            <p><input type=text name=username>
            <p><input type=submit value=Login>
        </form>
    '''

@app.route('/logout')
def logout():
    # remove the username from the session if it's there
    session.pop('username', None)
    return redirect(url_for('index'))
```

how to generate good secret keys:
```terminal
$ python -c 'import secrets; print(secrets.token_hex())'
'192b9bdd22ab9ed4d12e236c78afcb9a393ec15f71bbf5dc987d54727823bcbf'
```

A session makes it possible to remember information from one request to another.



##### `Flask-WTF`

Makes it easier to make input templates in html

```bash
pip install flask_wtf
```

**How to use**

forms.py
```python
from flask_wtf import FlaskFrom
from wtforms import StringField, PasswordField, SubmitField
from wtforms.validators import DataRequired

class LoginForm(FlaskForm): # class 이름(FlaskForm)
    username = StringField('Username', validators=[DataRequired()]) 
    password = PasswordField('Password', validators=[DataRequired()])
    submit = SubmitField('Login')
```

templates/login.html
```html
{% extends "base.html" %}
{% block title %}로그인{% endblock %}
{% block content %}

    <h2>Login</h2>
    <form method="post", action="{{ url_for('login.login') }}">
        {{ form.hidden_tag() }} // The `form.hidden_tag()` in Flask-WTF generates hidden CSRF (Cross-Site Request Forgery) token input fields for your form. //
        {{ form.username.label }} {{ form.username(size=32) }}
        {{ form.password.label }} {{ form.password(size=32) }}
        {{ form.submit() }}
    </form>    
{% endblock %}
```

views/auth_views.py
```python
from flask import Blueprint

bp = bp = Blueprint('main', __name__, url_prefix='/')

@bp.route('/')
def login():
	form = LoginForm()
	return render_template(login.html, form=form) # 이게 ㄹㅇ 간편함

```

**Types of fields**(EX)
- StringField
- DateField((_default field arguments_, _format='%Y-%m-%d'_))
- EmailField(_default field arguments_)
- FileField(_default field arguments_)

for more visit: [Fields — WTForms Documentation (3.0.x)](https://wtforms.readthedocs.io/en/3.0.x/fields/#basic-fields)


**Things you can do with Flask-WTF**

1. **Data Entry:**
    
    - Allow users to input text data, such as names, addresses, and other personal information.
    - Provide text areas for longer inputs, like comments or messages.
2. **Selections and Choices:**
    
    - Include radio buttons for selecting one option from a set of choices.
    - Use checkboxes for selecting multiple options from a list.
    - Incorporate drop-down lists for compact, single-choice selections.
    - Implement multi-select lists to choose multiple items.
3. **File Uploads:**
    
    - Enable users to upload files, such as documents, photos, and videos.
4. **User Authentication:**
    
    - Facilitate login forms for user authentication.
    - Provide registration forms for account creation.
5. **Data Submission:**
    
    - Submit form data to a server for processing.
    - Use GET or POST methods to send data, depending on the nature of the data and the application's requirements.
6. **Feedback and Surveys:**
    
    - Create survey forms to collect feedback, opinions, or other information.
    - Implement rating systems using radio buttons or sliders.
7. **Search Functionality:**
    
    - Offer search forms to query databases or search website content.
8. **Order and Reservation Forms:**
    
    - Enable e-commerce functionalities with order forms, including product selection, billing, and shipping details.
    - Allow booking and reservations for services or events.
9. **Email and Contact Forms:**
    
    - Provide a way for users to contact website administrators or support services.
10. **Interactive Controls:**
    
    - Utilize range sliders for selecting numerical values.
    - Include date pickers for easy date input.
11. **Validation and Error Feedback:**
    
    - Implement client-side validation to check the correctness of input before submission.
    - Display error messages or indications for invalid inputs.
12. **Dynamic Form Elements:**
    
    - Use JavaScript to add interactivity, like dynamically adding or removing form fields based on user actions.
13. **Styling and Customization:**
    
    - Customize the appearance of forms with CSS to match the website's design.
    - Apply custom styles to form controls for a consistent user interface.


**How to use Flaskform with models(database)**:

```python

@app.route('/')
def index(): 
	form = MyForm() 
	if form.validate_on_submit(): 
		user = User(name=form.name.data) 
		db.session.add(user) 
		db.session.commit() 
		return redirect('/success') 
	return render_template('index.html', form=form)

```

**How to use Flaskform with id, password verification**:

```python
@bp.route('/', methods=['GET', 'POST'])

def login():
    form = LoginForm()
    if request.method == 'GET':
        return render_template('form.html', form=form)

    else:
        if form.validate_on_submit():
            username = form.username.data  # 폼.변수.data 형식으로 extract해온다
            password = form.password.data
            # Retrieve user from the database
            user = Fcuser.query.filter_by(username=username).first() # Bring user info from db
            if user and check_password_hash(user.password, password):
                # Valid username and password
                session['user_id'] = user.id
                return redirect(url_for('main.bookinfo'))
            else:
                # Invalid username or password
                flash('Invalid username or password', 'error')
                return redirect(url_for('login.login'))
```



**How to use FlaskForm in Registration**

```python
@bp.route('/register', methods=['GET', 'POST'])
def register():
    form = RegisterForm()
    if request.method == 'GET':
        return render_template("register.html", form=form)
    else:
        if form.validate_on_submit():
            if form.password.data != form.re_password.data:
                flash('비밀번호가 일치하지 않습니다.', 'error')
                return redirect(url_for('login.register'))
            else:
                fcuser = Fcuser()
                fcuser.username = form.username.data
                fcuser.userid = form.userid.data
                fcuser.password = generate_password_hash(form.password.data)
                db.session.add(fcuser)
                db.session.commit()
                return redirect(url_for('login.login'))
```


**예외 처리 후 render template/redirect에 javascript를 통해 alert을 먹일 수 있다**

```javascript
<script>
            {% with messages = get_flashed_messages(with_categories=true) %}
              {% if messages %}
                {% for category, message in messages %}
                  alert("{{ message }}");
                {% endfor %}
              {% endif %}
            {% endwith %}
          </script>
```




##### `session`


세션에 대해 잠시 생각해 보자. session은 request와 마찬가지로 플라스크가 자체적으로 생성하여 제공하는 객체이다. 브라우저가 플라스크 서버에 요청을 보내면 request 객체는 요청할 때마다 새로운 객체가 생성된다. 하지만 session은 request와 달리 한번 생성하면 그 값을 계속 유지하는 특징이 있다.

> 세션은 서버에 브라우저별로 생성되는 메모리 공간이라고 할수 있다.


`ChatGPT 보충설명`

```
당신이 웹 브라우저에서 하는 활동이 세션을 통해 저장되고, 이 세션 정보는 쿠키를 통해 브라우저에 전달되어 다음 방문 때 사용되는 과정을 잘 이해하셨습니다. 좀 더 쉽게 정리해보겠습니다:

1. **처음 방문시**: 웹사이트를 처음 방문할 때, 서버는 세션을 생성하고, 이 세션을 식별하는 고유한 쿠키(예: 세션 ID)를 당신의 브라우저에 보냅니다.
    
2. **브라우저의 역할**: 브라우저는 이 쿠키를 저장합니다. 이 쿠키는 웹사이트를 다시 방문할 때 서버에 자동으로 보내지게 되며, 서버는 이 쿠키를 통해 당신을 식별합니다.
    
3. **개인화된 경험**: 쿠키를 통해 식별된 세션 정보를 바탕으로, 웹사이트는 당신에게 맞춤형 콘텐츠나 이전의 활동(예: 장바구니 내용, 로그인 상태)을 제공합니다.
    
4. **세션의 지속**: 당신이 웹사이트를 사용하는 동안, 세션은 계속 유지됩니다. 이를 통해 웹사이트는 당신의 연속적인 활동을 추적하고, 개인화된 서비스를 제공할 수 있습니다.
    
5. **다시 방문할 때**: 다음에 웹사이트를 방문하면, 당신의 브라우저는 저장된 쿠키를 서버에 보냅니다. 서버는 이 쿠키를 통해 당신이 이전에 방문했던 사용자임을 식별하고, 관련된 세션 정보를 바탕으로 서비스를 제공합니다.
    

이 과정을 통해 웹사이트는 사용자의 경험을 개인화하고, 사용자의 세션 정보를 효율적으로 관리할 수 있습니다.
```


Flask의 경우 'g'를 세션으로 쓴다

` from flask import g`

  
In Flask, `g` is a global object that is provided as a temporary storage during the handling of a request. It stands for "global", but it is not actually global in the same sense as a truly global object would be in Python. Instead, `g` is specific to each request and is reset between requests. It's a part of Flask's application context.



여기서는 `@bp.before_app_request` 애너테이션을 사용했다. 이 애너테이션이 적용된 함수는 라우팅 함수보다 항상 먼저 실행된다. 즉, 앞으로 load_logged_in_user 함수는 모든 라우팅 함수보다 먼저 실행될 것이다.



##### `@login_required`

```python

def login_required(view): 
	@functools.wraps(view) 
	def wrapped_view(*args, **kwargs): 
		if g.user is None: 
			_next = request.url if request.method == 'GET' else '' 
			return redirect(url_for('login.login', next=_next)) 
		return view(*args, **kwargs) 
	return wrapped_view

```

**Explanation**

`wrapped_view`. 
It is used to preserve the metadata of the original view function, like its name, docstring, etc. This is important for Flask's routing system and for debugging purposes.

In summary, the wrapper function (`wrapped_view`) is like a service that adds something extra to your original function every time it is called.

`_next = request.url if request.method == 'GET' else ''` captures the current URL if the request method is GET. This URL is intended to be used as the destination after the user logs in.




##### request.args.get 에 관한 고찰:
1. **`request`**: 클라이언트의 정보들을 담은 Flask의 global object. 서버에 저장돼 있음.
2. **`request.args`**: query parameter(뒷) 부분
3. **`request.args.get`**: key를 넣으면 value를 출력함


그렇다면 query 뒷 부분에는 어떤 key들이 존재할까?


1. **검색 쿼리**: `?search=keyword` - 사용자가 검색어를 입력할 때 사용됩니다.
2. **페이지네이션**: `?page=2` - 여러 페이지를 가진 목록에서 특정 페이지를 표시할 때 사용됩니다.
3. **필터링 및 정렬**: `?sort=price_asc&category=books` - 특정 기준으로 항목을 필터링하거나 정렬할 때 사용됩니다.
4. **트래킹 및 분석**: `?utm_source=newsletter` - 마케팅 캠페인이나 사용자 트래픽 소스를 추적할 때 사용됩니다.

등이 있다고 한다.

그러나 이런 key 값들은 개발자가 스스로 지정해줘야 하고 암묵적으로 용도에 맞게 변수를 설정해준다고 함.



왜 안 되는지 도저히 모르겠다.
