# for...of statement
for...of 문은 반복 가능한 객체(Array, Map, Set, String, TypedArray, arguments 등)에서 가져온 반복기의 각 값에 대해 하나 이상의 문을 실행한다.

## 구문

```js
for (variable of object) {
    statements
}
```

* variable : 각 반복에 서로 다른 속성값이 variable에 할당된다.
* object : 반복되는 열거가능(enumerable)한 속성이 있는 객체.

## 예

### Array에 대한 반복

```js
let iterable = [10, 20, 30];

for (let value of iterable) {
    console.log(value);
}
// 10
// 20
// 30
```

블록 내부에서 값을 변경하지 않는다면 let 대신 const로 사용하는 것도 가능하다.

```js
let iterable = [10, 20, 30];

for (const value of iterable) {
    console.log(value);
}
// 10
// 20
// 30
```

### String에 대한 반복

```js
let iterable = "boo";

for (let value of iterable) {
    console.log(value);
}
// "b"
// "o"
// "o"
```

### Map에 대한 반복

```js
let iterable = {"a": 1, "b": 2, "c": 3};

for (let entry of iterable) {
    console.log(entry);
}
// [a, 1]
// [b, 2]
// [c, 3]

for (let [key, value] of iterable) {
    console.log(`${key}, ${value}`);
}
// a, 1
// b, 2
// c, 3
```

## Refs
* [for...of (MDN)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...of)
