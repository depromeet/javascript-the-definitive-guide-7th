# 4장 표현식과 연산자



*JS 의 표현식은 어떤 값으로 평가되는 구절이다.*



## 4.1 기본 표현식



### 기본 표현식

- 단독으로 존재하며 자신보다 더 단순한 표현식을 포함하지 않음
  - ex) 상수, 리터럴 값, 일부 키워드 (true, false, null, this, ...), 변수 참조



### 리터럴

- literal 을 google 에 검색하면 다음과 같이 나온다.
  -  representing the exact words of the original text.
  - 즉, 정확히 text 그대로를 나타낸다는 것이다.
- 따라서 숫자, 문자열, 정규 표현식, 함수, 객체 등 글자가 그대로 값으로 쓰이는 value 를 리터럴이라고 한다.
  - 다만, `{ [key]: value }` 같은 객체는 리터럴이 아니겠쥬... ㅎㅎ??





## 4.2 객체와 배열 초기화 표현식

- 딱히 말할거리가 음씀...



## 4.3 함수 정의 표현식

- 함수 정의 표현식을 함수 리터럴이라고 부를 수 있다...!
  - 함수도 객체이며, 글자 그대로 적힌 로직을 수행하기 때문에
  - 근데 잘 안쓰는듯 첨들어봄



## 4.4 프로퍼티 접근 표현식

### 조건부 프로퍼티 접근

- object?.property 는 다음 둘 중 어떤 의미를 가지는가?
  - 1. nullable 할 수 있는 object 의 property 에 접근하는 것
    2. nullable 할 수 있는 property 에 접근하는 것
  - 1번이 더 의미에 맞는 것 같다. 공식 문서도 그렇고...
  - 설계 의도를 생각해보면...
    - 애초에 nullable 한 property 는 접근했을 때 undefined 를 내려주지만 
      nullable 한 객체에 접근하면 이는 TypeError 가 일어난다. 이를 방지하기 위해 만든 것임
- **?., ?.[], ?.()** 등으로 참조할 대상이 없을 때의 동작을 수행할 수 있다.



## 4.5 호출 표현식

### method, 함수?

- 객체에 할당된 무언가를 호출하게 될 경우 이 호출은 메서드 호출이다.
  - ex) [].sort(~~~)
  - 당연히 그 반대는 함수...



### this

- 위의 method, function 의 차이는 할당되는 this 의 차이를 만들어 낸다.
  - 메서드로 호출하게 될 경우 함수 내부에서 사용하는 this 는 호출자가 된다.
  - 함수는 어떻게 보면 전역 객체가 해당 함수를 호출한다고 생각할 수 있다. 따라서 this 는 전역 객체가 된다.
- callback 함수는 호출자가 누군지에 따라 달라진다.
  - 내장 메서드는 특별한 경우가 아니면 전역 객체가 this 가 된다.
  - 예를 들어, [].map(() => {}) 에서 map 내부 콜백함수는 전역 객체가 이를 호출하게 되므로 this 는 전역 객체이다.
- eventListener 는 `addEventListener` 를 할 때 해당 element 가 이벤트가 발생하면 콜백 함수를 실행하도록 등록하는 함수이다.
  - 즉, element 에게 호출자를 위임하는 것이다.
  - 이 때문에 this 가 element 가 된다.
- setTiemout 은 묘한데...
  - 브라우저의 경우 callback 을 실행하는 주체가 window 인데... [nodejs 는 Timer](https://nodejs.org/api/timers.html#settimeoutcallback-delay-args) 임!!





## 4.6 객체 생성 표현식

### 생성자 함수

- new 를 앞에 붙여서 함수를 호출하면 생성자 함수로서 호출하게 된다.
  - 이 때, 생성자 함수로서 객체를 만들게 되면 해당 인스턴스를 this 로 바인딩한다.
- 화살표 함수는 new 를 사용할 수 없당





## 4.7 연산자 개요

### Side Effect

- 어떤 동작의 결과가 다른 구문에도 영향을 미치는 것...
- ex) 할당 연산자, useEffect, ...
  - 그래서 React 는 rendering 로직에서 side effect 를 발생시킬 수 있는 로직을 작성하면 안된다!!





## 4.8 산술 표현식



### 연산자의 동작 방식

