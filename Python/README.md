# Python


## 목표
* 코딩테스트를 위해 python에 빠르게 능숙해지기
* 다른 언어와는 다른 pythonic한 방법에 익숙해지기
* 다시 볼 때 찾기 쉽게 정리

## 학습 방법
* 새롭게 배우는 개념 정의하고 직접 말로 설명해보기
* 여러 다른 개념들의 관계와 차이를 정리하고 구조화하기
* 배운 개념을 코드로 쓰며 실습하기

## 파이썬 공식 문서
[공식 문서](https://docs.python.org/3/)

## 파이썬 라이브러리 뜯어보기
[라이브러리 내부코드 확인](https://github.com/python/cpython/tree/main/Lib)

[inspect모듈 활용](https://jimmy-ai.tistory.com/247)

```python
import inspect
import random

print(inspect.getsource(random))
print(inspect.getsource(random))
```


## 유용한 내장 함수
* help() : 도움말
  * class, 함수 등의 사용법을 보고 싶을 때
* dir() : 현재 메모리에 할당된 변수들 확인
  * class, 함수를 넣으면 그 안에 정의된 함수를 확인
* del() : 삭제
```python
help(help)
dir()
dir(list)          
```

## 내장 함수 시간 복잡도 확인
* https://wiki.python.org/moin/TimeComplexity
* https://www.ics.uci.edu/~pattis/ICS-33/lectures/complexitypython.txt

## 찾기
### Python
[class](Python_0723.md)

[control statements](Python_0720_0721.md)

[data structure method](Python_0725_0726.md)

[function](Python_0720_0721.md)

[loop](Python_0720_0721.md)

[try, except](Python_0724.md)

[variable, datatype](Python_0718_0719.md)

## 기타
[코드 스타일 가이드 PEP8](https://peps.python.org/pep-0008/)
[python의 유용한 내장 함수](https://ksc38317.tistory.com/21)