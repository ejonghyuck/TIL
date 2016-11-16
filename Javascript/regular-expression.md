# Regular Expression

## 정규식 만들기
1) 기본적인 사용법.
스크립트가 로딩됐을 때 컴파일을 제공한다.
같은 정규식이 계속 지속될 때 이렇게 사용하는 것이 좋다.

```js
var re = /패턴/플래그;
```

2) RegExp 생성자를 이용한 방법.
생성자 함수를 사용하면 정규식의 실시간(런타임) 컴파일을 진행한다.

```js
var re = new RegExp("패턴");
var re = new RegExp("패턴", "플래그");
```

## 정규식 패턴 작성
정규식 패턴은 /abc/와 같은 단순한 문자들로 구성되거나,
/ab*c/ 또는 /Chapter (\d+)\.\d*/와 같이 단순한 문자들과 특수 문자들의 조합으로 구성된다.

### 단순한 패턴
단순한 패턴은 직접 찾고자 하는 문자들로 구성된다.
`/abc/` 패턴일 경우 문장에서 abc가 들어있을 경우 일치하게 된다.

### 특수한 패턴
더 복잡하고 다양한 일치를 필요로 할 경우 특수한 문자들을 포함한다.
특수한 패턴들의 경우 [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/정규식#.ED.8A.B9.EC.88.98_.EB.AC.B8.EC.9E.90.EB.A5.BC_.EC.82.AC.EC.9A.A9.ED.95.98.EA.B8.B0) 혹은 [MSDN](https://msdn.microsoft.com/ko-kr/library/ae5bf541(v=vs.100).aspx)에서 확인.

### 괄호
괄호는 괄호 내용과 일치하는 부분 문자열들을 기억(기록)하게 한다.

## 정규식의 사용
메서드 | 설명
--- | ---
`exec` | 일치하는 문자열을 찾는 메서드이며, 정보를 가지고 있는 배열을 반환한다.
`test` | 일치하는 문자열이 존재하는지 검사하는 메서드이며, true나 false를 반환한다.
`match` | 일치하는 문자열을 찾는 메서드이며, 그 결과를 배열로 반환한다. 일치하는 부분이 없으면 null이 반환된다.
`search` | 첫 번째로 일치하는 부분 문자열을 찾는 메서드다. 일치하는 부분이 있으면 처음으로 일치하는 문자열의 오프셋이 반환되며, 일치하는 부분이 없으면 -1이 반환된다.
`replace` | 정규식이나 검색 문자열을 사용하여 문자열에서 텍스트를 바꾼다. 대체 후 텍스트가 반환된다.

## Refs
* [정규식 (MDN)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/정규식)
* [정규식 구문 (MSDN)](https://msdn.microsoft.com/ko-kr/library/ae5bf541(v=vs.100).aspx)
