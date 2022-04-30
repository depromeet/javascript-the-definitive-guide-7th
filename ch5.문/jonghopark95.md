# 5장 문



*JS statement 는 자바스크립트의 문장이나 명령어이다.*



**표현식은 평가를 통해 값으로 바뀌지만, 문은 실행을 통해 어떤 동작을 수행한다.**

- 표현문(Expressions statement) : 할당, 함수 호출 처럼 부수 효과가 있는 식은 그 자체로 문이 될 수 있다.
- 선언문(Declarations statement) : 변수를 선언하거나 함수를 정의
  - ex) 함수 표현식, 함수 선언문



## 5.1 표현문



- 증감 연산자, delete, 함수 호출 등은 부수 효과가 있는 표현식이다.



## 5.2 복합문과 빈문



### 복합문

- statement 여러 개를 묶어서 하나의 statement 블록으로 만들 수 있다... 
  - JS 가 statement 하나를 예상하는 곳에 여러 개를 넣어야 할 경우 사용한다.



### 빈 문

- 빈 statement 를 실행할 때 아무 일도 하지 않는다.
  - 예를 들어, 빈 바디를 갖는 루프를 만들고자 할 때 사용한다. (사용할 일이 있나?)



## 5.3 조건문



### exhaustive check (switch)

- https://jhpa.tistory.com/18
- [eslint switch-exhaustiveness-check](https://github.com/typescript-eslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rules/switch-exhaustiveness-check.md)



### switch

- component 분기별로 return 할 때 자주 사용...
- 최근에는 조건이 두개 밖에 없었는데 (삼항 사용해도 됬는데) 좀 더 명확하게 의도를 드러내고 싶어서 사용





## 5.4 반복문

### 

### for/of vs for/in

- for/of 는 iterable 객체에서 동작한다.
- of 뒤의 대상 값이 변하면 순회 대상도 변한다. (레퍼런스가 변하면 안변함)



- for/in 은 이터러블 객체 뿐만이 아닌 어떤 객체든 사용할 수 있다.
- 이름이 symbol 인 프로퍼티는 열거하지 않는다.
- 내장 메서드는 enumerable 하지 않다.



### for/await

- 비동기 iterator 와 동작할 수 있는 for/await 이 있다.



## 5.5 점프 문

### 

### break

- break 는 가장 가까운 루프나 switch 를 즉시 빠져나간다.



### continue

- 루프 안에서 continue 를 사용하면 다음 반복으로 넘어간다.



### throw

- throw 는 모든 값들을 던질 수 있다!!
  - ex) React Suspense 는 Promise 를 throw 한다.
- throw 되면 이를 catch 할 절을 만날 때까지 상위 콜 스택을 순회한다.



## 5.6 기타 문

### use strict

- strict mode 를 실행하게 해줌
- 명시적으로 해당 지시자를 사용하지 않아도 class 바디나 ES6 모듈을 사용하게 되면 자동으로 스트릭트 모드를 따르게 된다.



## 5.7 선언



### import / export

- ```tsx
  import { Row } from "./Row";
  import { Column } from "./Column";
  // ...
  
  export default Object.assign(Table, {
    Row,
    Column
  })
  ```



