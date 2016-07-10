# Shader Built-In Functions
쉐이더 내장 함수들을 정리 및 설명한 문서.

## 삼각함수
### 기본 삼각함수
* sin(x)
x의 사인 값을 구한다.
* cos(x)
x의 코사인을 구한다.
* tan(x)
x의 탄젠트 값을 구한다.
### 역삼각함수
* asin(x)
x의 각 성분의 아크사인 값을 구한다.
각 성분들의 값은 [ p/2, p/2] 이내에 있어야 한다.
* acos(x)
x의 모든 성분의 아크코사인 값을 구한다.
각 성분들의 값은 [ 1, 1] 사이에 있어야 한다.
* atan(x)
x의 아크탄젠트 값을 구한다.
반환되는 값은 [ p/2, p/2]의 범위 안에 있다.
* atan2(y, x)
y/x의 아크탄젠트 값을 구한다.
y와 x의 부호는 [-p, p] 범위 안에 있는 반환 값들의 사분면을 결정하는 데 쓰인다.
atan2는 원점을 제외한 모든 다른 점들에 대해서도 잘 정의되어 있는데, 심지어 x가 0이고 y가 0이 아닌 상황에서도 잘 동작한다.
### 쌍곡 삼각함수
* sinh(x) x의 하이퍼볼릭 사인을 구한다.
* cosh(x) x의 하이퍼볼릭 코사인(hyperbolic cos ine)을 구한다.
* tanh(x) x에 대한 하이퍼볼릭 탄젠트 값을 구한다.
### 기타
* sincos(x, out s , out c)
x의 사인과 코사인 값을 구한다. sin(x)는 출력 매개변수 s에 저장되고,
* degrees(x)
라디안 값을 각도 값(0∼360도 사이)으로 바꾼다.
* radians(x)
x를 각도에서 호도(radian) 값으로 바꾼다.

## 수학함수
* sqrt(x)
제곱근(성분별).
* rsqrt(x)
제곱근의 역수인 1 / sqrt(x)를 구한다.
* exp(x)
e의 x 제곱 값인 e^x를 구한다.
* exp2(x)
2의 x 제곱 값인 2^x를 구한다.
* pow(x, y)
x의 y 제곱 값인 x^y를 반환한다.
* ldexp(x, exp)
x * 2^exp 값을 구한다.
* log(x)
지수가 e 인 x의 로그 값을 구한다.
x가 음수면 indefinite 값을 반환한다.
x가 0이면 +INF를 반환한다.
* log2(x)
지수가 2인 x의 로그 값을 구한다.
x가 음수이면 indefinite를, 0이면 +INF를 돌려준다.
* log10(x)
지수가 10인 X의 로그 값을 구한다.
x가 음수이면 indefinite를, 0이면 +INF를 돌려준다.

## 값 변환 함수
* abs(x)
절대값을 구한다(x의 각 성분에 대해).
* sign(x)
x의 부호를 구한다.
x < 0이면 -1을 돌려주고, x=0이면 0을, x > 0이면 1을 돌려준다.
* ceil(x)
x보다 크거나 같은 정수 값을 반환한다.
* floor(x)
x보다 같거나 작은 것 중 가장 큰 정수 값을 구한다.
* round(x)
x에 가장 가까운 정수를 찾는다.
* max(a, b)
a와 b 중 큰 것을 선택한다.
* min(a, b)
a와 b 중 작은 것을 선택한다.
* clamp(x, min, max)
[min, max] 범위를 벗어나지 않게 값을 자른다.
* saturate(x)
x의 값을 [0, 1] 사이가 되게 자른다.
Unity의 Mathf.Clamp01과 동일한 함수.
* lerp(a , b, s)
s가 0이면 a를, 1이면 b를 반환하는 a와 b의 선형 보간 값인 a + s(b - a)의 결과 값을 계산한다.
* step(a , x)
(x = a) ? 1 : 0 값을 반환한다.
* smoothstep(min, max, x)
x < min이라면 0을 반환하고, x > max이면 1을 반환한다. [min, max]
사이에서는 0과 1사이의 smooth Hermite 보간 값을 반환한다.
* fmod(a , b)
a/b의 부동 소수점형(실수형) 나머지를 구한다. a = i * b + f에서 I는
정수형이고, f는 x와 같은 부호를 가지고 있고, f의 절대값은 b보다는 작다.
* frac(x)
x의 소숫점 부분을 구한다
* frexp(x, out exp)
x의 가수와 지수를 계산한다.
frexp는 exp의 결과 매개변수에 저장된 가수와 지수를 계산한다.
만일 x가 0일 때는 가수와 지수의 계산 결과를 0으로 계산한다.
* modf(x, out ip)
x값을 x와 같은 부호를 같은 소수부와 정수부로 나눈다.
부호가 있는 소수 부분은 반환되고, 정수부는 out ip 값에 저장된다.

