# CG-HLSL Float, Half, Fixed
CG와 HLSL에서 사용되는 Float, Half, Fixed에 대한 정리 및 설명한 문서.

## Float
32bit의 매우 정밀한 부동 소수점.

## Half
16bit의 중간 정밀도의 부동 소수점.
-6만 ~ 6만 사이의 값.

## Fixed
12bit의 낮은 정밀도의 부동 소수점.
-2 ~ 2의 사이의 값.

## Tips
* 모바일 플랫폼에선 가급적이면 fixed를 사용하자
* 색상과 단위벡터는 fixed를 사용하자
* 꼭 float을 사용해야 하는 경우가 아니면 half를 사용하자

## Refs
* [float, half, fixed](http://blog.naver.com/rapha0/220059691061)
* [Cg(Programming Language)](https://en.wikipedia.org/wiki/Cg_(programming_language))
