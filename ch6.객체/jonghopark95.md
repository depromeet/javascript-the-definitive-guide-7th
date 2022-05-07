# 6장. 객체



## 6.1 객체 소개



### property? field? attribute? 

- 여러분들 다른 개발자랑 소통할 때 어떤 이름을 사용하나유?



### [Object.defineProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

```js
Object.defineProperty(obj, prop, descriptor)
```

- descriptor

  - configurable (boolean, **false**): 이미 있는 속성을 변경 (속성을 수정... ) 하거나 삭제될 수 있는가?
  - enumerable (boolean, **false**): enumeration 하는 동안 property 가 보여지는가?
    - 해당 속성값에 따라 property 가 Object.assign, spread operator, for ... in, Object.keys 에 의해 영향을 받을 수 있다.

  - writable (boolean, **false**): property 에 값을 설정할 수 있는가?

```js
const test = { a: 1, b: 2 };
Object.defineProperty(test, 'a', { enumerable: false });
Object.entries(test) // => [Array(2)];

Object.defineProperty(test, 'b', { writable: false });
test.b = 10;
test // => { b: 2, a: 1 };
```



## 6.2 객체 생성



### 객체를 생성하는 방법 3가지

- 객체 리터럴 (객체 내부에 표현식이 있는 경우도 리터럴이라고 하는군요...)
- new 키워드 (생성자 함수를 new 로 호출)
- Object.create()



### [Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

```js
Object.create(proto)
Object.create(proto, propertiesObject)
```

- 첫 번째 인자를 prototype 으로 새로운 객체를 만든다.
  - 해당 인자로 들어올 수 있는 값은 null, Object, primitive wrapper objects
  - 첫 번째 인자로 들어오는 대상이 primitive type 이거나 null 인 경우, 
    method 를 prototype chaining 으로 호출할 수 없기 때문에 일반적인 객체와 다르게 동작할 수 있다.
- 두 번째 인자는 만들어질 object 의 property 를 설정할 수 있게 한다.
  - 리터럴로 만들어지는 객체와 다르게, 
    특별한 설정 없이 create 로 만들게 되는 경우 enumerable 이 false 이기 때문에
    Object. keys, entries, values 등에 프로퍼티가 체킹되지 않는다.





## 6.3 프로퍼티 검색과 설정



### associative array (연관 배열)

- 문자열을 인덱스로 사용하는 배열. (hash, map, dictionary) 
- JS 는 연관 배열임



### ?. 은 앞 대상이 nullable 한지 체크하는 것이다

- 대상 프로퍼티를 찾지 못하면 undefined 임



## 6.4 프로퍼티 삭제

- delete 는 configurable 이 false 면 제거하지 않는다... (ex. 전역 객체 프로퍼티, ...)





## 6.5 프로퍼티 테스트



### [Object.prototype.propertyIsEnumerable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/propertyIsEnumerable)

- enumrable 한 속성인지, 해당 속성이 있는지 체크한다.



### [Object.hasOwn()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwn)

- Object.prototype.hasOwnProperty 와 비슷하지만, Object.hasOwn 은 static method 이다.
- Object.hasOwn 은 Object.create(null) 과 같이 hasOwnProperty 를 상속받지 못한 객체들에게도 사용할 수 있고, 
  hasOwnProperty 를 재정의한 object 에도 사용할 수 있기 때문이다.



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

- for...in 은 객체의 enumrable 한 property 를 순회한다.
  - 열거 가능한 모든 속성을 순회하며, prototype 도 순회한다.
  - Array 를 for in 으로 돌게 될 경우, 키 값을 조회하므로 의도치 못한 값을 얻을 수 있다.
- for...of 는 iterable object 의 value 를 순회한다.



### 열거 메서드

- Object.keys()
  - 열거 가능 여부를 따진다. 열거 가능한 프로퍼티 이름을 배열로 반환한다.
  - 열거 불가능한 프로퍼티, 상속 프로퍼티, 이름이 심벌인 프로퍼티는 내보내지 않는다.
- Object.getOwnPropertyNames()
  - 열거 가능 여부를 따지지 않고, 이름이 문자열인 프로퍼티 이름을 배열로 반환한다.
  - 즉, Symbol 은 반환하지 않는다.
