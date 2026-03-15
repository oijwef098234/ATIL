## JVM이란?

> java가 os에 구애받지 않고 동작하도록 존재하는 가상머신

- 뒤에 VM이라는 단어 자체가 Virtual Machine이다.

## 왜 사용하냐?

- OS에 종속되지 않고 CPU가 java를 인식하기 위해 사용함

## 과정

1. java 프로그램이 실행된다.
2. java compailer가 JVM이 인식할 수 있는 Java bytecode로 변환한다.(기계어 아님)
3. JVM이 Java bytecode를 OS에 맞게 기계어로 변환하여 CPU에게 넘겨준다.

## JVM의 구성요소

![image.png](attachment:c5885303-a961-4999-b5e6-c019a41aa7ca:image.png)

### 클래스 로더

- 이름 그대로 자바 프로그램의 클래스들을 로드하는 역할을 한다.

### 실행엔진

- 로드된 클래스들을 실행하는 역할을 한다.

### Runtime Data Area

- 프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간