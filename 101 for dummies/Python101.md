If the number of variables is less than the number of values, you can add an `*` to the variable name and the values will be assigned to the variable as a list:

```
fruits = ("apple", "banana", "cherry", "strawberry", "raspberry")  
  
(green, yellow, *red) = fruits  
  
print(green)  
print(yellow)  
print(red)
```

result:

```
apple
banana
['cherry', 'strawberry', 'raspberry']
```


Normally, when you create a variable inside a function, that variable is local, and can only be used inside that function.

To create a global variable inside a function, you can use the `global` keyword.



#  Str

**Slicing**

When you need to cut certain parts in a str variable, use `:` .


How to check str : 
```python
txt = "The best things in life are free!"

print("free" in txt)
```

if True, it prints out `True`

not in 도 가



reformatting 할 때 꿀팁:

str.split() 을 통해 Str을 리스트로 만든다.
그 다음에 for loop을 통해 print하게 한다.

Ex.

```python
a = "Hello, World!"

print(a.split(",")) # returns ['Hello', ' World!']
bb = a.split(", ") # 나누고 싶은 부분

for b in bb:
  print(b)
```


**format()**

```
name = "John"
age = 36  
txt = "My name is {}, and I am {}."  
print(txt.format(name, age))
```

result:
`My name is John, and I am 36.`

format type **does not** actually change the data type of the variable. It makes a clone string that looks exactly like the variable. You can use it in all types of data.

ChatGPT additional comment: 
creating a new string that includes a formatted version of the variable's value. **This new string is a separate entity from the original variable**, and it can be used for display, further processing, or any other purpose you may have.


cf) You can call format using '{}'.
Ex.
```python
quantity = 3  
itemno = 567  
price = 49.95  

myorder = "I want to pay {2} dollars for {0} pieces of item {1}."  
print(myorder.format(quantity, itemno, price))
```


All the other string methods:
[Python - String Methods (w3schools.com)](https://www.w3schools.com/python/python_strings_methods.asp)



There are four collection data types in the Python programming language:

- **List** is a collection which is ordered and changeable. Allows duplicate members.
- **[Tuple](https://www.w3schools.com/python/python_tuples.asp)** is a collection which is ordered and unchangeable. Allows duplicate members.
- **[Set](https://www.w3schools.com/python/python_sets.asp)** is a collection which is unordered, unchangeable*, and unindexed. No duplicate members.
- **[Dictionary](https://www.w3schools.com/python/python_dictionaries.asp)** is a collection which is ordered** and changeable. No duplicate members.





# 헷갈리는 개념 정리:

class 에서 self를 이용하여 정해줄 때 

```python
class Person:  
  def __init__(self, a, b):  
    self.name = a  
    self.age = b
  
p1 = Person("John", 36)  
  
print(p1.name)  
print(p1.age)
```


함수 인풋에 들어가는 건 나중에 그 클래스를 불러올 때 넣을 input 값이고

아래 . 어쩌고 는 클래스를 특정 변수에 넣고 그 변수를 불러올 때 사용하는 것이다.


그러니까

class person 을 실제 사용할 때는 아래처럼 사용하는 것이다:

```python
person1=Person(a="John", b=36)
p1.name
p1.age
```

사실 주로 둘다 같은 이름으로 불리는 게 일반적이지만 이렇게 알아둘 것.


**Parent Class and Child Class**

특정 class의 특성을 다 받는 새로운 class를 만들 수 있음 

Ex.

```python
class Person:
	def __init__(self, fname, lname):
		self.fname = fname
		self.lname = lname

	def nameprint(self):
		print(self.fname + " " + self.lname)
```

이렇게 class Person을 정해준 후


```python
class OtherPerson(Person):
	pass
```

이렇게 괄호 안에 넣으면 Person의 모든 특성들이 다 OtherPerson으로 복사된다.
물론 여기서 `__init__`을 조절하거나 추가 method를 만들어주는 것을 할 수 있다. 
한 번 확인해보자:

```python
person1 = OtherPerson("Tommy", "Lee")

person1.nameprint()
```

output: Tommy Lee

굳


cf) The child's `__init__()` function **overrides** the inheritance of the parent's `__init__()` function.
To keep the inheritance of the parent's `__init__()` function, add a call to the parent's `__init__()` function:
```python
class Student(Person):  
  def __init__(self, fname, lname):  
    Person.__init__(self, fname, lname)
```

