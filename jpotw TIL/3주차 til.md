# 2023-12-20

## 한 것
- 카카오톡이 AI 요약 기능을 발표함 → 망햇다
- gpt4v를 이용한 크롤링 ver.1 만듦
- 무엇을 해야될지 고민중

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


