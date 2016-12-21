# Direct3D Init

## Direct3D 초기화 과정
1. `IDirect3D9` 인터페이스 얻기
2. 하드웨어 버텍스 프로세싱을 지원하는지 알아보기 위해 `장치 특성(D3DCAPS9)`을 확인
3. `D3DPRESENT_PARAMETERS` 구조체 인스턴스를 초기화
4. 초기화된 `D3DPRESENT_PARAMETERS`에 따라 `IDirect3DDevice9` 객체를 생성

## 1. IDirect3D9 인터페이스 얻기
Direct3D 초기화는 IDirect3D9 인터페이스를 얻는 것부터 시작한다.

```cpp
IDirect3D9* d3d9 = Direct3DCreate9(D3D_SDK_VERSION);
```

Direct3DCreate9 함수는 `장치 검증`과 `IDirect3D9 객체 생성`이라는 두 가지 용도로 이용한다.

* 장치 검증 : 시스템 그래픽 장치가 제공하는 특성의 정보를 얻는 과정을 의미
* IDirect3D9 객체 생성 : IDirect3D9 객체 생성이며, 함수의 인수에는 항상 `D#D_SDK_VERSION`을 전달해야 한다.

## 2. 하드웨어 버텍스 프로세싱 확인
버텍스(vertex)는 3D 기하물체를 구성하는 기본 단위이며, 이를 처리하기 위한 버텍스 프로세싱은 `소프트웨어 버텍스 프로세싱(Software Vertex Processing)`과 `하드웨어 버텍스 프로세싱(Hardware Vertex Processing)`이 있다.
소프트웨어 버텍스 프로세싱은 모든 기기에서 동작하지만, 그래픽카드에서 직접 처리해주는 하드웨어 버텍스 프로세싱이 훨씬 빠르기 때문에 하드웨어 버텍스 프로세싱을 이용하는 것이 좋다.

문제는 하드웨어 버텍스 프로세싱이 불가능한 그래픽카드가 존재하기 때문에 (10년전 그래픽 카드도 돌아가기 때문에 그럴 일은 거의 없겠지만) 먼저 장치의 특성을 파악할 필요가 있다.

```cpp
HRESULT IDirect3D9::GetDeviceCaps(
    UINT Adapter, 
    D3DDEVTYPE DeviceType, 
    D3DCAPS9* pCaps); 
```

* Adapter : 특성을 얻고자 하는 디스플레이 어댑터를 지정한다
* DeviceType : 이용할 장치 타입을 설정한다. `D3DDEVTYPE_HAL (하드웨어)`, `D3DDEVTYPE_REF (소프트웨어)` 이렇게 존재하며, 기본적으로 하드웨어를 선택하면 된다
* pCaps : 초기화된 특성 구조체(D3DCAPS9)를 리턴한다

하드웨어 버텍스 프로세싱이 가능한지 체크하기 위해선 위에서 `IDirect3D9::GetDeviceCaps`를 통해 장치의 특성을 얻은 다음, 아래처럼 DevCaps와 D3DDEVCAPS_HWTRANSFORMANDLIGHT의 비트연산(&)을 통해서 비교하면 된다.

```cpp
int vp = 0;
if (caps.DevCaps & D3DDEVCAPS_HWTRANSFORMANDLIGHT)
    vp = D3DCREATE_HARDWARE_VERTEXPROCESSING;
else
    vp = D3DCREATE_SOFTWARE_VERTEXPROCESSING;
```

* `D3DCREATE_HARDWARE_VERTEXPROCESSING` : 하드웨어 버텍스 프로세싱을 사용
* `D3DCREATE_SOFTWARE_VERTEXPROCESSING` : 소프트웨어 버텍스 프로세싱을 사용

## 3. D3DPRESENT_PARAMETERS 구조체 채우기
`D3DPRESENT_PARAMETERS` 구조체에는 채워야 할 인수가 매우 많은 편이지만, 만들고자 하는 IDirect3DDevice9 객체의 특성을 결정하는데 이용되기 때문에 중요하게 채워야 한다.

```cpp
typedef struct _D3DPRESENT_PARAMETERS_
{
    UINT                BackBufferWidth;
    UINT                BackBufferHeight;
    D3DFORMAT           BackBufferFormat;
    UINT                BackBufferCount;
    D3DMULTISAMPLE_TYPE MultiSampleType;
    DWORD               MultiSampleQuality;
    D3DSWAPEFFECT       SwapEffect;
    HWND                hDeviceWindow;
    BOOL                Windowed;
    BOOL                EnableAutoDepthStencil;
    D3DFORMAT           AutoDepthStencilFormat;
    DWORD               Flags;
    UINT                FullScreen_RefreshRateInHz;
    UINT                PresentationInterval;
} D3DPRESENT_PARAMETERS;
```

