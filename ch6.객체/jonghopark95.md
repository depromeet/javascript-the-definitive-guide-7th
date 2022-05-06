# 6장. 객체



## 6.1 객체 소개



### attribute? property? field?

- 여러분들 다른 개발자랑 소통할 때 어떤 이름을 사용하나유?



### [Object.defineProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

- writable
- enumerable
  - for...in loop 나 Object.keys 같은 메서드의 property enumeration 에서 보이게 된다.
- configurable
- get
- set



## 6.2 객체 생성



### 객체를 생성하는 방법 3가지

- 객체 리터럴
  - 동적으로 만들어지는 객체도 리터럴이라 하는구나... (리터럴을 평가)
- new 키워드
- [Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
  - 인자로 전달된 대상을 prototype 으로 새로운 객체를 만든다...



### [Object vs Object.prototype ? (프로토타입)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object#description)

- MDN 문서를 봤을 때 차이





## 6.3 프로퍼티 검색과 설정



### associative array (연관 배열)

- 문자열을 인덱스로 사용하는 배열... (hash, map, dictionary)
- JS 는 연관 배열임



### ?. 은 앞 대상이 nullable 한지 체크하는 것이다

- 대상 프로퍼티를 찾지 못하면 undefined 임
- 





## 6.4 프로퍼티 삭제

- delete 는 configurable 이 false 면 제거하지 안흔ㄴ다... (ex. 전역 객체 프로퍼티, ...)





## 6.5 프로퍼티 테스트



### [Object.prototype.propertyIsEnumerable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/propertyIsEnumerable)

- 



### [Object.hasOwn()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwn)

- 



### in 연산자의 또 하나의 역할

- 존재하지 않는 프로퍼티와, 존재하지만 값이 undefined 인 프로퍼티를 구분할 수 있다.

  - ```js
    let o = { x: undefined; }
    o.x !== undefined //  => false
    o.y !== undefined // 	=> false
    'x' in o;					//	=> true
    'y' in o; 				//	=> false
    ```

    



## 6.6 프로퍼티 열거



### for - in vs for - of





### 열거 메서드

- Object.keys()
- Object.getOwnPropertyNames()
- Object.getOwnPropertySymbols()
- Reflect.ownKeys()



### 프로퍼티 열거 순서

- ```js
  const e = {
      'c' : 3,
      '2': 4,
      [Symbol('a')]: 1,
      '-1': 6,
      'd': 5
  } // => {2: 4, c: 3, -1: 6, d: 5, Symbol(a): 1}
  ```

- 1. 음이 아닌 정수인 문자열 프로퍼티
  2. 음수, 부동 소수점 숫자, 문자열이 이름인 프로퍼티
  3. 이름이 심벌인 프로퍼티



## 6.7 객체 확장



### Object.assign

- 후술하겠지만...





## 6.8 객체 직렬화

- primitive type, 유한 숫자는 직렬화, 복원 가능
- NaN, Infinity, -Infinity 는 null
- Date 는 ISO 형식 문자열, but parse 는 문자열로 둠
- 함수, RegExp, Error, undefined 는 직렬화, 복원 X
- 열거 가능한 프로퍼티만 직렬화
- 직렬화 할 수 없다면 프로퍼티 문자열에서 생략 



### [stringify 3번째 인자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#the_space_argument)

- 숫자면 해당 숫자만큼 space
- 문자면 \n 뒤에 해당 문자 삽입





## 6.9 객체 메서드



### Overriding

- toString
- toLocaleString
- valueOf
- toJSON





## 6.10 확장된 객체 리터럴 문법



### reduce 에 spread 연산자를 쓰면 좋지 않을 수 있다...

- n^2
- 대체제

