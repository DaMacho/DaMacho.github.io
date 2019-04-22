---
layout: post
title: Pythonic & Concepts (1)
tags: [Python, Computer Science]
excerpt_separator: <!--more-->
---

# Study Summary

# 01장 철학과 개념

## Chapter 01 - Pythonic & Concept (1)     
<!--more-->
**파이썬답게 코딩하기 (프로그래밍 언어의 개념과 흐름에 대한 고찰), 심경섭 저**
> 스터디 발표를 위한 정리 자료.   
> 봐도 모르겠으면, 나중에 꼭 책으로 다시 읽을 것!

## Pythonic

**Pythonic이란?** - 파이썬을 파이썬답게 사용하기 위한 세부지침(?) (ex. PEP 8)   

Pythonic의 지향점은,    
프로그래밍 언어를, 글을 읽거나 쓰는 것처럼 자연스럽게 만드는 것,   
영어로 된 글을 쓴다는 느낌으로 코드를 작성하는 것이 아닐까?   
라고 저자 말하심.

## 변수 (variable)

변수: 메모리에 어떤 값을 가리키고 있는 이름. => 메모리에 있는 어떤 값에 이름을 붙여 내가 쉽게 사용하게 하는 것.

변수의 lifetime: 선언 - 할당 - 소멸.

파이썬에서 변수의 선언과 할당은 **=** 연산자를 통해 단번에 일어난다.

### Scope

**Scope란** - 변수의 유효 범위, (변수가 거주가능한 구역?).   
파이썬에서는 그 범위가 **네임스페이스(namespace)**를 기반으로 계산된다.

파이썬의 네임스페이스는 **local, enclosed, global, built-in** 으로 나눌 수 있다.   
- **local** 변수는 지역적인 네임스페이스로 클래스나 함수의 내부로 한정된다.
- **enclosed** 변수는 함수 안에 함수가 있는 구조, 즉 외부 함수가 있고 내부 함수가 있는 구조에서    
내부 함수가 외부 함수의 변수에 접근할 수 있는 것을 의미한다. ('Nested Function' 참고)
- **global** 변수는 전역적으로 사용이 가능한 네임스페이스로 파일 단위의 모듈 안에서 유효하다.    
특정 모듈을 import하는 경우 import한 모듈의 global 변수도 사용이 가능하다.
- **built-in** 변수는 파이썬에 내장되어 있는 네임스페이스로 파이썬으로 작성된 코드 어디에서나 사용할 수 있다.

변수를 확인할 때도 위 순서대로 LEGB 확인하는 것이 바람직하다.   
예시 1-1)


```python
msg = "Hello"

def read_work():
    print(msg)
    print("What")
    
def read_exception():
    print(msg)
    msg = "What"
    print(msg)
    
def main():
    print("=== firtst read ===")
    read_work()
    
    print("=== second read ===")
    read_exception()
    
main()
```

    === firtst read ===
    Hello
    What
    === second read ===
    


    ---------------------------------------------------------------------------

    UnboundLocalError                         Traceback (most recent call last)

    <ipython-input-1-9e0f1d219803> in <module>()
         17     read_exception()
         18 
    ---> 19 main()
    

    <ipython-input-1-9e0f1d219803> in main()
         15 
         16     print("=== second read ===")
    ---> 17     read_exception()
         18 
         19 main()
    

    <ipython-input-1-9e0f1d219803> in read_exception()
          6 
          7 def read_exception():
    ----> 8     print(msg)
          9     msg = "What"
         10     print(msg)
    

    UnboundLocalError: local variable 'msg' referenced before assignment


#### 왜 Error?   
- 9번째 줄에서 사용된 msg 변수 때문.   
9번째 줄에서 전역변수 msg의 값을 변경한 것이 아닌, 지역변수 msg를 선언.   
8번째 줄에서 지역변수 msg의 print()수행이 변수의 선언보다 먼저 일어났다고 판단하여 오류 발생.   
파이썬이 변수 msg를 전역변수가 아닌 지역변수로 판단한 것.

### Global

**global** 키워드는 지역(?)에서 전역 변수를 사용할 수 있게 해준다. (지역구에서 전국구 형님을 부를 수 있다!)


```python
msg = "Hello"

def read_work():
    print(msg)
    print("What")
    
def read_exception():
    global msg
    print(msg)
    msg = "What"
    print(msg)
    
def main():
    print("=== firtst read ===")
    read_work()
    
    print("=== second read ===")
    read_exception()
    
main()
```

    === firtst read ===
    Hello
    What
    === second read ===
    Hello
    What
    

### Nonlocal

**nonlocal** 키워드는 'Nested function'(중첩함수, 함수가 중첩되어 있는 것)에서 상위 함수의 변수를 사용할 수 있게 해준다.


```python
def greeting(name):
    greeting_msg = "Hello "
    
    def add_name():
        return f'{greeting_msg}{name}'
    
    msg = add_name()
    print(msg)
    

greeting("Python")
```

    Hello Python
    