## 테스트 함수
* all(x)
x의 모든 성분들이 0이 아닌지를 테스트한다.
* any(x)
x의 성분 중 0이 아닌 것이 있는지 테스트한다.
* isfinite(x)
x가 유한이면 true, 그렇지 않으면 false를 반환한다.
* isinf(x)
x가 +INF이거나 INF이면 true를, 그렇지 않으면 false를 반환한다.
* isnan(x)
x가 NAN이거나 QNAN이면 true를, 그렇지 않으면 false를 반환한다.
CNAN:Net a numbor-잘못된 실수 값

## 벡터 함수
* cross(a , b)
a , b 벡터의 외적을 구한다.
* dot(a , b)
a , b 벡터의 내적을 구한다.
* distance(a , b)
a , b 사이의 거리를 구한다.
* len(v)
벡터 길이
* length(v)
벡터 v의 길이를 구한다.
* normalize(v)
v/ length(v)로 정규화된 벡터 v를 구한다.
v의 길이가 0이면 결과는 정의되지 않는다.
* determinant(m)
m행렬의 행렬식을 구한다.
* transpose(m)
행렬 m의 전치 행렬을 구한다.
* mul(a, b)
a와 b의 행렬 곱셈 연산을 수행한다. a가 벡터이면 행 벡터로 인식된다.
b가 벡터면 열 벡터로 인식된다. a의 열과 b의 행은 반드시 같아야 한다.
결과 값은 a행×b열인 행렬이다.

## 기타 함수
* clip(x)
x의 어떤 성분이라도 0부다 작으면 현재 픽셀 값을 버린다.
이것을 이용하여 x의 각 원소가 평면으로부터의 거리를 나타내게 하면 평면 클립도 구현할 수가 있다.
이것은 texkill 같은 명령어를 만들고자 할 때 사용할 수 있는 내장 함수이다.
* ddx(x)
스크린 공간 기준인 x 좌표의 편 미분계수를 구한다.
* ddy(x)
스크린 공간 기준인 y 좌표의 편 미분계수를 구한다.
* fwidth(x)
abs(ddx(x))+abs(ddy(x))를 반환한다.

## 고급기법에서 사용되는 함수
* faceforward(n, i, ng)
관찰자를 향하는 표면 노말을 리턴한다.
* reflect(i, n)
반사 벡터 v를 돌려준다. I는 입사 광선, n은 표면의 법선 벡터이라면 결과
값은 v = i 2 * dot(i, n) * n이다.
* refract(i, n, eta)
굴절 벡터 v를 구한다.
입사 광선은 i이고, 표면의 법선은 n이고, eta는 굴절색이다.
eta로 주어진 n과 i 사이의 각도가 너무 크다면 refract는 (0, 0, 0)을 반환한다.

## Refs
* [쉐이더 내장함수](http://blog.naver.com/inushaha/10020452097)
* [HLSL의 내장함수](http://indra207.egloos.com/4856054)
