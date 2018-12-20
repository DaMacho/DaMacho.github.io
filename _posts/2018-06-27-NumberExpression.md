---
layout: post
title: Level 2. 숫자의 표현 (Python)
tags: [Algorithm]
excerpt_separator: <!--more-->
---

**문제:**    
숫자의 표현 (level 2).

문제 설명:   
Finn은 요즘 수학공부에 빠져 있습니다. 수학 공부를 하던 Finn은 자연수 n을 연속한 자연수들로 표현 하는 방법이 여러개라는 사실을 알게 되었습니다. 예를들어 15는 다음과 같이 4가지로 표현 할 수 있습니다.   
<!--more-->

- 1 + 2 + 3 + 4 + 5 = 15   
- 4 + 5 + 6 = 15   
- 7 + 8 = 15   
- 15 = 15   
자연수 n이 매개변수로 주어질 때, 연속된 자연수들로 n을 표현하는 방법의 수를 return하는 solution를 완성해주세요.   

제한사항   
- n은 10,000 이하의 자연수 입니다.

<br>

[접근]   
반복문 중첩을 사용하자.   
첫번째 바깥의 반복문은 덧셈을 시작하는 첫 숫자를 설정하는 반복문.   
두번째 중첩된 반복문은 시작한 숫자부터 연속된 수를 더하는 반복문.   

- 함수는 인자로 자연수 n을 받는다.
- 표현방법의 수를 변수 answer로 설정하고 0으로 초기화.
- for문을 사용하여 덧셈을 시작하는 첫 숫자를 루프마다 업데이트한다.
    - 연속된 수의 합을 저장하는 변수 s를 설정하고 0으로 초기화.
    - while문, 조건은 수의 합 s가 n보다 작다로 한다.
        - 변수 s에 첫 숫자가 더해진다.
        - 더해지는 숫자는 1씩 커지고 결국 연속된 수의 합이 s에 저장된다.
        - if문, 덧셈결과 s가 n과 같으면, 표현하는 방법이 존재한다는 것이므로 answer에 1을 더해준다.


```python
def solution(n):
    answer = 0
    for i in range(1, n+1):
        s = 0
        while s < n:
            s += i
            i += 1
            if s == n:
                answer += 1
    return answer
```


```python
# 표현되는 방법들도 알고 싶다면...
def solution_2(n):
    answer = 0
    a2 = []
    for i in range(1, n+1):
        s = 0
        while s < n:
            s += i
            i += 1
            if s == n:
                answer += 1
                a2.append(i-1)
    return answer, a2
```


```python
solution_2(30)
```




    (4, [8, 9, 11, 30])



문제 출처
- https://programmers.co.kr/learn/courses/30/lessons/12924
