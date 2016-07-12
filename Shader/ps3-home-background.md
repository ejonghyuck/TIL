# Playstation 3 Home Background
Shadertoy에 업로드 한 [ps3 home background 쉐이더](https://www.shadertoy.com/view/Xtt3R4)를 설명한 문서.

현재 작성중.

[![ps3-home](image/ps3-home.png)](https://www.shadertoy.com/view/Xtt3R4)

## Shader Code

```Shader
const vec3 top = vec3(0.318, 0.831, 1.0);
const vec3 bottom = vec3(0.094, 0.141, 0.424);
const float widthFactor = 1.5;

vec3 calcSine(vec2 uv, float speed, 
              float frequency, float amplitude, float shift, float offset,
              vec3 color, float width, float exponent, bool dir)
{
    float angle = iGlobalTime * speed * frequency * -1.0 + (shift + uv.x) * 2.0;
    
    float y = sin(angle) * amplitude + offset;
    float clampY = clamp(0.0, y, y);
    float diffY = y - uv.y;
    
    float dsqr = distance(y, uv.y);
    float scale = 1.0;
    
    if(dir && diffY > 0.0)
    {
        dsqr = dsqr * 4.0;
    }
    else if(!dir && diffY < 0.0)
    {
        dsqr = dsqr * 4.0;
    }
    
    scale = pow(smoothstep(width * widthFactor, 0.0, dsqr), exponent);
    
    return min(color * scale, color);
}

void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    vec2 uv = fragCoord.xy / iResolution.xy;
    vec3 color = vec3(mix(bottom, top, uv.y));

    color += calcSine(uv, 0.2, 0.20, 0.2, 0.0, 0.5,  vec3(0.3, 0.3, 0.3), 0.1, 15.0,false);
    color += calcSine(uv, 0.4, 0.40, 0.15, 0.0, 0.5, vec3(0.3, 0.3, 0.3), 0.1, 17.0,false);
    color += calcSine(uv, 0.3, 0.60, 0.15, 0.0, 0.5, vec3(0.3, 0.3, 0.3), 0.05, 23.0,false);

    color += calcSine(uv, 0.1, 0.26, 0.07, 0.0, 0.3, vec3(0.3, 0.3, 0.3), 0.1, 17.0,true);
    color += calcSine(uv, 0.3, 0.36, 0.07, 0.0, 0.3, vec3(0.3, 0.3, 0.3), 0.1, 17.0,true);
    color += calcSine(uv, 0.5, 0.46, 0.07, 0.0, 0.3, vec3(0.3, 0.3, 0.3), 0.05, 23.0,true);
    color += calcSine(uv, 0.2, 0.58, 0.05, 0.0, 0.3, vec3(0.3, 0.3, 0.3), 0.2, 15.0,true);

    fragColor = vec4(color,1.0);
}
```

## Const
* 1 `const vec3 top` : 그라디언트를 위한 가장 윗 부분의 색상값
* 2 `const vec3 bottom` : 그라디언트를 위한 가장 밑 부분의 색상값
* 3 `const float widthFactor` : 웨이브 선의 길이값

## mainImage
### parameters
* `out vec4 fragColor` : 최종적으로 출력 될 픽셀의 컬러값
* `in vec2 fragCoord` : 텍스쳐의 위치 정보

### code
* 1 `vec2 uv = fragCoord.xy / iResolution.xy` : uv 좌표 계산
* 2 `vec3 color = vec3(mix(bottom, top, uv.y))` : Top-Down 그라디언트이기 때문에 uv의 y좌표를 기준으로 그라디언트 색상값 추출
* 3~9 `color += calcSine(~)` : calcSine 함수를 통해 색상 증감
* 10 `fragColor = vec4(color,1.0)` : 최종 컬러값 입력

## calcSine
### parameters
* `vec2 uv` : uv 좌표
* `float speed` : 웨이브 선의 속도값
* `float frequency` : 주기
* `float amplitude` : 진폭
