# ES6: Modules (import & export)

## Modules?
ES6에선 모듈을 정의할 수 있는 문법을 제공한다.
`import`와 `export` 키워드로 외부 모듈을 가져오거나 모듈에 대한 내용을 설정할 수 있으며, 비동기 모델을 포함하고 있다.
(비동기 모델 : 모듈이 로드될 때까지 실행되지 않는다)

## 문제점
2017년 2월, 모든 브라우저에 구현되어 있지 않다.
때문에 Babel같은 트랜스파일러를 사용해야 한다.

## 모듈의 정의
모듈의 정의는 `export`를 통해서 가능하다.

```js
export name1, name2, ..., nameN;
export *;
export default name;
```

* name : export 할 속성이나 함수, 객체
* \* : 파일 내의 정의된 모든 속성, 함수, 객체
* default : 모듈의 기본값, 기본이 되기 때문에 파일 내에서 한 번만 호출이 가능하다.

### 예제

```js
// 속성을 export 하는 것은 CommonJS의 패턴과 동일하다.
export function foo() {
    // something.
}
export {
    foo: function () {},
    bar: 'bar'
};
```

```js
// ES6에 추가된 객체 리터럴(Enhanced Object Literals)을 함께 사용하면
// 더 간결하게 변수를 export 할 수 있다.
var foo = () => {};
var bar = () => {};
export { foo, bar };
```

```js
// 모듈 전체를 export 하고자 할 땐 default 키워드를 함께 쓸 수 있다.
export default () => {}
```

```js
// export default는 CommonJS의 아래 패턴과 동일하다.
module.exports = () => {};
```

## 모듈 로드
export 한 모듈을 불러올 때에는 `import` 키워드를 사용하면 된다.

```js
import name from 'module-name';
import {
    member [as alias],
    ...
} from 'module-name';
import 'module-name' [as alias];
```

* module-name : 불러올 모듈명, 파일명
* member : 모듈에서 export 한 멤버의 이름
* alias : 불러온 멤버를 할당할 변수명

### 예제

```js
// my-module 모듈을 가져와 myModule 라는 변수에 할당한다.
import myModule from 'my-module';
```

```js
// 위 코드는 아래처럼 쓸 수 있다.
import 'my-module' as myModule;
```

```js
// 위 코드들은 CommonJS의 아래 패턴과 동일하다.
var myModule = require('my-module');
```

```js
// 모듈에서 특정한 멤버만 변수로 할당할 수 있다.
import { foo, bar } from 'my-module';
```

```js
// 위 코드는 CommonJS의 아래 패턴과 동일하다.
var myModule = require('my-module');
var foo = myModule.foo;
var bar = myModule.bar;
```

```js
// 모듈이나 멤버의 이름을 다른 변수명으로 설정할 수 있다.
// 변수명이 긴 경우 요긴하게 사용할 수 있다.
import {
    reallyReallyLongModuleName as foo
} from 'my-module';
```

```js
// 단순히 특정 모듈을 불러와 실행만 할 목적이라면
// 아래처럼 import만 하는 것도 좋다.
import 'my-module';
```