- `+` 연산자
  - 피 연산자중 하나가 객체라면 객체에서 기본 값으로 변환시킨다.
    - Date 는 toString, 나머지는 valueOf 를 호출한다.
  - 이후 기본값이 문자열이면 다른 하나를 문자열로 변환하고 병합한다.
  - 둘 다 문자열이 아니라면 숫자로 변환하고 더한다.



### 비트 연산자

- 비트 연산자는 피 연산자에 정수 값을 예상하며, 해당 값은 FP64 가 아닌 32bit 정수인 것처럼 동작한다.
  - 즉, 필요하면 피 연산자를 숫자로 변환하고, 소수 부분과 32번째 이후 비트를 모두 버려서 coerce 시킨다.



## 4.9 관계 표현식



### equality operator vs strict equality operator

- 일단, 한국 번역인 동등 vs 일치 연산자 표현을 극혐함 (항상 헷갈림)
- == 은 값을 비교할 때 타입 변환을 허용한다.
  - 이 때문에 falsy table 같은 괴상한 규칙이 생겨났고, 예측할 수 있는 비교를 하기 위해 사전에 형변환을 하여야 했음
  - 이제 잘 안쓰지만, nullish 값들을 비교할 때 편한 경우가 있음 (저번에 얘기했으니 스킵)



### equality operator 의 동작 방식

- 하나가 null 이고 하나가 undefined 이면 같은 값이다.
- 하나가 숫자고 하나는 문자열이면 문자열을 숫자로 변환하고, 변환된 값을 사용해 비교한다.
- 값 중 하나가 boolean 이면 1, 0으로 변환하여 비교한다.
- 값 중 하나가 객체라면 객체를 기본 값으로 변환한다 (toString, valueOf).
  - 일반 적으로 내장 클래스는 valueOf 를 먼저 시도하는데, Date 는 toString 을 먼저 시도한다.
- 이외는 모두 다른 값이다.
  - NaN == NaN 도 다른 값이다.



### in operator

- in operator 는 객체의 prototype 을 chaining 하며 해당 프로퍼티가 존재하는지 순회함. 
  - 특정 객체에서만 확인하고 싶으면 Object.hasOwnProperty 를 사용하장



## 4.10 논리 표현식



### 비싼 연산이 필요한 값들은 최대한 후방에 배치하자.

- &&, || 을 사용할 때 앞의 값이 비싼 연산 비용을 사용하는데 뒤의 값이 falsy 한 경우, 성능이 떨어진다.
  - 조건을 나중에 비교해도 되거나 비싼 연산 비용을 차지하는 경우 이를 후방에 배치하자.



## 4.11 할당 표현식



### [Logical nullish assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_nullish_assignment)

- 요즘 요거 간간히 쓰는 중...

```js
const a = { duration: 50 };

a.duration ??= 10;
console.log(a.duration);
// expected output: 50

a.speed ??= 25;
console.log(a.speed);
// expected output: 25
```





## 4.12 평가 표현식



### eval

- eval 은 인자를 받아서 JS 코드로 분석할 수 있는지 시도하고, 실패하면 SyntaxError 를 일으킨다.
  - 핵심은 eval 이 자신이 호출한 코드의 변수 환경을 사용한다는 것...
    - 즉 eval 은 자신의 실행 컨텍스트를 만들어서 호출한 코드의 환경을 사용한다.
- 전역 eval 을 사용하면 전역 상태에 있는 문자열에 수정을 가할 수 있다. (잘쓰면 유용하다.)



## 4.13 기타 연산자



### 조건 연산자 (삼항 연산자)

- 옛날엔 삼항 연산자를 정말 기피했는데... 요즘엔 쓰다보니 유용한듯?



### nullish

- 저자가 nullish coalescing 을 안쓴다는데... 난 오히려 다른 표현이 더 헷갈림 (first-defined??)
  - MDN 엔 [nullish](https://developer.mozilla.org/en-US/docs/Glossary/Nullish) 라는 표현을 적극적으로 씀 (nullish assignment, nullish coalescing, ...)
  - 오히려 다른 표현으로 외우면 헷갈릴듯



### typeof

- 대부분 primitive value 와 object 이지만, 함수는 특별하게 function 으로 반환한다.



### delete

- 배열에서 delete 를 사용해 프로퍼티를 삭제하게 되면 해당 인덱스로 조회했을 때 undefined 이지만, 길이는 변하지 않는다.



### void

- 이런게 있네
- 피 연산자를 평가하고, 그 값을 버리고 undefined 를 반환한다.
- side effect 를 발생시킬 때...? 훔 실 사용처를 잘 모르겠다