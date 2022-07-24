## Set

Javascript Set vs. Array 성능 비교에 대한 토론
https://stackoverflow.com/questions/39007637/javascript-set-vs-array-performance

## WeakMap

jotai 가 weakmap 을 쓰더라구용
https://jotai.org/docs/guides/core-internals

가비지 콜렉션이 일어나는 시점은 자바스크립트 엔진이 결정한다
https://ko.javascript.info/garbage-collection
https://github.com/thlorenz/v8-perf/blob/master/gc.md

#### When to useMemo and useCallback

https://kentcdodds.com/blog/usememo-and-usecallback

try catch에서 catch에 들어오는 error가 Error라는 보장이 있는가? unknown으로 타이핑했다.

https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-4.html#defaulting-to-the-unknown-type-in-catch-variables---useunknownincatchvariables

```tsx
try {
  executeSomeThirdPartyCode();
} catch (err) {
  // err: unknown

  // Error! Property 'message' does not exist on type 'unknown'.
  console.error(err.message);
}
Object is of type 'unknown'.
```