* `BackBufferWidth` : 픽셀 단위의 후면 버퍼 너비
* `BackBufferHeight` : 픽셀 단위의 후면 버퍼 높이
* `BackBufferFormat` : 후면 버퍼의 픽셀 포맷, 주로 사용하는 것은 32비트 픽셀 포맷인 `D3DFMT_A8R8G8B8` 이다
* `MultiSampleType` : 후면 버퍼에 이용할 멀티 샘플링 타입이다. 멀티 샘플링은 주로 안티 앨리어싱(계단 현상 제거)으로 알려져 있지만 성능 저하가 발생한다
    * `D3DMULTISAMPLE_NONE` : 멀티 샘플링 미사용
    * `D3DMULTISAMPLE_2_SAMPLES` : 2단계 멀티 샘플링 사용
    * `D3DMULTISAMPLE_4_SAMPLES` : 4단계 멀티 샘플링 사용
    * `D3DMULTISAMPLE_8_SAMPLES` : 16단계 멀티 샘플링 사용 *(주로 2~4단계만 지원한다)*
* `MultiSampleQuality` : 멀티 샘플링의 퀄리티
* `SwapEffect` : 플리핑 체인의 버퍼가 교환되는 방법을 지정하는 `D3DSWAPEFFECT` 열거형의 멤버이다. `D3DSWAPEFFECT_DISCARD`를 지정하는 것이 가장 효과적이다
* `hDeviceWindow` : 서비스와 연결된 윈도우 핸들, 화면이 그려진 애플리케이션 윈도우를 지정한다
* `Windowed` : 윈도우 모드로 실행하려면 true, 전체 화면 모드로 실행하려면 false를 지정한다
* `EnableAutoDepthStencil` : Direct3D가 자동으로 깊이/스텐실 버퍼를 만들고 관리하길 원한다면 true로 설정
* `AutoDepthStencilFormat` : 깊이/스텐실 버퍼의 포맷, 24비트 깊이 버퍼와 8비트 스텐실 버퍼의 예약은 `D3DFMT_d24S8` 이다
* `Flags` : 몇 가지 부가적인 특성들을 지정한다, 사용하지 않을 때에는 0을 지정하고 다음과 같은 플래그들이 존재한다
    * `D3DPRESENTFLAG_LOCKABLE_BACKBUFFER` : 후면 버퍼를 잠글 수 있게 한다, 잠글 수 있는 후면 버퍼는 성능 저하의 원인이 된다
    * `D3DPRESENTFLAG_DISCARD_DEPTHSTENCIL` : 다음 후면 버퍼를 시연한 뒤에 깊이/스텐실 버퍼를 버린다
* `FullScreen_RefreshRateInHz` : 화면 주사율을 지정한다, 보통 `D3DPRESENT_RATE_DEFAULT`를 지정하여 기본 주사율을 사용한다
* `PresentationInterval` : 시연 간격을 지정하며 보통 수직 동기화로 알려져 있다
    * `D3DPRESENT_INTERVAL_IMMEDIATE` : 화면을 즉시 시연한다, 프레임 제한이 없으므로 화면 밀림이 발생할 수 있다
    * `D3DPRESENT_INTERVAL_DEFAULT` : 적절한 시연 간격을 Direct3D가 결정하도록 하게 한다

## 4. IDirect3DDevice9 인터페이스 만들기
`D3DPRESENT_PARAMETERS` 구조체를 모두 설정했다면 IDirect3DDevice9 객체를 생성하면 된다.
CreateDevice 함수를 이용해 생성할 수 있으며 함수의 인자들은 다음과 같다.

```cpp
CreateDevice(
    UINT Adapter, 
    D3DDEVTYPE DeviceType, 
    HWND hFocusWindow, 
    DWORD BehaviorFlags, 
    D3DPRESENT_PARAMETERS* pPresentationParameters, 
    IDirect3DDevice9** ppReturnedDeviceInterface);
```

* `Adapter` : 만들어질 IDirect3DDevice9 객체와 대응될 물리 디스플레이 어댑터를 지정한다
* `DeviceType` : 이용할 장치 타입을 지정한다
    * `D3DDEVTYPE_HAL` : 하드웨어 장치로 지정
    * `D3DDEVTYPE_REF` : 소프트웨어 장치로 지정
* `hFocusWindow` : 장치와 연결될 윈도우 핸들이며, 주로 장치가 화면을 그릴 윈도우가 된다
* `BehaviorFlags` : 버텍스 프로세싱을 지정한다, 이전에 하드웨어 버텍스 프로세싱 지원 유무를 파악하고 vp에 값을 담았기 때문에 vp를 지정하면 된다
* `pPresentationParameters` : 장치 특성의 일부를 정의하는 초기화 된 D3DPRESENT_PARAMETERS 인스턴스를 지정한다
* `ppReturnedDeviceInterface` : 생성된 장치를 리턴한다
