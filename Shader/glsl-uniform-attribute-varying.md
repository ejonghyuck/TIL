# GLSL Uniform, Attribute, Varying
GLSL의 Uniform, Attribute, Varying를 정리 및 설명한 문서.

## Uniform
Application에서 OpenGL API를 통해 Shader로 전달되는 읽기 전용 값을 저장하는 변수.

```ShaderLab
uniform float iGlobalTime;
uniform vec2 iResolution;
```

## Attribute
Vertex Shader에서만 사용 가능한 타입.
각각의 정점(vertex) 정보를 전달하기 위해 사용.

```ShaderLab
attribute vec4 a_position;
attribute vec2 a_texCoord0;
```

## Varying
Vertex Shader의 Output이자 Fragment Shader의 Input으로 사용될 변수를 지정하는 데 사용.
Application쪽에선 건드릴 수 없는 변수.

```ShaderLab
varying vec2 texCoord;
varying vec4 color;
```

## Refs
* [[OpenGL] Uniform, Attribute, Varying](http://dalbom.tistory.com/category/OpenGL%20ES)
