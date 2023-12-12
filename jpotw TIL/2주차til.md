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