```python
def greeting(name):
    greeting_msg = "Hello"
    
    def add_name():
        greeting_msg += " "
        return f'{greeting_msg}{name}'
    
    msg = add_name()
    print(msg)
    

greeting("Python")
```


    ---------------------------------------------------------------------------

    UnboundLocalError                         Traceback (most recent call last)

    <ipython-input-9-04619847a977> in <module>()
         10 
         11 
    ---> 12 greeting("Python")
    

    <ipython-input-9-04619847a977> in greeting(name)
          6         return f'{greeting_msg}{name}'
          7 
    ----> 8     msg = add_name()
          9     print(msg)
         10 
    

    <ipython-input-9-04619847a977> in add_name()
          3 
          4     def add_name():
    ----> 5         greeting_msg += " "
          6         return f'{greeting_msg}{name}'
          7 
    

    UnboundLocalError: local variable 'greeting_msg' referenced before assignment



```python
def greeting(name):
    greeting_msg = "Hello"
    
    def add_name():
        nonlocal greeting_msg
        greeting_msg += " "
        return f'{greeting_msg}{name}'
    
    msg = add_name()
    print(msg)
    

greeting("Python")
```

    Hello Python
    

### Variable Shadowing

\- 전역 변수나 상위 변수를 지역 변수나 멤버 변수로 가려(숨겨)버리는 것.


```python
var_shadowing = "global"

def outer_function():
    var_shadowing = "outer"
    
    def inner_function():
        var_shadowing = "inner"
        print(f'Scope of inner_function: {var_shadowing}')
        
    inner_function()
    print(f'Scope of outer_function: {var_shadowing}')
    
    
if __name__ == "__main__":
    outer_function()
    print(f'Global scope: {var_shadowing}')
```

    Scope of inner_function: inner
    Scope of outer_function: outer
    Global scope: global
    

### Free variable

**Free variable**이란 코드에서 사용됐지만 정의되지 않은 변수를 나타내는 용어이다.   
=> 함수 내부에서 사용됐지만 함수 내에는 정의되어 있지 않은 변수.   
   
파이썬에서는 **nonlocal** 키워드로 사용한 변수를 free variable 이라고 지칭할 수 있다.   
지역변수도 아니고 전역변수도 아니지만, 사용할 수 있는 변수를 말한다.

<br>

## First-Class

**First-Class**는 파이썬에서 사용하는 여러 기술을 이해하기 위한 가장 기본적인 개념.   
파이썬은 **first-class 속성**을 가진 **first-class 함수**를 지원하기 때문에 함수를 인자로 넘기거나 클로저(closure)를 사용할 수 있다.   
\- 클로저는 nested function의 일종으로 후에 다시 다룰 예정.

### First-Class Citizen

**First-Class Citizen**, 이 **<u>속성</u>**을 가진 어떤 개체는,   
다른 개체의 매개변수로 전달되거나, 함수의 반환값으로 사용되거나, 변수에 값으로 할당될 수 있다.   
여기서 '개체'란 자료형 또는 함수 또는 클래스가 될 수 있다.   

예시)   
자료형 중, 스칼라(scalar) 형식의 자료형(integer, floating-point)들은 거의 다 first-class citizen.    
변수에 할당은 물론, 매개변수 또는 반환값으로 사용가능.   
   
array나 string은 **프로그래밍 언어에 따라 다르다.**   
파이썬에서는 first-class citizen이지만,    
C언어에서 array가 전달될 때 그 자체가 아닌 array의 첫번째 원소의 주소값이 전달된다.   
다시 말하면, array 그 자체로 매개변수나 함수의 반환값으로 사용될 수 없다.

**요약) 개체, 즉 자료형이나 함수 등이 그 자체로 매개변수로 전달되거나 반환값으로 사용될 수 있으면 그 개체는 first-class citizen 속성을 가지고 있다.**

### First-Class Function

first-class citizen 속성을 가진 함수.   
즉, 변수에 함수를 할당할 수 있고, 매개변수로 함수를 전달할 수 있고, 반환값으로 함수를 사용할 수 있다.   
   
이 속성을 통해 여러 가지 기술이 탄생했다.   
클로저(Closure)나 데코레이터(decorator)가 대표적이다. (후술 예정)


```python
# first_class_function_variable
'''
함수를 변수에 저장하고, 
그 변수를 함수처럼 사용하여 호출하는 예제.
'''

def square(x):
    return x*x

def main():
    print("Function call")
    print(square(10))
    print()
    
    print("Assign variable")
    f = square
    print(f(10))
    
    
if __name__ == "__main__":
    main()
```

    Function call
    100
    
    Assign variable
    100
    


```python
# first_class_function_parameter
'''
함수를 하나의 개체로 보고,
매개변수로 전달하고, 
반환값으로 사용함.
'''

def square(x):
    return x*x

def bind(func, arg):
    return func(arg)

def main():
    print("Set argument")
    arg = 5
    print(f'{arg} \n')
    
    print("Assign variable & send parameter")
    squares = bind(square, arg)
    print(squares)
    
    
if __name__ == "__main__":
    main()
```

    Set argument
    5 
    
    Assign variable & send parameter
    25
    

## Higher-Order Function

