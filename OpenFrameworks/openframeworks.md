# OpenFrameworks

## [OpenFrameWorks](http://openframeworks.cc/ko/)란?
오픈프레임웍스(openFrameworks)는 C++를 기반으로 한 오픈 소스 라이브러리로서 "창의적인 코딩"을 위해 디자인되었다.
오픈프레임웍스는 C++로 작성 되었으며, 윈도, Mac OS X, 리눅스에서 작동하는 크로스 플랫폼 소프트웨어 프레임워크이다.
오픈프레임웍스는 Zachary Lieberman, Theo Watson, Arturo Castro, 그리고 오픈프레임웍스 커뮤니티에 의해 공동 개발되었다.

## 특징
오픈프레임웍스는 기존의 다양한 라이브러리를 통합하여 손쉽게 사용가능하도록 설계되어있다.
그래픽에는 오픈지엘이, 오디오 작동에는 rtAudio를, 글꼴에는 프리타입이, 이미지 작업에는 freeImage가, 그리고 비디오 재생에는 퀵타임이 사용되었다.

## 라이센스
openFrameworks는 MIT 라이센스에 따라 배포된다.
이 라이센스의 규정에 따라 openFrameworks는 누구나 사용이 가능하며 어떠한 목적으로도 사용이 가능하다.
(상업/비 상업적 용도, 공개 또는 개인의 용도, 오픈소스/비공개소스 등) 많은 openFrameworks 사용자들이 커뮤니티에 그들의 작업을 공유하지만, 그것이 꼭 의무는 아니다.

## [다운로드](http://openframeworks.cc/ko/download/) 및 설치
(OS X 환경 기준으로 작성, 윈도우 및 리눅스는 위에 다운로드 링크 참조)

## Xcode

### 예제 열기
1. osx 라이브러리 설치 (위 다운로드 링크 참조)
2. 앱스토어에서 Xcode 설치
3. 터미널에서 `xcode-select --install` 명령어를 통해 Xcode's command line tools 설치
4. 예제폴더(Examples) 중 하나를 선택하여 `.xcodeproj`를 눌러 Xcode 실행
5. 만약 scheme가 `openFrameworks`로 되어있을 경우 앱 이름(ex. 3DPrimitivesExample)으로 되어있는 scheme 선택

### 새로운 프로젝트 생성
1. `projectGenerator_osx` 폴더의 `projectGenerator.app`을 실행
2. 프로젝트 이름 입력 후 generator 버튼을 통해 프로젝트 생성
3. `.xcodeproj`를 눌러 Xcode 실행

### 기타
좀 더 자세한 설명 및 디버깅 관련은 [Xcode 셋업 가이드](http://openframeworks.cc/ko/setup/xcode/) 참고

![](http://openframeworks.cc/setup/xcode/example-running.png)

## Emscripten
오픈프레임웍스로 개발한 프로젝트를 웹에서 작동하게 하고 싶다면 Emscripten을 통해 오픈프레임웍스의 C++ 코드를 javascript로 변환시킨 다음, 빌드된 html 파일을 웹에서 열면 된다.

### 설치
1. [설치 페이지](http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html)에서 OS에 맞는 SDK 다운로드
2. 압축 해제 후 터미널에서 `./emsdk update`, `./emsdk install latest`, `./emsdk activate latest` 입력을 통해 설치
3. 설치가 끝나면 `source ./emsdk_env.sh` 입력으로 path 지정
### 프로젝트 컴파일
1. 터미널에서 프로젝트 디렉토리로 이동
2. `emmake make` 입력
3. 컴파일이 끝나면 bin 폴더에 html 파일이 생성됨
4. `emrun bin/ProjectName.html` 명령어를 통해 html 파일 실행
5. 만약 사파리에서 정상적으로 표시되지 않는다면 `emrun --browser chrome bin/3DPrimitivesExample.html` 명령어를 통해 크롬으로 실행

![](http://openframeworks.cc/setup/emscripten/3dprimitives.png)

## Refs
* [오픈프레임웍스(위키피디아)](https://ko.wikipedia.org/wiki/%EC%98%A4%ED%94%88%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8D%EC%8A%A4)
* [Xcode 셋업 가이드(오픈프레임웍스)](http://openframeworks.cc/ko/setup/xcode/)
* [emscripten 셋업(오픈프레임웍스)](http://openframeworks.cc/ko/setup/emscripten/)
