# Direct3D Init

## Direct3D 초기화 과정
1. IDirect3D9 인터페이스 얻기
2. 하드웨어 버텍스 프로세싱을 지원하는지 알아보기 위해 장치 특성(D3DCAPS9)을 확인
3. D3DPRESENT_PARAMETERS 구조체 인스턴스를 초기화
4. 초기화된 D3DPRESENT_PARAMETERS에 따라 IDirect3DDevice9 객체를 생성

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


## 작성중...
