# Project Settings
Visual Studio 2013(or higher)에서 DirectX 9 프로젝트 생성 및 설정하는 방법에 대하여

## DXSDK_Jun10.exe 설치
DirectX 9 프로그래밍을 하기 위해선 우선 DirectX 개발 킷이 필요하다.
Microsoft Download Center에 있는 [DirectX Software Development Kit](https://www.microsoft.com/en-us/download/details.aspx?id=6812)를 다운받으면 된다.

설치 자체는 Next를 누르면 가능하며, 설치될 위치 정도만 설정하면 될 듯 하다.

### s1023 에러 발생시
["S1023" error when you install the DirectX SDK (June 2010)](https://support.microsoft.com/ko-kr/kb/2728613) 참고

## 프로젝트 생성
DirectX 9 프로그래밍을 하기 위해선 일단 기본적으로 Win32 Project를 생성해야 할 필요가 있다.
프로젝트 생성은 Visual Studio에서 다음과 같이 진행하면 된다.

`새 프로젝트 -> Visual C++ -> Win32 Project 선택 -> 프로젝트 생성`

## 프로젝트 설정
프로젝트를 생성했기 때문에 프로젝트를 DirectX 9 전용으로 설정해야 할 필요가 있다.
`프로젝트 속성 -> VC++ 디렉터리` 에 가면 포함 디렉터리, 라이브러리 디렉터리 두 개를 수정하면 된다.

* 포함 디렉터리 : $(DXSDK_DIR)Include 추가
* 라이브러리 디렉터리 : $(DXSDK_DIR)Lib\x86 추가
