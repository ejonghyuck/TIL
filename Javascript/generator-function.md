# function* declaration
**function\*** 선언(끝에 별표가 있는)은 `ECMAScript 2015 (ES6)` 표준에 처음 등장한 기술이며, *generator function*을 정의한다.
이 함수는 `Genereator` 객체를 반환한다.

generator function은 GeneratorFunction 생성자와 function* expression 을 사용해서 정의할 수 있다.

## 문법

```js
function* name([param[, param[, ... param]]]) {
    statements
}
```

* name : 함수명
* param : 매개변수
* statements : 함수의 본체 구문들

## 설명
Generator는 함수 외부로 빠져나갔다가, 다시 함수 내부로 돌아올 수 있는 함수다.
이 때 함수 내외로 이동하는 중에도 컨텍스트(변수 값)는 그대로 저장되어 있다.

Generator function은 호출하더라도 함수가 즉시 실행되지 않는다.
대신 함수를 위한 `iterator object`가 반환된다.
반환된 iterator의 next() 메서드를 호출하면 generator function이 실행되고, 첫 번째 `yield expression`을 만날 때까지 진행된다.
그리고 해당 yield expression이 가리키는 값이 iterator로 반환된다.
만약 실행 도중 `yield* expression`을 만나면 다른 genereator function으로 넘어가게 된다.

## 예제

### 간단한 예제

```js
function* idMaker() {
    var index = 0;
    while(index < 3)
        yield index++;
}

var gen = idMaker();

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // undefined
```

### yield*를 이용한 예제

```js
function* anotherGenerator(i) {
    yield i + 1;
    yield i + 2;
    yield i + 3;
}

function* generator(i){
    yield i;
    yield* anotherGenerator(i);
    yield i + 10;
}

var gen = generator(10);

console.log(gen.next().value); // 10
console.log(gen.next().value); // 11
console.log(gen.next().value); // 12
console.log(gen.next().value); // 13
console.log(gen.next().value); // 20
```

### Generator에 인자값을 넘기는 예제

```js
function* logGenerator() {
    console.log(yield);
    console.log(yield);
    console.log(yield);
}

var gen = logGenerator();

// the first call of next executes from the start of the function
// until the first yield statement
gen.next(); 
gen.next('pretzel'); // pretzel
gen.next('california'); // california
gen.next('mayonnaise'); // mayonnaise
```

### Generator는 생성자로써 사용될 수 없다

```js
function* f() {}
var obj = new f; // throws "TypeError: f is not a constructor"
```

## Refs
* [function* (MDN)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function*)