**Higher-Order function**은 first-class function과 유사한 개념이다.   
함수가 매개변수로 전달되거나, 또는 함수를 반환값으로 사용하는 함수를 higher-order function이라고 한다.    
둘 중 하나만 성립해도 higher-order function이다.


```python
# upper_case
'''
이 예제에서는 함수가 매개변수나 반환값으로 사용되지 않았다.
이 예제에 higher-order function을 적용해본다면?
'''

LOWER_LIST = ["min", "yong", "hun"]
UPPER_LIST = []

def convert():
    for data in LOWER_LIST:
        UPPER_LIST.append(data.upper())
        
def main():
    print("===print result===")
    convert()
    print(LOWER_LIST)
    print(UPPER_LIST)
    

if __name__ == "__main__":
    main()
```

    ===print result===
    ['min', 'yong', 'hun']
    ['MIN', 'YONG', 'HUN']
    


```python
# brief_upper_case
''' '''

LOWER_LIST = ["min", "yong", "hun"]
UPPER_LIST = []

def convert(data):
    return data.upper()
        
def main():
    print("===print result===")
    UPPER_LIST = map(convert, LOWER_LIST)
    print(LOWER_LIST)
    print(list(UPPER_LIST))
    print()
    print("===variable UPPER_LIST===")
    print(UPPER_LIST)
    

if __name__ == "__main__":
    main()
```

    ===print result===
    ['min', 'yong', 'hun']
    ['MIN', 'YONG', 'HUN']
    
    ===variable UPPER_LIST===
    <map object at 0x000002AA778ACE10>
    


```python
arg = "b"
b = arg.upper
b_ = arg.upper()

print(arg)
print(b)
print(b_)
```

    b
    <built-in method upper of str object at 0x000002AA7354F960>
    B
    


```python
advanced = list(map(lambda data: data.upper(), LOWER_LIST))

print("===advanced form===")
print(advanced)
```

    ===advanced form===
    ['MIN', 'YONG', 'HUN']
    

## Nested function

**중첩함수(Nested function)**는 함수로 감싸인 함수를 뜻한다. 쉽게 말하면 함수 안에 정의된 함수이다. 외부함수를 'Outer function', 내부함수를 'Inner function'이라고 표현한다.

중첩함수는 단순히 하나의 함수안에 정의된 또 다른 하나의 함수뿐 아니라, 여러 개의 함수를 중첩한 형태로도 표현될 수 있다. 이때 각각의 내부함수들은 **scope chain**에 의해 자신을 감싸고 있는 외부함수의 메모리에 접근이 가능하다.

외부함수의 메모리에 접근이 가능하다는 것은 외부함수가 가지고 있는 변수나 매개변수 등에 접근할 수 있고 사용할 수 있다는 뜻이다. 정확히는, 외부함수의 메모리를 복사해서 가지고 있다.    
앞서 설명한 **네임스페이스의 LEGB 규칙** 중에서 **enclosed**에 해당하는 내용이다.

내부함수를 사용하면 외부함수의 자원을 이용할 수 있는 장점이 있다. 그래서 같은 매개변수를 사용하거나, 연관된 기능을 하는 함수들을 하나의 외부함수에 묶어서 사용하기도 한다. 이때 유의점은, 외부에서 내부함수로 접근할 수 없다는 점이다.

즉, 외부에서 내부함수를 직접 호출할 수 없다는 것이다. 내부함수가 외부함수 안에 감싸여 정의되어 있기 때문에 파이썬은 외부함수 단에서 내부함수의 존재를 알지 못한다.   
그래서 내부함수가 실행되기 위해, 먼저 외부함수를 호출하고 함수 내 로직에 따라 내부함수가 호출되어야한다.


```python
# calculator_1
'''중첩함수 예제, 
내부함수를 사용하여, 외부함수의 매개변수 사용'''

def calculator(x, y):   # 외부함수
    
    def add():   # 내부함수
        return x+y
    
    def sub():   # 내부함수
        return x-y
    
    return (add(), sub())


if __name__ == "__main__":
    print("=== print calculation ===")
    print(calculator(10, 5))
```

    === print calculation ===
    (15, 5)
    


```python
# calculator_2
'''
중첩함수 예제,
외부함수와 내부함수가 각각 매개변수를 받고,
외부함수는 내부함수를 반환값으로 사용함.
'''

def calculator(x):
    
    def add(y):
        return x+y
    
    return add


if __name__ == "__main__":
    print("=== print calculation ===")
    f = calculator(10)   # 변수 f에 매개변수와 함께 외부함수를 할당.
    print(f(5))   #변수 f에 매개변수를 주고 실행.
    print(f(10))
```

    === print calculation ===
    15
    20
    

**변수 f**에 **외부함수(calculator())를 할당**할때 **외부함수의 매개변수** 10을 x로 할당함.   
**변수 f**에는 **외부함수의 반환값**인 **함수 add()**가 할당됐다.   
**변수 f를 실행**할때, **매개변수로 전달한 값**은 함수 add(), 즉 **내부함수의 매개변수** y로 전달 됨.

중첩함수 중, 위 두번째 예제와 같은 것을 **클로저(Closure)**라 부른다.
