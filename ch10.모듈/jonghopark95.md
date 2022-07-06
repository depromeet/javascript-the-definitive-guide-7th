# Module Programming in JS

모듈화 프로그래밍의 목표는 변수 범위를 사용하는 영역에 한정하고, 전역 공간으로의 노출을 줄이는데에 있습니다.

Javascript 에서는 ES6 이전까지는 모듈을 언어 자체의 기능으로 제공하지 않았습니다. 2015 년에 이르러서야 이를 자체의 기능으로 탑재하게 되었습니다. 해당 글에선 ES6 이전 JS 에서 module programming 의 역사, ES6 의 JS module 에 대한 특징과 syntax 를 기술하도록 하겠습니다.

### 목차

1. ES6 이전의 module programming
   1. DIY Module
   2. CommonJS
   3. AMD
2. ES6 module 의 특징
   1. ES6 module system 의 주요 목표
   2. ES6 module system 의 특징 및 장점
   3. `<script> vs <script type='module'>`
3. ES6 module syntax
   1. export
      1. export per module
      2. export as default
      3. export 대상 이름 변경 (as)
   2. import
      1. import as default
      2. import as asterisk
      3. import 대상 이름 변경 (as)

---

## ES6 이전의 module programming

### DIY Module

ES6 이전엔 IIFE 로 변수의 전역 공간으로서의 노출을 막을 수 있었습니다.

```js
var module = {};
function require(fileName) {
  return module[fileName] == null ? undefined : module[fileName];
}

module["counter.js"] = (function () {
  var count = 0;

  return function counter() {
    return {
      addCount() {
        count = count + 1;
      },
      minusCount() {
        count = count - 1;
      },
      getCount() {
        return count;
      },
    };
  };
})();

var counter = require("counter.js")();
counter.addCount();
counter.addCount();
counter.minusCount();
console.log(counter.getCount()); // 1
```

### CommonJS

Node.js 진영에서 자주 사용하는 모듈 시스템입니다.

CommonJS 모듈은 서버와 동기적으로 파일들을 불러오는 목적으로 디자인 되었습니다.

Node 에서 파일들은 파일 시스템에 존재하므로, 웹에서 네트워크를 통해 가져오게 되는 파일에 비해 비교적 빠르게 파일들을 가져올 수 있습니다. 이 때문에 굳이 여러 개의 파일을 하나의 파일로 번들링 할 필요가 없었습니다.

CommonJS format 은 아래와 같은 특징을 가지고 있습니다.

