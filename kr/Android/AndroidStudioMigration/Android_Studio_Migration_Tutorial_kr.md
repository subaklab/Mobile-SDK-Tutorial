# DJI SDK Demo를 Android Studio로 Import하기

<!-- toc -->

DJI SDK Demo 라이브러리를 Android Studio로 import하는 방법에는 2가지가 있다. 첫번째로 Android Studio 버전의 DJI SDK Demo 라이브러리를 import하는 것이다. 두번째는 Eclipse 버전의 DJI SDK Demo를 Android Studio로 import하는 것이다.


## Android Studio 버전 Import하기

### 1. DJI Android SDK 다운받기
DJI Android SDK 다운받기 : <https://developer.dji.com/mobile-sdk/downloads/>

다운 받은 패키지의 압축을 푼다. "./V2.1.0/Android Studio" 아래 **Android Studio** 폴더를 찾는다.

### 2. 프로젝트 Import하기

**Android Studio** 폴더에는 2개 프로젝트가 있다 : **DJI_SDK_Android_GetStart** 와 **DJI-SDK-Android-V2.1.0**. 여기서 **DJI-SDK-Android-V2.1.0**를 사용할 것이다.

Android Studio를 열고 **Open an existing Android Studio project**를 선택한다. 다이얼로그 윈도우 팝업이 뜨면 프로젝트 폴더의 path를 선택을 요구한다. **./V2.1.0/Android Studio/DJI-SDK-Android-V2.1.0** 위치와 import를 하기를 선택한다.

![importAS](../../images/Android/AndroidStudioMigration/importAS.png)

 **주의:** Android SDK가 지정한 경로 'xxx'를 참조하지 못한다면 Android Studio는 'xxx'대신 기본 SDK를 사용하며 프로젝트의 local.properties를 수정할 것이다. "OK"를 클릭하여 계속 진행한다. 이런 경우는 **local.properties** 파일에 Android SDK 경로 설정이 여러분의 컴퓨터의 Android SDK 경로로 수정되어야 하기 때문이다.
 
![sdkPath](../../images/Android/AndroidStudioMigration/sdkPath.png)


## Eclipse 버전 import하기

### 1. DJI Android SDK 다운받기
DJI Android SDK 다운받기: <https://developer.dji.com/mobile-sdk/downloads/>

다운 받은 패키지의 압축을 푼다. "./V2.1.0/Eclipse" 아래 **Eclipse**  폴더를 찾는다.


### 2. 프로젝트 Import 하기

Android Studio를 열어서 **Import project(Eclipse ADT, Gradle,etc.)**를 선택한다. 다이얼로그 팝업 윈도우가 뜨면 프로젝트 경로를 선택하라고 요청한다. "./V2.1.0/Eclipse/DJI-SDK-Android-V2.1.0" 경로를 선택하여 DJI SDK Demo를 import한다.

![importEclipseProject](../../images/Android/AndroidStudioMigration/importEclipseProject.png)

모든 다이얼로그 박스를 남겨두고 "Next"를 클릭한다. 기본 값으로 남겨둔다. 마지막 창에서 "Finish"를 클릭한 후에 팝업 윈도우가 떠서 dependency 라이브러리를 추가를 묻는다. 그냥 "OK"를 클릭하자.

여러분의 프로젝트에서 DJI-SDK-LIB의 "libs" 폴더를 확인하고 .so나 jars 형태이다.

![neededLibs](../../images/Android/AndroidStudioMigration/neededLibs.png)

만약 "run" 아이콘이 회색처리 되어 있다면 edit configuration이 필요하다. "Run->Edit Configurations..."를 클릭하고 "+" 기호를 클릭하여 Android Application를 선택한다. 설정에서 "Target Device"를 "USB device"로 변경하고 "OK"를 클릭한다.

![7.png](../../images/Android/AndroidStudioMigration/7.png)
![8.png](../../images/Android/AndroidStudioMigration/8.png)

아이콘이 녹색이여야 정상이다. 이를 클릭하면 컴파일 및 데모를 안드로이드 장치에 설치된다.

![compileAndRun](../../images/Android/AndroidStudioMigration/compileAndRun.png)


