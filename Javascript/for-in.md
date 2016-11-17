# for...in statement
for...in 문은 반복 가능한 객체(Array, Map, Set, String, TypedArray, arguments 등)의 각 요소에 대해 하나 이상의 문을 실행한다.

## 구문

```js
for (variable in object) {
    statements
}
```

* variable : 개체의 속성 이름 혹은 배열의 요소 인덱스가 variable에 할당된다.
* object : 반복할 개체 또는 배열.

## 예

### Array에 대한 반복

```js
let arr = ["zero", "one", "two"];

for (let index in arr) {
    console.log(`${index}, ${arr[index]}`);
}
// 0, zero
// 1, one
// 2, two
```

블록 내부에서 값을 변경하지 않는다면 let 대신 const로 사용하는 것도 가능하다.

```js
let arr = ["zero", "one", "two"];

for (const index in arr) {
    console.log(`${index}, ${arr[index]}`);
}
// 0, zero
// 1, one
// 2, two
```

### Map에 대한 반복

```js
let map = {a: 1, b: 2, c: 3};

for (let key in map) {
    console.log(`${key}, ${map[key]}`);
}
// a, 1
// b, 2
// c, 3
```

## Refs
* [for...in (MDN)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...in)
