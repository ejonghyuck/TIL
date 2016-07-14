# Makefile

## Make를 쓰는 이유
가장 큰 이유는 대체로 다음과 같다.

1. 소스 코드 전부를 object 파일을 만든 다음, link하는 과정을 손으로 직접 하지 않기 위해서
2. 변경된 부분만 다시 컴파일 하기 위해서

## Makefile의 구성
* **Target** : 생성하고자 하는 목적
* **Dependency** : Target을 만들기 위해 필요한 요소
* **Command** : 실행되어야 할 명령어들(쉘스크립트)
* **Macro** : 코드를 단순화 시키기 위한 방법

## clang c++11 기반의 Makefile 샘플
```Makefile
CXX=clang++
CXXFLAGS=-g -std=c++11 -Wall -pedantic
BIN=prog

SRC=$(wildcard *.cpp)
OBJ=$(SRC:%.cpp=%.o)

all: $(OBJ)
    $(CXX) -o $(BIN) $^

%.o: %.c
    $(CXX) $@ -c $<

clean:
    rm -f *.o
    rm $(BIN)
```
