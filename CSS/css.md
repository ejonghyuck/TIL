# CSS

## CSS란?
CSS는 Cascading Style Sheet의 약자이며, 마크업 언어가 실제로 어떻게 표시될 것인지를 기술하는 언어이다.
HTML, XHTML 등에 주로 많이 사용되며, W3C의 표준이다.
레이아웃과 스타일을 정의할 때 자유도가 높은 것이 특징이다.

## CSS를 왜 사용하는가?
과거에는 HTML에 디자인적 요소를 포함하여 작성하다 보니 HTML의 존재 목적인 구조화된 문서가 아니라 디자인과 구조가 함께 난리를 치는 문서가 됐다.
때문에 W3C에서 디자인적인 요소를 HTML에서 분리하기 위해 css를 개발하였다.

HTML에서 디자인적인 요소가 제외되었기 때문에 디자이너와 프로그래머 사이의 작업이 원활해지며, css 파일만 교체하여 모바일-PC처럼 특정 상황에 맞춰서 스타일을 다르게 표시하는 것이 가능해지는 것이 가장 큰 장점이다.

## CSS 맛보기
```html
<p><font face="Arial">Hello!</p>
<p><font face="Arial">Hello!</p>
<p><font face="Arial">Hello!</p>
<p><font face="Arial">Hello!</p>
```

위처럼 HTML에서 서체를 지정하기 위해서는 일일이 서체를 지정해야 했으나 css로 표현한다면 아래 한 줄을 통해 일괄적으로 적용이 가능하다.
```css
p { font-family: Arial }
```
