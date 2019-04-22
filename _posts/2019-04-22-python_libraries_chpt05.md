---
layout: post
title: Python Library Recipe, Ch 05
tags: [Python]
excerpt_separator: <!--more-->
---

# Study Summary

# Chapter 05 - 범용 OS/런타임 서비스     
<!--more-->

**파이썬라이브러리레시피, 이케우치 다카히노 등 5명.**    
> 스터디 발표를 위한 정리자료.    
>    
> 5장에서는 운영체제가 제공하는 기능을 이용하는 것을 비롯하여 서버의 운용과 관리에 도움이 되는 표준 라이브러리를 소개함. 작업 자동화나 효율화에 도움이 될듯.

## 5.1 운영체제의 기능 이용하기

[OS모듈 공식문서](https://docs.python.org/3.4/library/os.html)    
   
**OS 모듈의 주요 기능**   
- **실행 중인 프로세스 속성 조작**
- 파일 기술자(descriptor) 조작
- **파일과 디렉터리 조작**
- 프로세스 관리
- **다양한 시스템 정보에 대한 접근**
- 스케줄러 인터페이스
- **무작위 문자열 생성**

**(1) 실행 중인 프로세스 속성 조작**   
   
속성의 종류 | 함수 |
:--- | :--- 
환경변수 | environ, getenv(), putenv(),...
사용자 ID | getuid(), setuid(), geteuid(), seteuid(), ...
그룹 ID | getgid(), setgid(), getgroups(), setgroups(), ...
프로세스 ID | getpid(), getpgid(), geppid(), ...
스케줄링 우선도 | getpriority(), setpriority(), ...
   
os 모듈에서 프로세스 속성에 대한 기능 대부분은 UNIX 계열 운영체제 기능에 의존하므로, Windows 환경에서는 이용할 수 없는 것이 많다. (공식 문서 참고)    
   
os.environ 은 파이썬 프로스를 구동할 때 환경변수를 저장하는 맵 형식의 객체이다. UNIX와 Windows에서 모두 사용가능하다.


```python
# 환경변수에 접근하기
import os

print(os.environ['SYSTEMDRIVE'])
print(os.environ['SYSTEMROOT'])
# os.environ['HAM'] = 'egg' 
```

    C:
    C:\WINDOWS
    

os.environ에 저장된 것은 os모듈이 import된 시점의 환경변수이다. 보통은 Python 구동 시에 site.py가 처리되는 도중의 시점의 환경변수라고 할 수 있다. 그 후에 변경된 환경변수는 반영되지 않으므로, 직접 접근하여 변경해야한다. 또한 설정한 환경변수는 실행 중인 프로세스 상의 ox.environ에만 반영되어 있으므로, 다른 프로세스 간에 값을 공유하는 용도로 사용할 수 없다.   
   
동일하게, os모듈로 조작할 수 있는 다른 프로세스 속성도 실행 중인 프로세스에만 반영된다.

**(2) 파일과 디렉터리 조작**   
파일 조작 관련 함수. (도서 또는 공식 문서 참고)   
예시)   
- chdir()
- chmod()
- chwon()
- getcwd()
- listdir()
- mkdir()
- makedirs()
- remove()
- removedirs()
- rmdir()




```python
# 기본적인 파일 조작
import os

print(os.getcwd())
print(os.listdir('.'))
print(len(os.listdir('.')))
```

    D:\DSS\Projects\blog_posts\jupyters
    ['.ipynb_checkpoints', 'pythonic_coding_chpt01_01.ipynb', 'pythonic_coding_chpt01_02.ipynb', 'python_libraries_chpt05.ipynb']
    4
    

**(3) 다양한 시스템 정보에 대해 접근**   
시스템 정보 관련 함수와 상수. (도서 및 문서 참고)   
예시)   
- cpu-count()
- getloadavg()   

파일 경로 관련 상수. (도시 및 문서 참고)   
예시)   
- curdir
- pardir
- sep
- extsep

**(4) 무작위 문자열 생성**   
os.urandom()은 운영체제가 제공하는 난수 생성기를 사용하여 무작위 문자열을 반환한다.


