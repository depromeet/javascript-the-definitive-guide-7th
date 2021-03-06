# 8장. 함수





## 8.1 함수 정의



***function 키워드와 arrow function 의 차이점***

- this 
  - function 키워드는 해당 함수를 호출한 시점 (runtime binding) 에 따라서 달라진다.
    만약, this 를 수정하려면 `bind()` 메서드를 사용해야 한다.
  - arrow function 은 자신의 this 바인딩을 제공하지 않는다. 
    arrow function 은 자신이 정의된 환경의 this ( lexical context ) 를 상속한다.
- 생성자 함수
  - arrow function 은 prototype 프로퍼티가 없다. 따라서 새로운 클래스의 생성자로 사용할 수 없다.





## 8.2 함수 호출



***5가지 함수 호출 방법***

- 함수로 호출
- 메서드로 호출
  - 반환값이 필요없는 경우 `this` 를 반환하도록 하면 method chain 을 구성할 수 있다.
- 생성자로 호출
  - 인자가 없는 경우 빈 괄호를 생략할 수 있다.
- call, apply 메서드로 간접적으로 호출
- JS 언어 기능을 통한 묵시적 호출
  - getter, setter 가 있는 경우 프로퍼티 값에 접근하는 경우 메서드가 호출될 수 있다.





## 8.3 함수 매개변수





## 8.4 값인 함수



***first-class citizen (1급 객체)***

1. 객체가 변수, 데이터에 할당되어질 수 있어야 한다.
2. 함수의 인자로 넘길 수 있어야 한다.
3. 리턴값을 객체를 반환할 수 있어야 한다.





## 8.5 네임스페이스인 함수



***IIFE***

```js
(function (){
  
}());
```





## 8.6 클로저



***lexical scope***

함수가 호출 시점의 스코프를 사용하는 것이 아닌 정의된 시점의 변수 스코프를 사용한다.



***클로저***

lexical scope 를 구현하기 위해선 JS 함수 객체의 내부에 코드 뿐이 아닌 스코프의 참조도 포함되어 있어야 한다.

함수 객체와 스코프를 조합한 것을 클로저라고 부른다.





## 8.7 함수 프로퍼티, 메서드, 생성자



***prototype property***

함수를 생성자로 사용하면 생성된 객체는 prototype 프로퍼티를 통해 프로퍼티를 상속한다.



***call, apply***

```
f.call(o);
f.apply(o);

// 다음 코드와 비슷하다.
o.m = f;
o.m();
delete o.m;
```



## 8.8 함수형 프로그래밍

