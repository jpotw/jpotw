
# Hello Coding - POCU

## 0. Basic of Basic

- 실행할 때`C# Program.cs`에서 Program 들어가고 실행(Ctrl + F5) 하면 됨.
- 파이썬처럼 그냥 변수 지정하면 안 되고 꼭꼭 type을 앞에 써야 한다
	- Ex.
```c#
string a = Console.Read()
int b = int.Parse(Console.Read())
```



```c#
using System;

namespace //"프로젝트명"//
{
	class Program
	{
	static void Main(string[] args)
	}
	{
	//여기부터 쓰고 싶은거 쓰면 됨//
	}
}
```


## if

`C#`에서는 if문을 쓸 때 `:`을 쓰지 않는다:

형식:
```c#
if ()
{

}
```


## While

While 쓰는 법 = if 랑 똑같음

```c#
while ()
{
}
```

`break` : while 문에서 나가는 역할
`continue`: while문을 다시 시작하는 역할


## for

`for`: 반복횟수가 **정해진** 코드
`while`: 반복횟수가 **정해지지 않은** 코드

```c#
for (//1:초기화 코드; 2:반복 조건식; 4:증감문//)
{
	//3:반복할 코드//
}
```

- **초기화 코드**: 반복문이 실행되기 전 딱 한 번만 실행됨
- **반복 조건식**: 반복문을 실행할지 검사 (if True: 돌리고 else: 반복문 종료)
- **증감문**: 한 번의 반복문 회차가 끝난 뒤 실


`C#`에서 두 가지 변수를 동시에 설정 및 초기화하는 것은 **불가능**하다.
대신 이런 방법은 가능하다:

```c#
int passcodeLength = 6;
int passcodeIndex;
int realPasscodeIndex; //변수들을 미리 설정

for (passcodeIndex = 0, realPasscodeIndex = passcodeIndex + 1; passcodeIndex<passcodeLength; passcodeIndex++, realPasscodeIndex++)
```

여기서 realPasscodeIndex++을 안 해주면 처음 초기화때만 실행되고 숫자가 늘지 않는다. 처음에는 초기화만 해준다는 사실을 기억
→ 근데 이런 방식은 추천되지 않는다고 함

**상세 설명**

**1번**
```c#
for (int i = 0; i < 3; i++)
{
    int realNum = i + 1;
    Console.Write(realNum);
    Console.WriteLine(" 번째 숫자를 맞춰주세요:");
    guesses[i] = int.Parse(Console.ReadLine());
}
```

과

**2번**
```c#
int i 
int realNum 
for (i = 0, realNum = i+1; i<3; i++, realNum++) 
{ 
	int realNum = i + 1; 
	Console.Write(realNum); 
	Console.WriteLine(" 번째 숫자를 맞춰주세요:"); 
	guesses[i] = int.Parse(Console.ReadLine()); }
```

이 있을 때 1번이 좀 더 일반적인 방식이라고 함.

왜?
더 심플함 = 한눈에 들어와서 더 이해하기 쉬움



for문에서 i는 걍 i번 반복해라 이뜻임

Ex.
```csharp
for (int i = 0; i<3; i++)
```
이건 그냥 `C#` 언어로 3번 반복해라  이뜻


## Random

```csharp
Random random = new Random();
```


헷갈렸던 `hasDuplicate` 복습

```csharp
bool hasDuplicate = false;
for (int i= 0; i < index; i++)
{
    if (numbers[index] == numbers[i])
    {
      hasDuplicate = true;
      break;
    }
}
```

여기서 내가 궁금했던 거는:

indx == i 여서 `numbers[index] == numbers[i]` 되면 어떡하지? 였는데
그걸 위해서 for loop으로 감싸준 거임 (`i<index` 때문에 index == i가 될 수 없음)


**헷갈렸던 변수 복습**

```c#
while (true)
{
bool isPasswordCorrect = true;
for (passcodeIndex = 0; passcodeIndex < passcodeLength; passcodeIndex++)
{ if (userInput[passcodeIndex] != passcodeNumbers[passcodeIndex])
    {
        **isPasswordCorrect = false;**
        Console.WriteLine("틀렸음");
        break;
    }
}
if (isPasswordCorrect)
{ 
    Console.WriteLine("문이 열렸습니다");
    break;
}
}
```

여기서 왜 `isPasswordCorrect = false`를 해야 되는지 몰랐음.
그래서 생략하고 돌려보니 뭔 값을 입력해도 if isPasswordCorrect가 실행돼서 "문이 열렸습니다" 후 종료되더라.



반복문을 여럿 설정해주면 각각의 이름을 설정해주면 헷갈림을 덜 수 있을듯




## array


int new 아니고 new int임 그럼 그만큼의 수의 array가 생김