- 하나의 파일은 하나의 모듈을 지닐 수 있습니다.
- 동기적으로 파일들을 가져오는 특성 상 정적 분석이 가능합니다.
- [순환 의존성을 지원합니다.](https://blog.outsider.ne.kr/1283)

### Asynchronous Module Definition (AMD)

브라우저 진영에서 자주 사용하던 모듈 시스템입니다.

CommonJS 모듈은 하나의 파일이 하나의 모듈을 지니기 때문에 비교적 느린 네트워크 환경에 있는 브라우저 환경에서는 적합하지 않았습니다. AMD format 은 여러 스크립트 들을 하나의 파일로 만들어 주기 때문에 브라우저 환경에 보다 적합합니다.

AMD format 은 다음과 같은 특징을 가집니다.

- 여러 개의 모듈을 하나의 파일로 만들어줍니다.
- 동적으로 모듈을 가져올 수 있습니다.

```js
var MyModule = require("MyModule");

// module
define(["dependencies", ...], function(){
    return MyModule;
})

// import 할 때
require(["dependencies", ...], function(MyModules, ...){
    // do stuff here
});
```

## ES6 module 의 특징

### ES6 module system 의 주요 목표

앞서 언급했다시피 ES6 이전엔 CommonJS, AMD 는 저마다의 이유로 각자의 생태계를 구축하고 있었습니다.

이 때문에 같은 언어로 작성된 모듈 임에도 서로 호환이 되지 않았습니다.

ES6 에선 이를 극복하기 위해 하나의 표준을 만들고자 하였고, 동시에 각 체계의 장점을 가져오고자 하였습니다.

이에 ES6 의 모듈 시스템은 다음과 같은 주요 특징을 가지게 됩니다.

- CommonJS 와 유사하게, 모듈 간의 순환 참조를 지원합니다.
- AMD 와 유사하게, 비동기 module import 를 지원합니다.
- 정적 모듈 구조를 가집니다.

### ES6 module system 의 특징 및 장점

**_CommonJS 와 유사하게, 모듈 간의 순환 참조를 지원합니다._**

순환 참조는 다음과 같은 상황을 뜻합니다.

- 모듈 A, B 가 있을 때, A 가 B 를 참조하고, B 가 A 를 참조하는 상황
- 모듈 A, B, C 가 있을 때, A 가 B 를 참조하고, B 가 C 를 참조하고 C 가 A 를 참조하는 상황

해당 상황은 피할 수 있다면 피해야 하는 상황이지만, 불가피하게 발생하는 경우가 있습니다.

예를 들어 DOM 트리에선 부모 노드가 자식 노드를 참조하고, 자식 노드가 부모 노드를 참조하며, 특히 대규모 애플리케이션에선 리팩토링 도중에 이와 같은 상황이 발생하는 경우가 있습니다.

모듈 시스템에서 이를 지원하면 시스템 상에서 오류가 발생하지 않으므로, 좀 더 원활한 리팩토링이 가능합니다.

**_AMD 와 유사하게, 비동기 module import 를 지원합니다._**

서버의 파일 시스템과는 달리, 브라우저의 경우 네트워크 환경에 따라 모듈을 가져오는 속도가 다소 느릴 수 있습니다. 이 경우 유저에게 안 좋은 경험을 끼칠 수 있습니다.

ES6 에서의 모듈은 AMD 와 유사하게 모듈이 로드되는 시간을 고려하지 않고 코드를 작성할 수 있습니다.

**_정적 모듈 구조를 가집니다._**

ES6 모듈 시스템은 정적인 구조를 가집니다. 이 말은 ES6 에서 모듈을 항상 top level 에서 import 해야 함을 의미하며 조건문 내부에서 import 구문을 사용할 수 없음을 뜻합니다.

만약, 모듈을 동적으로 로드하고 싶다면 [import()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import) 를 사용해야 합니다.

이와 같은 구조를 택함으로써 다음과 같은 장점을 취할 수 있습니다.

- [Tree Shaking](https://webpack.js.org/guides/tree-shaking/) 을 할 수 있습니다.

- import 대상을 빠르게 lookup 할 수 있습니다.

  - 예를 들어, CommonJs 의 경우

    ```js
    // commonJS
    var lib = require("lib");
    lib.someFunc(); // => 동적으로 lib 에 someFunc 이 있는지 lookup 하고 접근합니다.
    ```

    ES6 에서 import 하는 경우 어떤 대상이 export 되는지 정적으로 알 수 있기 때문에, lookup 의 속도가 줄어듭니다.

    ```js
    import lib from "lib";
    lib.someFunc(); // => 정적으로 lib 에 someFunc 의 유무를 알고 접근합니다.
    ```

- [Typing 을 할 수 있습니다](https://www.typescriptlang.org/). 정적으로 Typing 을 분석하기 위해선 정적인 모듈 구조가 필수입니다.

### `<script> vs <script type='module'>`

ES6 로 파일을 모듈 방식으로 불러오려면 `<script type='module'>` 과 같이 작성해야 합니다.

이 방식을 취하면 `<script>` 와 아래와 같은 차이를 보입니다.

- strict mode
  - 후자는 `use strict ` 같은 구문의 존재 여부와 관계 없이 `strict mode` 에 존재하게 됩니다.
- 변수 범위
  - 전자는 top-level 에 있는 변수들이 `global` 객체에 존재하게 됩니다. 후자는 모듈 공간에서의 top-level 로 존재하며 export 하지 않으면 다른 모듈에서 참조할 수 없습니다.
- 실행 방식

  - 전자는 대상 파일을 동기적으로 실행합니다. 후자는 비동기적으로 실행합니다. 이 때문에 import 하는 대상의 load 여부를 염두하지 않고 코드를 작성할 수 있습니다.

- import 구문 사용 가능 여부
  - 전자는 `import` 구문을 사용할 수 없습니다. 어떤 파일을 읽어오려면 `import()` 를 사용해야 합니다.

## ES6 module 의 문법

### export

export 되는 모듈은 항상 `strict mode` 입니다.

export 구문은 `type='module'` 이 없는 embedded script 에서는 사용할 수 없습니다.

export 는 다음 두 가지 스타일의 방식이 있습니다.

- Named Exports (하나의 모듈에 0 ~ N 개의 대상을 내보낼 수 있습니다.)
- Defualt Exports (하나의 모듈에 1개의 대상만 내보낼 수 있습니다.)

**_named exports_**

named exports 는 모듈 내에 여러 번 사용할 수 있으며, `var, let, const, function, class` 를 export 할 수 있습니다. exports 는 top-level 에 있는 한 어느 위치에서든 사용할 수 있습니다.

```js
// 선언 이전에 export 할 수 있습니다.
export { myFunction, myVariable };

export let myVariable = Math.sqrt(2);
export function myFunction() {
  /* ... */
}

// 선언 이후에도 가능합니다.
// export { myFunction, myVariable };
```

**_default exports_**

default exports 는 모듈 내에 단 한번만 사용할 수 있습니다.

여러 번 사용하면 이전에 `default` 로 지정한 대상은 덮어씌워집니다.

```js
export default function () { /* ... */ }
export default class { .. } // 이전 대상을 덮어씌웁니다.
```

**_'as' alias_**

as 를 사용하면 기존 식별자 대신 다른 별칭으로 대상을 export 할 수 있습니다.

```js
export { myFunction as function1, myVariable as variable };
```

as alias 로 default 를 내보낼 수도 있습니다. 아래 두 구문은 같은 default export 입니다.

```js
export { myFunction as default };
export default myFunction;
```

**_Re-exporting / Aggregating_**

다른 모듈에서 export 한 대상을 가져와 다시 export 하거나, 합쳐서 export 할 수 있습니다.

예를 들어, math 라는 폴더 아래에 add, divide 라는 모듈이 있다고 가정해봅시다.

```js
~/math
ㄴ add.js
ㄴ divide.js
ㄴ index.js
```

`index.js` 에 각 모듈을 가져와 다시 export 해줌으로써 하나의 경로로 import 되게 할 수 있습니다.

```js
// index.js

import { default as add } from "./add.js";
import { default as divide } from "./divide.js";

export { add, divide };
```

### import

export 와 마찬가지로 import 되는 대상은 항상 `strict mode` 에 있습니다.

import 구문은 `type='module'` 이 없는 embedded script 에서는 사용할 수 없습니다.

만약, 해당 속성이 없는 embedded script 에서 module 을 import 해야 하는 경우, `import()` 사용을 고려할 수 있습니다.

ES6 에서 import 는 `read-only view` 입니다. 즉, 해당 값을 수정할 수 없으며 오직 read 할 수만 있습니다.

```js
export let counter = 3;
export function add() {
  counter++;
}

//------ main.js ------
import { counter, add } from "./module";

console.log(counter); // 3
add();
console.log(counter); // 4

// import 된 대상은 수정할 수 없습니다.
counter++; // TypeError
```

ES6 에서 모듈은 singleton 이므로, 여러 번 import 하더라도 하나의 instance 만 존재합니다.

즉, 위의 예시 처럼 `counter` 를 변경한다 하더라도 `another.js` 에서 import 하는 경우 다른 counter 를 가지게 됩니다.

**_single, multiple importing_**

```js
import { myExport } from "/modules/my-module.js";
import { foo, bar } from "/modules/my-module.js";
```

`bracket ({})` 을 사용하는 경우 `named exports` 를 import 할 수 있습니다.

**_default importing_**

```js
import myDefault from "/modules/my-module.js";
```

import 에 bracket 이 없는 경우 `default export` 를 import 합니다.

`default export` 로 사용한 이름 외에 다른 이름을 사용하더라도 상관없습니다.

**_entire importing_**

```js
import * as myModule from "/modules/my-module.js";
```

`asterisk (*)` 를 사용하여 `default, named exports` 를 모두 가져올 수 있습니다.

**_'as' alias_**

```js
import { reallyReallyLongModuleExportName as shortName } from "/modules/my-module.js";
```

`named exports` 로 가져온 대상의 별칭을 사용하고 싶다면 `as` alias 를 사용할 수 있습니다.

만약, import 하는 모듈의 `named export` 이름이 `default` 라면 이를 `as` 를 사용하여 별칭을 사용해야 합니다.

`default` 는 예약어이기 때문입니다.

```js
import { default as myDefault } from "/modules/my-module.js";
```

**_importing for only side effect_**

```js
import "/modules/log.js";
```

가끔 import 모듈의 반환값이 필요하진 않지만 로그 작성 등의 이유로 해당 모듈 내부의 `effect` 가 요구되는 경우가 있습니다.

이런 경우 `from` 없이 import 를 할 수 있습니다.

## 참고

- [Exploring ES6](https://exploringjs.com/es6/ch_modules.html#static-module-structure)
- [Why AMD](https://requirejs.org/docs/whyamd.html)

- [MDN - import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
- [MDN - export](