```python
# 무작위 문자열 생성
import os

# 10바이트의 무작위 문자열 생성
os.urandom(10)
```




    b'\xdb\xed\x86\x0e\x10\xb7\x88\x8a\xa4\x82'



random모듈이 생성하는 난수는 의사 난수이므로 보안상 용도로는 부적합하다.   
보안상 필요라면, os.urandom()을 사용하거나 내부에서 osurandom()을 사용하여 난수를 생성하는 random.SystemRandom 클래스를 이용할 것을 추천한다.

## 5.2 스트림 다루기

[io모듈 공식문서](https://docs.python.org/3.4/library/io.html)    
   
**io 모듈이 제공하는 클래스**   
- 문자열을 파일과 같은 인터페이스로 다루는 StringIO 클래스
- 바이트열을 파일과 같은 인터페이스로 다루는 BytesIO 클래스
- 기타, 스트림 객체의 추상 기반 클래스군   

> 알게 모르게 이 모듈 덕을 본 경우가 많다고 한다...   
> 나는 정말 모르겟...

**(1) 인메모리 텍스트 스트림 다루기 - StingIO**   
   



```python
# StringIO 기본적인 사용
import io

stream = io.StringIO("this is test\n")   # 초기값 설정 가능
print(stream.read(10))   # 스트림으로부터 지정한 크기만큼 읽어온다.

print(stream.tell())   # 현재 오프셋을 반환

print(stream.seek(0, io.SEEK_END))   # 오프셋을 스트림 맨 끝으로 변경함.

print(stream.write('test'))   # 스트림에 문자열을 쓴다.

print(stream.getvalue())   # 스트림이 가진 모든 내용을 반환

stream.close()   # 스트림의 닫음.
stream.write('test')   # 스트림을 닫은 뒤에 쓰려하면 예외 발생.
```

    this is te
    10
    13
    4
    this is test
    test
    


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-4-45512986cdf7> in <module>()
         14 
         15 stream.close()   # 스트림의 닫음.
    ---> 16 stream.write('test')   # 스트림을 닫은 뒤에 쓰려하면 예외 발생.
    

    ValueError: I/O operation on closed file


**(2) 단위 테스트 활용 예**
- 파일 객체 대신 사용
- 표준 출력 등을 캡처할 때


```python
# StringIO를 이용한 표준 출력 캡처
import io
from unittest.mock import patch

def print_hoge():
    print('hoge')   # print() 는 sys.stdout.write() 와 같음.
    
@patch('sys.stdout', new_callable=io.StringIO)   # 표준 출력을 StringIO로 대체
def test_print_hoge(mocked_object):   # mocked_object가 대체한 후의 스트림
    print_hoge()
    assert mocked_object.getvalue() == 'hoge\n'   # 스트림에 쓰인 내용을 검증
    
test_print_hoge()
```

## 5.3 인터프리터 관련 동작 및 조작

[sys모듈 공식문서](https://docs.python.org/3.4/library/sys.html)    
   
- 파이썬 인터프리터에서 사용하는 변수 및 파이썬 인터프리터의 동작에 관한 함수 제공

**(1) 명령줄의 인수 얻기 - sys.argv**   
   
sys.argv는 Python 스크립트를 실행할 때 주어지는 인수가 저장되는 리스트.   
sys.argv[0] 은 실행된 스크립트 자신의 파일명이다.


```python
# 예시

import sys
print(sys.argv)
```

    ['C:\\Anaconda3\\lib\\site-packages\\ipykernel_launcher.py', '-f', 'C:\\Users\\Sanghun\\AppData\\Roaming\\jupyter\\runtime\\kernel-983620e9-b406-4689-b4d5-a6f581180cb8.json']
    

교재 예시)   
> \# hoge.py 파일   
> import sys   
> print(sys.argv)   
   
hoge.py 실행 결과   
> $ python hoge.py -a abc   
> ['hoge.py', '-a', 'abc']   
   
- sys.argv를 사용해서 commend line 인수를 처리할 수도 있다.
- 복잡한 commend line 인수 처리가 필요하다면, "5.4 명령줄 옵선과 인수 다르기 -argparse"의 argeparse 모듈을 적용할 수도 있다.

**(2) 라이브러리의 import path 조작하기 - sys.path**   
   
sys.path는 import 대상 모듈이나 패키지의 위치가 되는 여러 개의 파일 경로를 저장한 리스트이다. sys.path에 파일 경로를 추가하면 해당 파일 경로에 있는 Python 패키지나 모듈을 import문으로 import 할 수 있다.   

sys.path는 다음 요소를 통해 초기화된다.
- 실행된 Python 스크립트가 있는 경로 또는 대화모드인 경우에는 빈 문자열(스크립트 실행시 해당 디렉터리에서 탐색)
- 환경변수 PYTHONPATH로 설정된 경로
- Python 설치 위치

Python 스크립트가 위치한 디렉터리를 환경변수 PYTHONPATH로 지정하여 대화형 콘솔에서 실행한 예시.   

> $ ls /home/my/scripts   
> myscript.py   
   
> $ PYTHONPATH=/home/my/scripts python

PYTHONPATH가 지정된 상태에서 sys.path 값 확인.   
> \>>> import sys   
> \>>> import pprint   
> \>>> pprint.pprint(sys.path)   
> ['',   
> '/home/my/scripts',   
> '/usr/lib/python3.4',   
> ...,   
> '...']   

> \>>> import myscript

모듈은 리스트의 맨 앞 경로부터 순서대로 검색되어, 먼저 발견된 것을 import한다.   
그러므로 표준 라이브러리와 같은 이름의 모듈을 생성하는 것은 피하거나 주의할 필요가 있다.

**실행할 때 sys.path에 경로 추가**   
> \>>> import sys   
> \>>> sys.path.append('/home/my/scripts')   
> \>>> import myscript   

**(3) 프로그램 종료하기 - sys.exit**   

sys.exit()는 호출 시, Python 스크립트 실행을 종료시키고, 대화모드에서는 대화모드를 종료시킨다.   

(교재 및 문서 참고.)

**(4) 콘솔 입출력 - sys.stdin, stdout, stderr**   

인터프리터가 사용하는 콘솔의 입출력용 객체.   
- sys.stdin : 표준 입력 객체. 읽기 전용.
- sys.stdout : 표준 출력 객체. 쓰기 전용.
- sys.stderr : 표준 오류 출력 객체. 쓰기 전용.

## 5.4 명령줄(commend line) 옵션과 인수 다루기

[argparse모듈 공식문서](https://docs.python.org/3.4/library/argparse.html)    
   
- UNIX 계열 운영체제의 관례에 따라 커맨드라인 옵션을 해석(parse)하는 기능을 제공

**(1) 명령줄 옵션 다루기**   
   
이하는 두 개의 인수를 갖는 스크립트를 예를 설명한다.   
스크립트는 문자열과 정수를 인수로 받으며 지정된 수만큼 반복하여 문자열을 표시하는 스크립트이다.   
argparse모듈을 사용하여 옵션을 정의하고, 주어진 인수를 해석하여 해석된 값을 가지고 간단한 처리를 실행한 예시이다.


```python
import argparse

# parser의 인스턴스 작성
parser = argparse.ArgumentParser(description='Example command')

# 문자열을 받는 -s 옵션 정의
parser.add_argument('-s', '--string', type=str, 
                    help='string to display', 
                    required=True)

# 수치를 받는 -n 옵션 정의
parser.add_argument('-n', '--num', type=int, 
                    help='number of times repeatedly display the string', 
                    default=2)

# 인수를 해석(parse)하여 얻어진 값을 변수로 지정
args = parser.parse_args()


# 해석으로 얻은 값 다루기. 디버깅용 처리
print(args.string * args.num)
```

    usage: ipykernel_launcher.py [-h] -s STRING [-n NUM]
    ipykernel_launcher.py: error: the following arguments are required: -s/--string
    


    An exception has occurred, use %tb to see the full traceback.
    

    SystemExit: 2
    


    C:\Anaconda3\lib\site-packages\IPython\core\interactiveshell.py:2969: UserWarning: To exit: use 'exit', 'quit', or Ctrl-D.
      warn("To exit: use 'exit', 'quit', or Ctrl-D.", stacklevel=1)
    

(교재 및 문서 참고)
