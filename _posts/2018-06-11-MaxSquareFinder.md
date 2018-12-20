---
layout: post
title: Level 2. 가장 큰 정사각형 찾기 (Python)
tags: [Algorithm]
excerpt_separator: <!--more-->
---

**문제:**   
가장 큰 정사각형 (level 2).

문제 설명:   
1와 0로 채워진 표(board)가 있습니다. 표 1칸은 1 x 1 의 정사각형으로 이루어져 있습니다. 표에서 1로 이루어진 가장 큰 정사각형을 찾아 넓이를 return 하는 solution 함수를 완성해 주세요. (단, 정사각형이란 축에 평행한 정사각형을 말합니다.)
<!--more-->
예를 들어

1	2	3	4   
0	1	1	1   
1	1	1	1   
1	1	1	1   
0	0	1	0   

가 있다면 가장 큰 정사각형은

1	2	3	4   
0	~~1	1	1~~   
1	~~1	1	1~~   
1	~~1	1	1~~   
0	0	1	0   

가 되며 넓이는 9가 되므로 9를 반환해 주면 됩니다.

제한사항   
표(board)는 2차원 배열로 주어집니다.   
표(board)의 행(row)의 크기 : 1000 이하의 자연수   
표(board)의 열(column)의 크기 : 1000 이하의 자연수   
표(board)의 값은 1또는 0으로만 이루어져 있습니다.

입출력 예   

board|answer
---|---
[[0,1,1,1],[1,1,1,1],[1,1,1,1],[0,0,1,0]]|9
[[0,0,1,1],[1,1,1,1]]|4

입출력 예 설명   
입출력 예 #1   
위의 예시와 같습니다.   

입출력 예 #2   
| 0 | 0 ~~| 1 | 1 |~~   
| 1 | 1 ~~| 1 | 1 |~~    
로 가장 큰 정사각형의 넓이는 4가 되므로 4를 return합니다.

<br>

[접근]   
1. 컨볼루션 레이어처럼 계산해보면 어떨까?
    - 주어진 배열에서 만들 수 있는 가장 큰 정사각형을 만들어서 그 배열의 합이 넓이와 같은 경우를 찾자!
    - 정사각형을 이동시키면서 그리고 가로 세로 길이를 1씩 줄이면서 구해보자!
    - 근데 이게 대충 생각만 해봐도 매우 비효율적인게 느껴짐... (배열합 구하는 거를 몇번을 해야하는겨?)
2. 깊이 우선, 너비 우선 탐색을 적용해볼까?
    - 해보다가 깊이 우선, 너비 우선 복습만 함...
3. 결국 다른 분이 풀어서 블로그에 올리신 [포스팅](http://whatisthenext.tistory.com/138) 참고함.
    - Dynamic Programming을 통해 푸신 글을 봄.
    - 신세계 열림... ㅎㄷㄷ...
    - DP를 자료구조 공부하면서 배웠지만 자세히 공부 안함... 반성함...
   
   
결론 -  Dynamic Programming이 재귀보다 좋고, 매우 뛰어난 방법임을 깨닳음.


```python
def solution(board):
    # check whether board is consist of only zeros
    _s = 0
    for i in range(len(board)):
        _s += sum(board[i][:])
    if _s == 0:
        return 0
    
    _s = 0  # initialize _s to 0
    
    table = [[x for x in sub] for sub in board]
    for x in range(1,len(table)):
        for y in range(1, len(table[x])):
            if table[x][y] == 0:
                continue
            else:
                _min = min([table[x][y-1], table[x-1][y], table[x-1][y-1]])
                table[x][y] = _min + 1
                if _s < table[x][y]:
                    _s = table[x][y]
                    
    # if board looks like identity matrix _s would be 0, but max square would be 1.
    if _s == 0:
        return 1
    else:
        return _s ** 2
```

문제 출처
- https://programmers.co.kr/learn/courses/30/lessons/12905