- Object.getOwnPropertySymbols()
  - 열거 가능 여부를 따지지 않고, 이름이 심벌인 프로퍼티를 배열로 반환한다.
- Reflect.ownKeys()
  - 열거 가능 여부를 따지지 않고, 문자열, 심별도 구분하지 않고 배열로 반환한다.



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

```js
Object.assign(target, ...sources)
```

- source object 중에 enumrable 하고 own property 인 property 를 target object 에 복사한다.
  (Symbol 도 된다...)
- 만약 property definition (enumerable, writable, ...) 까지 복사하고 싶다면,
  [Object.getOwnPropertyDescriptor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor) 를 사용해야 한다.



## 6.8 객체 직렬화



### 직렬화 여부 

- stringify
  - primitive type, 유한 숫자는 stringify 가능
  - NaN, Infinity, -Infinity => null
  - Date => ISO 형식 문자열로
  - 함수, RegExp, Error, undefined stringify X
  - enumerable 하지 않으면 stringify X => property 문자열에서 생략
- parse
  - primitive type, 유한 숫자는 parse 가능
  - IOS 형식 문자열을 따로 parse 하지 않음
  - 함수, RegExp, Error, undefined parse X



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



### reduce 에 spread 연산자를 쓰는 것은 anti-pattern 이다?

[참고1 - The reduce ({...spread}) anti-pattern](https://www.richsnapp.com/article/2019/06-09-reduce-spread-anti-pattern)

[참고2 - Why using object spread with reduce probably a bad idea](https://prateeksurana.me/blog/why-using-object-spread-with-reduce-bad-idea/)



- **n^2!!**

```js
const users = [
    { active: true, id: 1, name: 'a'}, 
    { active: true, id: 2, name: 'b'}, 
    { active: false, id: 3, name: 'c'}
]

const test1 = users.reduce((acc, user) => { // O(n^2)
    if(user.active){
        return {...acc, [user.id + 10]: user.name};
    }

    return {...acc, user};
}, {})
```



- TC39 reduce desugaring

  ```js
  let ab = { ...a, ...b };
  
  // Desugars to - 
  let ab = Object.assign({}, a, b); 
  ```

  

- babel

  ```js
  const obj1 = { x: 9 }
  
  const obj2 = { y: 10 }
  
  const x = { ...obj1, ...obj2, z: 8} 
  
  
  // transpile
  function _extends() { _extends = Object.assign || function (target) { for (var i = 1; i < arguments.length; i++) { var source = arguments[i]; for (var key in source) { if (Object.prototype.hasOwnProperty.call(source, key)) { target[key] = source[key]; } } } return target; }; return _extends.apply(this, arguments); }
  
  var obj1 = {
    x: 9
  };
  var obj2 = {
    y: 10
  };
  
  var x = _extends({}, obj1, obj2, {
    z: 8
  });
  ```



- typescript

  ```ts
  const obj1 = { x: 9 }
  
  const obj2 = { y: 10 }
  
  const x = { ...obj1, ...obj2, z: 8} 
  
  // transpile...
  "use strict";
  const obj1 = { x: 9 };
  const obj2 = { y: 10 };
  const x = Object.assign(Object.assign(Object.assign({}, obj1), obj2), { z: 8 });
  ```



- 대체제

  ```js
  const users = [
      { active: true, id: 1, name: 'a'}, 
      { active: true, id: 2, name: 'b'}, 
      { active: false, id: 3, name: 'c'}
  ]
  
  const test1 = users.reduce((acc, user) => { // O(n^2)
      if(user.active){
          return {...acc, [user.id + 10]: user.name};
      }
  
      return {...acc, user};
  }, {})
  
  const test2 = {}; 
  users.forEach((user) => { // O(n)
      if(user.active){
          test2[user.id + 10] = user.name;
      } else {
          test2['user'] = user;
      }    
  })
  
  const test3 = Object.fromEntries(users
      .map((user) => {
          if(user.active){
              return [user.id + 10, user.name];
          } else {
              return ['user', user];
          }
      })
  )
  
  const test4 = users.map((user) => {
      if(user.active){
          return ({[user.id + 10]: user.name });
      } else {
          return ({['user']: user });
      }
  }).reduce((acc, curr) => Object.assign(acc, curr), {});
  
  ```

  