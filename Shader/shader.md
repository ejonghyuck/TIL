# Shader

## Shader란?
'색의 농담, 색조, 명암 효과를 주다.' 라는 뜻의 shader란 동사와 행동의 주체를 나타내는 접미사 '-er'을 혼합해 만든 단어.

## Shader의 종류
쉐이더는 크게 `정점 쉐이더(vertex shader)`와 `픽셀 쉐이더(pixel shader)`로 나뉜다.

## 3D 파이프라인
![](https://open.gl/media/img/c2_pipeline.png)

### 정점 쉐이더란?
3D 물체를 구성하는 정점들의 위치를 화면 좌표로 변환하는 것.

### 픽셀 쉐이더란?
화면에 출력할 최종 색상을 계산하는 것. 프래그먼트 쉐이더(fragment shader)라고 부르기도 한다.

### 래스트라이저
래스터라이저(rasterizer)는 정점 쉐이더가 출력하는 정점의 위치를 차례대로 모아 모양을 만든 다음, 그 안에 들어갈 픽셀들을 찾아낸다.
그리고 래스터라이저가 찾아낸 픽셀 수만큼 픽셀 쉐이더가 호출된다.

## 쉐이더 프로그래밍이란?
쉐이더 프로그래밍이란 정점 쉐이더와 픽셀 쉐이더에서 사용할 함수를 하나씩 만드는 것을 의미한다.
DirectX에서 지원하는 쉐이더 언어는 [HLSL](https://msdn.microsoft.com/en-us/library/windows/desktop/bb509561(v=vs.85).aspx)이며, 이는 C와 매우 비슷한 문법을 사용한다.
또한 [GLSL](https://www.opengl.org/documentation/glsl/)이나 [CgFX](http://developer.download.nvidia.com/shaderlibrary/webpages/cgfx_shaders.html) 등의 쉐이더 언어 역시 서로 비슷한 문법을 가지고 있다.

* HLSL : High Level Shader Language의 약자
* GLSL : OpenGL Shader Language의 약자이며 OpenGL에서 지원하는 쉐이더 언어
* Cg : 엔비디아에서 지원하는 쉐이더 언어
