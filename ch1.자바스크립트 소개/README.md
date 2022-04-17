## 스터디 일시 : 4/16(토) 13:30

처음에 ?? 을 봤을때 정말 많이 헷갈림

그 이유가 falsy 랑 nullish 가 헷갈려서인데falsy table 보시면 https://dorey.github.io/JavaScript-Equality-Table/ JS 에서 서로 호환되는게 막 복잡하잖아요 그래서 == 쓰지말라는거구

기억하려는건 0, -0, "", [],null, undeifned

|| 은 이 값들이 앞서 나오면 뒤에 값을 쓴다

?? 는 null, undefined 가 앞서 나오면 뒤에 값을 쓴다

이렇게 이해하니깐 좀 더 쉬웠다

저는 지금 있는 팀에서
a == null
요걸 많이써요
쉽게 말해서 null, undefined 는 같은 것으로 본다는건데
리액트 예로들면

```jsx
const a = fetchSomething()
return a == null ? null : (
  <div><div>
)
```

이런식으로 하거든요

?. 연산자있잖아요
이게 영어로 nullish coalescing operator 였나 이렇게 말하는데
저앞에 nullish 가 null, undefined 이다!로 알면 좋지 않을까

- lodash 는 nil 이라는 표현을 쓰더라고요
  https://www.geeksforgeeks.org/lodash-_-isnil-method/
