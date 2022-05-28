### sparse array

https://twitter.com/kdy1dev/status/1524667421113548801?s=21&t=u2X11U5KuFXX6z-OWyqz_g

```
const x = [...[,]];
const y = [,];

console.log(0 in x, 0 in y); // => true false
```

### flatMap

https://prateeksurana.me/blog/why-using-object-spread-with-reduce-bad-idea/

얕은 복사 vs 깊은 복사

깊은 복사가 데이터 자체를 통째로 복사 -> 원본까지 바뀜
얕은 복사가 값 자체를 복사하는 것이 아니라 주소값을 복사하여 같은 메모리를 가리킴


 deep copy는 JSON보다 immutable.js나 god lodash 사용하는 것 권장


 ### 폭죽파티

  <img src="./images/폭죽파티.png" width="500px" />