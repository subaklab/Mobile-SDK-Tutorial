# 카메라 앱 만들기

<!-- toc -->

이 튜토리얼의 최종 프로젝트를 다운 받을 수 있다: <https://github.com/DJI-Mobile-SDK/Android-FPVDemo-Part2.git>. 

최종 프로젝트 코드를 다운받기를 추천하며 이 튜토리얼을 참조할 수 있도록 열어놨다.

## 준비하기

(1) Mobile SDK for Android를 아래 링크에서 다운받기 : 
<http://dev.dji.com/cn/products/sdk/mobile-sdk/downloads>

(2) 비행장치(Phantom 3 Professional, Phantom 3 Advanced or Inspire 1) 펌웨어 업데이트 하기: <http://dev.dji.com/cn/products/sdk/mobile-sdk/downloads>(
*"비행장치 펌웨어 업데이트" 참조: <http://download.dji-innovations.com/downloads/phantom_3/en/How_to_Update_Firmware_en.pdf> 펌웨어 업데이트를 위한 설명서*)

(3) 안드로이드 개발 환경 설정(아직 설정하지 않은 경우). 이 튜토리얼에서 Eclipse 4.2.2를 사용한다. 아래 링크에서 다운받을 수 있다: <https://eclipse.org/downloads/packages/eclipse-classic-422/junosr2>. 일단 Eclipse가 설치하고나면, Eclipse Android 개발 툴 Plug-In을 설치해야만 한다. 방법은 아래 링크에서 찾을 수 있다: http://developer.android.com/intl/zh-TW/sdk/installing/installing-adt.html

*주의: 구글은 Eclipse의 안드로이드 개발 도구에 대한 지원이 끝났다. 이 데모를 완성하기 위해서는 Eclipse를 사용하거나 이미 데모를 완성했다면 Android Studio로 옮기는 방법을 아래 링크에서 찾을 수 있다: <https://developer.android.com/intl/zh-TW/sdk/installing/migrate.html>. 만약에 이 튜토리얼을 Android Studio에서 따라한다면, mobile SDK 폴더는 Eclipse뿐만 아니라 Android Studio 라이브러리를 포함해야 한다. Instructions on how to import the SDK library are given for both Eclipse and Android Studio below. However, we recommend that you follow this guide using the provided installation of Eclipse, and migrate your project afterwards, as we cannot ensure that results in Android Studio will be identical to those displayed in this tutorial.*

## 프로그래밍 환경 설정하기

### Eclipse

 삭제 

### Android Studio

(1) 새로 Android Studio 프로젝트 시작하기. 원하는 application 이름을 입력한다. 'Customize the Activity' 페이지가 나올때까지 next'를 클릭한다. activity 이름은 'FPVActivity'으로 정해야 한다. layout 이름은 자동으로 'activity_fpv'가 들어간다. 'Finish'를 누른다.

(2) DJI 웹사이트에서 다운받은 SDK 패키지 압축을 푼다. 메뉴에서 File -> New -> Import Module로 이동한다. 'Source Directory'필드에 DJI-SDK-LIB 폴더를 찾는다.(Android Studio\DJI-SDK-Android-V2.1.0\Lib\DJI-SDK-LIB). 'Finish'를 누른다.

*주의: DJI 웹사이트에서 다운받은 SDK 패키지내에 'Android Studio' 폴더가 있다. 데모 프로젝트에서 사용하는 라이브러리는 동일한 SDK 패키지내에 'Eclipse' 폴더에 있다. 만약 Android Studio에서 작업하고 있다면, 올바른 라이브러리('Android Stduio' 폴더)를 사용하고 있는지 확인해야 한다. 편의를 위해 SDK 패키지 다운로드 링크를 여기서 다시 기록해 두겠다. <http://dev.dji.com/cn/products/sdk/mobile-sdk/downloads>*

![importModule](../../images/Android/FPVDemo/importModuleScreenshot.png)

다음으로 왼쪽에 있는 파일 디렉토리에 'app' 모둘에서 오른쪽 클릭을 하고 'Open Module Settings'을 클릭한다. 'Dependencies' 탭으로 이동한다. 녹색 + 표시를 누르고 'Module Dependency'를 클릭해서 ':DJI-SDK-LIB'를 선택한다. 확인을 위해 'OK'를 누른다. Gradle이 재구성을 마치면, 환경준비를 마치게 된다.

![addDependency](../../images/Android/FPVDemo/addDependencyScreenshot.png)


## App 활성화

(1) <http://dev.dji.com>에서 계정 등록한다. 일단 등록하면 오른쪽 상단 모서리에 여러분의 이름을 클릭한다. 'Mobile SDK'을 클릭하고 'Crate App'에서 생성폼을 채운다. 'Identification Code'에는 프로젝트의 패키지 이름을 입력한다.

(2) 코드의 **<uses-permission ... >** 모두를 복사해서 **AndroidManifest.xml** 파일 내부에 meta-data element를 하이라이트시킨다. 아래와 같다.  

![appKeyMetaData](../../images/Android/FPVDemo/1_appKeyMetaData2.png)

Fill in the **android:value** field with the APP KEY that you have applied for from <http://dev.dji.com>.

![appKey](../../images/Android/FPVDemo/1_appKey.png)

FPVActivity.java file 파일에서 FPVActivity 클래스에 다음 변수를 추가한다.

~~~java
private static final String TAG = "MyApp";
~~~

활성화와 관련된 log error를 식별하기 위해서 이 string 값을 이용할 것이다.

onCreate 메소드에서 다음 코드를 추가하자. 많아 보이지만 이 코드들은 **DJIDrone.checkPermission()**라 불리는 단일 함수이다. 이 메소드는 DJI의 서버에 대해서 **AndroidManifest.xml**에 추가했던 정보를 체크함으로써 app의 유효성을 확인할 수 있다. 처음으로 여러분의 app을 실행한다면, 유효성을 확인하고 나면 app이 활성화 될것이다.

~~~java
	new Thread(){
		public void run(){
			try{
				DJIDrone.checkPermission(getApplicationContext(), new DJIGerneralListener(){
					@Override
					public void onGetPermissionResult(int result){
						if(result == 0) {
							// show success
							Log.e(TAG, "onGetPermissionResult ="+result);
							Log.e(TAG, 
"onGetPermissionResultDescription="+DJIError.getCheckPermissionErrorDescription(result));
						} else {
							// show errors
							Log.e(TAG, "onGetPermissionResult ="+result);
							Log.e(TAG, 
"onGetPermissionResultDescription="+DJIError.getCheckPermissionErrorDescription(result));
						}
					}
				});
			} catch (Exception e) {
        			// TODO Auto-generated catch block
        			e.printStackTrace();
        		}
		}
	}.start();
~~~

이 코드들을 나눠보자. 모든 코드를 thread내부에 두었다는 것을 명심하자. 이렇게 하는 이유는 **checkPermission()**이 네트워크에서 실행되기 때문이고 이런 절차는 반드시 thread에서 처리해야만 한다. 네트워크 연산이 완료되기를 기다리는 동안 전체 app이 멈추지 않게 하기 위해서다.

**checkPermission()**는 2개의 인자를 가진다: context와 **DJIGerneralListener()**. **DJIGerneralListener()**는 **onGetPermissionResult()**라는 1개 메소드를 가지는 인터페이스다: 이는 callback함수처럼 동작해서 **checkPermission()**가 응답을 받을 때 해야하는 것을 정의한다.

~~~java
@Override
public void onGetPermissionResult(int result){
	if(result == 0) {
		// show success
		Log.e(TAG, "onGetPermissionResult ="+result);
		Log.e(TAG, 
"onGetPermissionResultDescription="+DJIError.getCheckPermissionErrorDescription(result));
	} else {
		// show errors
		Log.e(TAG, "onGetPermissionResult ="+result);
		Log.e(TAG, 
"onGetPermissionResultDescription="+DJIError.getCheckPermissionErrorDescription(result));
	}
}
~~~

**onGetPermissionResult()**는 **result**라는 하나의 정수를 인자로 갖는다. **result**는 app에다가 에러 코드를 반환한다. 만약 error 코드가 0이면 app은 성공적으로 유효성을 검증한 것이다. 만약 그렇지 않다면 **onGetPermissionResult()**는 Log를 이용해서 적절한 error 메시지를 출력한다. 여러분의 app을 개발할 때, error 코드를 처리하는게 좋을지 말지를 결정해야 한다.

(3) 활성화 절차를 완료하기 위해서 Android 장치나 Android emulator에서 여러분의 프로젝트를 실행하자. 여러분의 코드를 실행에 관련된 내용은 아래에서 찾을 수 있다: <http://developer.android.com/intl/zh-TW/tools/building/building-eclipse.html>

반환 메시지를 보기 위해서는 개발 환경 창의 밑에 있는'LogCat' 패널을 확인하라. "MyApp"을 표시하는 'Tag' 필드로 반환 메시지를 식별할 수 있다.

![logcat](../../images/Android/FPVDemo/logcatScreenshot.png)

아래 테이블의 error 코드를 확인하자.


Error Code  	  | Description 
------------- | -------------
0   | Check permission successful
-1  | Cannot connect to Internet
-2  | Invalid app key
-3  | Get permission data timeout
-4  | Device uuid not match
-5  | Project package name does not match the app 	   key's identification code
-6  | App key is forbidden
-7  | Activated device number is up to the maximum 		available one
-8  | App key's platform is not correct
-9  | App key does not exist
-10 | App key has no permission
-11 | Server parser failed
-12 | Error in server obtaining uuid
-13 | Server app package name abnormal
-14 | Server parsing activation data failed
-15 | AES 256 encryption unsupported
-16 | AES 256 encryption failed
-17 | Get device uuid failed
-18 | Empty app key
-1000 | Server error 

0이 아닌 error 코드를 받으면 아래와 같은 지시를 따르도록 하자:

1. 인터넷에 연결되어 있는지 확인하기
2. http://dev.dji.com 사이트에서 앱을 생성할 때, 입력한 'Identification Code'와 프로젝트 패키지 이름이 같은지 확인하기 
3. APP KEY의 설치한 용량 제한을 넘었는지 확인하기. 만약 이런 문제가 해결되지 않는다면, 추가 문제해결을 위한 테이블을 참고하자.
추가 질문이 있다면 mobile SDK 지원을 담당하는 <sdk@dji.com>에 메일일 보낸다.



## Android Open Accessory (AOA) 지원 추가하기

DJI의 최신 리모트 컨트롤러의 최신 펌웨어는 USB Accessory를 이용해서 외부 장치에 연결한다. USB Debugging 예전 모델이 했던 것과 비슷하다. 이를 위해서 AOA 지원이 필요하다. 미래에는 DJI 드론을 이용해서 여러분의 app을 만들려고 할 때, 아래 보이는 바와 같이 AOA 지원이 필요할 것이다. 이 설정은 이전 리모트 장치와 하위 호환성을 가진다. 

(1) **AndroidManifest.xml**를 수정해서 **.DJIAoaActivity**가 main activity가 되도록 한다. 따라서 여러분의 app이 시작될 때 entry 포인트가 되도록 수정한 것이다. 이를 위해서 **<activity ... >** 엘리먼트를 찾아서 **android:name** 값을 **.FPVActivity**에서 **.DJIAoaActivity**로 바꾼다.

여러분의 **AndroidManifest.xml** 파일내에 'manifest' 엘리먼트 아래에 다음 code를 추가한다:
~~~xml
<uses-feature android:name="android.hardware.usb.accessory" android:required="false" />
<uses-feature android:name="android.hardware.usb.host" android:required="false" />
~~~ 

'application' 엘리멘트 아래에 다음 코드를 추가한다 :
~~~xml
uses-library android:name="com.android.future.usb.accessory"
~~~
**AndroidManifest.xml** 파일은 아래와 같아야 한다 :

~~~xml
	...
	
	<uses-feature android:name="android.hardware.usb.accessory" android:required="false" />
	<uses-feature android:name="android.hardware.usb.host" android:required="false" />
	<application
		android:label="@string/app_name"
		android:theme="@style/AppTheme">
		
		<uses-library android:name="com.android.future.usb.accessory" />
	...
		<activity
			android:name=".DJIAoaActivity"
			android:configChanges="orientation|screenSize|keyboardHidden|keyboard"
			android:screenOrientation="sensorLandscape" >
			
			<intent-filter>
				<action android:name="android.intent.action.MAIN" />
				<category android:name="android.intent.category.LAUNCHER" />
			</intent-filter>
			
			<intent-filter>
				<action android:name="android.hardware.usb.action.USB_ACCESSORY_ATTACHED" />
			</intent-filter>
			
			<meta-data
				android:name = "android.hardware.usb.action.USB_ACCESSORY_ATTACHED"
				android:resource = "@xml/accessory_filter" />
		</activity>
~~~

(2) 새로운 Android Activity 페이지를 생성하고 **DJIAoaActivity**를 activity 이름으로 사용하라. ()
Create a new Android Activity Page, using **DJIAoaActivity** as the activity name. (package에서 오른쪽 클릭을 해서 New -> Other -> Android -> Android Activity) 새로 생성한 **DJIAoaActivity.java** 파일에서 **onCreate()** 메소드에서 AOA 지원이 가능하도록 다음 코드로 교체하자.

~~~java
	private static boolean isStarted = false;
	...
	@Override
	protected void onCreate(Bundle savedInstanceState){
		super.onCreate(savedInstanceState);
		setContentView(new View(this));
		
		if (isStarted) {
			//Do nothing
		} else {
			isStarted = true;
			ServiceManager.getInstance();
			UsbAccessoryService.registerAoaReceiver(this); 
			Intent intent = new Intent(DJIAoaActivity.this, FPVActivity.class);
			startActivity(intent);
		}
		
		Intent aoaIntent = getIntent();
		if(aoaIntent != null) {
			String action = aoaIntent.getAction();
			if (action==UsbManager.ACTION_USB_ACCESSORY_ATTACHED || action == Intent.ACTION_MAIN){
				Intent attachedIntent = new Intent();
				attachedIntent.setAction(DJIUsbAccessoryReceiver.ACTION_USB_ACCESSORY_ATTACHED);
				sendBroadcast(attachedIntent);
			}
		}
		finish();
	}
	...
~~~ 

코드를 하나씩 살펴보자.

app이 시작할 때만 설정이 일어나도록 변수 **isStarted**를 생성했다.

~~~java
	private static boolean isStarted = false;
~~~

~~~java
	if (isStarted) {
		//Do nothing
	} else {
		isStarted = true;
		ServiceManager.getInstance();
		UsbAccessoryService.registerAoaReceiver(this); 
		Intent intent = new Intent(DJIAoaActivity.this, FPVActivity.class);
		startActivity(intent);
	}
~~~

app이 사작되면, main activity인 **FPVActivity**에서 intent를 설정해 준다. 아래 2줄 코드와 같다 :

~~~java
Intent intent = new Intent(DJIAoaActivity.this, FPVActivity.class);
startActivity(intent);
~~~

*주의: 향후 여러분의 app이 DJI remote와 사용하는 AOA 지원이 추가될 때, "FPVActivity.class"를 여러분의 class 이름으로 대체하기를 원할 것이다. 추가로 여러분이 첫번째 activity에 'FPVActivity'을 주지 않았다면 대신에 여러분이 여기서 선택한 이름을 넣으면 된다.*

다음으로 remote에 연결하기 위해서 broadcast를 보내는 역할을 하는 코드를 보자.

~~~java
Intent aoaIntent = getIntent();
if(aoaIntent != null) {
	String action = aoaIntent.getAction();
	if (action==UsbManager.ACTION_USB_ACCESSORY_ATTACHED || action == Intent.ACTION_MAIN){
		Intent attachedIntent = new Intent();
		attachedIntent.setAction(DJIUsbAccessoryReceiver.ACTION_USB_ACCESSORY_ATTACHED);
		sendBroadcast(attachedIntent);
	}
}
~~~

먼저 이 코드는 여기로 오게 만든 intent의 action을 얻는다. 이를 통해 app이 열렸는지 확인할 수 있고, app이 실행되는 장치가 DJI remote에 연결되었는지 확인할 수 있다. 만약 그렇다면, app과 DJI remote 사이에 연결을 설정하는 broadcast를 보낸다.

~~~java
if (action==UsbManager.ACTION_USB_ACCESSORY_ATTACHED || action == Intent.ACTION_MAIN){
	// Send broadcast
}
~~~ 


(3) Additionally, we will need to pause the AOA data connection service when the app paused (when it is partially visible), and resume the data connection when the app is resumed (when it is once again fully visible). 

Create a new Android Activity Page called **DemoBaseActivity**, and add the following code. This code allows you to pause or resume the AOA data connection service when the **onPause()** or **onResume()** lifecycle callbacks are called.

This will be our project's base activity. Change your **FPVActivity** class header so that it now extends **DemoBaseActivity**, rather than **Activity**.

~~~java
	...
	@Override
	protected void onResume(){
		super.onResume();
		ServiceManager.getInstance().pauseService(false); // Resume the service
	} 
	
	@Override
	protected void onPause() {
		super.onPause();
		ServiceManager.getInstance().pauseService(true); // Pause the service
	}
~~~	
	
## Implementing the First Person View

We're almost there! We've activated our app and set up a verification mechanism, as well as established a data connection between our app and the DJI remote controller. All that's left to do is create a live video feed of the Drone's camera to be viewed through the app.

(1) Before we start using the SDK API, we have to initiate it according to the type of the aircraft we are using. Unfortunately, as of now, there exists no way to automatically detect what type of aircraft the app is connected to. This means that we must either hardcode in which type of drone we are using, or have some sort of user input. For the purposes of this tutorial, we will be hardcoding in that we are using the Inspire 1. However, our method includes a **switch()** statement that allows us to change a simple variable **DroneCode** in our code if other drones are to be used.

In 'FPVActivity.java', in the FPVActivity class, add the variable below.

~~~java
private int DroneCode;
~~~

Import the following package.

~~~java
import dji.sdk.api.DJIDroneTypeDef.DJIDroneType;
~~~

Add two lines of code in the 'onCreate' method as shown below. Additionally, within the 'FPVActivity' class, copy the 'onInitSDK' method shown below.

~~~java
	@Override
	protected void onCreate(Bundle savedInstanceState){
		
		...
		DroneCode = 1; 
		onInitSDK(DroneCode);
		...
	}
	
	private void onInitSDK(int type){
		switch(type){
			case 0: {
				DJIDrone.initWithType(this.getApplicationContext(), DJIDroneType.DJIDrone_Vision);
				// The SDK initiation for Phantom 2 Vision or Vision Plus 
				break;
			}
			case 1: {
				DJIDrone.initWithType(this.getApplicationContext(), DJIDroneType.DJIDrone_Inspire1); 
				// The SDK initiation for Inspire 1 or Phantom 3 Professional.
				break;
			}
			case 2: {
				DJIDrone.initWithType(this.getApplicationContext(), DJIDroneType.DJIDrone_Phantom3_Advanced);
				// The SDK initiation for Phantom 3 Advanced
				break;
			}
			case 3: {
				DJIDrone.initWithType(this.getApplicationContext(), DJIDroneType.DJIDrone_M100);
				// The SDK initiation for Matrice 100.
				break;
			}
			default:{
				break;
			}
		}
	}
	
	...
~~~

(2) After initiating the SDK API, we have to connect to the drone. In the 'onCreate' method, use the following line of code to connect to the aircraft. Make sure to call this method only after the code that activates your APP key.

~~~java
	DJIDrone.connectToDrone(); // Connect to the drone
~~~	

(3) Now that the API has been initiated and we have connected to the drone, we can connect a video feed. If these two processes are not carried out first, calling API functions will have no result. Locate the **activity_fpv.xml** file (res/layout/activity_fpv.xml) and add the following **DjiGLSurfaceView** element code in the **activity_fpv.xml** file.

~~~xml
	<dji.sdk.widget.DjiGLSurfaceView
		android:id="@+id/DjiSurfaceView_02"
		android:layout_width="fill_parent"
		android:layout_height="fill_parent" />
~~~	

This view is responsible for displaying the video stream from the DJI drone.

In your 'FPVActivity.java' file, in the FPVActivity class, add the objects as shown below.

~~~java
private DJIReceivedVideoDataCallBack mReceivedVideoDataCallBack = null;
private DjiGLSurfaceView mDjiGLSurfaceView;
~~~

Add the following code in the 'onCreate' method, making sure to insert it after where you call the connectToDrone() method.

~~~java
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_fpv);
		
		...
		
		mDjiGLSurfaceView = (DjiGLSurfaceView)findViewById(R.id.DjiSurfaceView_02);
		mDjiGLSurfaceView.start();

		mReceivedVideoDataCallBack = new DJIReceivedVideoDataCallBack(){
			@Override
			public void onResult(byte[] videoBuffer, int size){
				mDjiGLSurfaceView.setDataToDecoder(videoBuffer, size);
			}
		};
		DJIDrone.getDjiCamera().setReceivedVideoDataCallBack(mReceivedVideoDataCallBack);
	}
~~~	

Let's work our way through this chunk of code.

Firstly, we associate our object **mDjiGLSurfaceView** with the **DjiSurfaceView_02** element we created in our **activity_fpv.xml** file just before.

~~~java
mDjiGLSurfaceView = (DjiGLSurfaceView)findViewById(R.id.DjiSurfaceView_02);
mDjiGLSurfaceView.start();
~~~

We then set our callback function **mReceivedVideoDataCallBack** that we just declared above.

~~~java
mReceivedVideoDataCallBack = new DJIReceivedVideoDataCallBack(){
@Override
public void onResult(byte[] videoBuffer, int size){
	mDjiGLSurfaceView.setDataToDecoder(videoBuffer, size);
	}
};
~~~

**mReceivedVideoDataCallBack** will now take the raw video data (raw H264 format) from the Drone's camera and feed it to our **DjiGLSurfaceView** element to handle, where a decoder provided by DJI will decode the raw data, upon which the **DjiGLSurfaceView** element will display it in our app window!

Finally, we set this callback function to be called when we receive data from the Drone's camera.

~~~java
DJIDrone.getDjiCamera().setReceivedVideoDataCallBack(mReceivedVideoDataCallBack);
~~~

(4) Finally, when the app is closed, we must terminate the data decoding process, then destroy our **DjiGLSurfaceView** element. If an **onDestroy()** method does not already exist, create the method using the code below. Otherwise, copy the code below into your existing **onDestroy()** method.

~~~java
	...
	
	@Override
	protected void onDestroy() {
		if (DJIDrone.getDjiCamera() != null) {
			DJIDrone.getDjiCamera().setReceivedVideoDataCallBack(null);
		}
		mDjiGLSurfaceView.destroy();
		
		...
	}
~~~

It is extremely important that in the **onDestroy()** method, you first terminate the video data processing operation by setting the callback function to **null** before you destroy the **DjiGLSurfaceView** object, as shown above. If you destroy the surface view first, the callback function will continue to send data to an object that does not exist, which could crash your app.

Conversely, in your **onCreate()** method, you must start the **DjiGLSurfaceView** object before assigning the callback function, for the same reason (refer to the beginning of step (3) where we modify the **onCreate()** method for an example of where we have already done this).

## Connecting to your Aircraft

After you have built and run the project successfully, you can now connect your mobile device to an aircraft to check the FPV. Follow the appropriate instructions for your specific aircraft model:

### 1. Connecting to a DJI Inspire 1 or Phantom 3 Professional/Advanced:

1. Turn on your remote controller, then turn on your aircraft

2. Connect your mobile device to the remote controller using a USB cable. Tap your own app and a message window "Choose an app for the USB device" will prompt.

3. Tap "OK" when the message window prompts "Allow the app to access the USB accessory".

4. Tap "OK" when the activation alert displays.

5. You are ready to use the FPV View app. 

### 2. Connecting to a DJI Phantom 2 Vision+ or Phantom 2 Vision:
	
1. Turn on your remote controller, then turn on your aircraft.

2. Ensure that the mobile device has access to the Internet. Tap the app to activate and select "OK" when the activation is done.

3. Turn on the Wi-Fi range extender

4. Turn on the Wi-Fi on your mobile device and connect to the Wi-Fi network of Phantom-xxxxxx (where xxxxxx is your range extender’s SSID number)

5. You are ready to use the FPV View app.

## Checking your results
If you can see the live video stream in the app, congratulations! You've succesfully implemented a First Person View!

![runAppScreenShot](../../images/Android/FPVDemo/runAppScreenShot.png)

## Creating a Handler

We will be using a handler to display error and confirmation messages. Set up this handler by copying the code below into your FPVActivity class:

~~~java
private Handler handler = new Handler(new Handler.Callback() {
        
        @Override
        public boolean handleMessage(Message msg) {
            switch (msg.what) {
                case SHOWDIALOG:
                    showMessage(getString(R.string.demo_activation_message_title),(String)msg.obj); 
                    break;
                case SHOWTOAST:
                    Toast.makeText(FPVActivity.this, (String)msg.obj, Toast.LENGTH_SHORT).show();
                    break;
                default:
                    break;
            }
            return false;
        }
    });
    
    private Handler handlerTimer = new Handler();
    Runnable runnable = new Runnable(){
        @Override
        public void run() {
        // handler自带方法实现定时器
        try {

            handlerTimer.postDelayed(this, TIME);
            viewTimer.setText(Integer.toString(i++));

        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        }
    };
~~~

## Implementing the Capture Function

The **private void captureAction()** function is used to take photos. In our source code, we implement a "Capture" button which calls this function whenever pressed.

~~~java
	 // function for taking photo
    private void captureAction(){
        
        CameraMode cameraMode = CameraMode.Camera_Capture_Mode;
        // Set the cameraMode as Camera_Capture_Mode. All the available modes can be seen in
        // DJICameraSettingsTypeDef.java
        DJIDrone.getDjiCamera().setCameraMode(cameraMode, new DJIExecuteResultCallback(){

            @Override
            public void onResult(DJIError mErr)
            {
                
                String result = "errorCode =" + mErr.errorCode + "\n"+"errorDescription =" + DJIError.getErrorDescriptionByErrcode(mErr.errorCode);
                if (mErr.errorCode == DJIError.RESULT_OK) {
                    CameraCaptureMode photoMode = CameraCaptureMode.Camera_Single_Capture; 
                    // Set the camera capture mode as Camera_Single_Capture. All the available modes 
                    // can be seen in DJICameraSettingsTypeDef.java
                    
                    DJIDrone.getDjiCamera().startTakePhoto(photoMode, new DJIExecuteResultCallback(){

                        @Override
                        public void onResult(DJIError mErr)
                        {
                            
                            String result = "errorCode =" + mErr.errorCode + "\n"+"errorDescription =" + DJIError.getErrorDescriptionByErrcode(mErr.errorCode);
                            handler.sendMessage(handler.obtainMessage(SHOWTOAST, result));  // display the returned message in the callback               
                        }
                        
                    }); // Execute the startTakePhoto API if successfully setting the camera mode as
                    	// Camera_Capture_Mode 
                } else {
                    handler.sendMessage(handler.obtainMessage(SHOWTOAST, result)); 
                    // Show the error when setting fails
                }
                
            }
            
        });
                   
    }
~~~

That was a lot of code we just threw at you, so let's break it down.

The first thing we need to do is define a CameraMode enum, which we will use to set the mode of the camera onboard our DJI Drone.

~~~java
CameraMode cameraMode = CameraMode.Camera_Capture_Mode;
~~~

The reason we defined this enum 'cameraMode' was so that we could pass it as a parameter for the **setCameraMode()** function that we are about to call. **setCameraMode()** sets the mode of the DJI drone's camera (Capture Mode, Playback Mode, Record Mode etc.). **setCameraMode()** takes in two parameters:

**setCameraMode(DJICameraSettingsTypeDef.CameraMode mode, DJIExecuteResultCallback mCall)**

The first parameter, a CameraMode enum, tells the function what mode to set the camera to. In this case, we tell it to set the camera to Capture Mode.
The second parameter is a callback function, which is run after **setCameraMode()** attempts to set the camera mode. The callback function is reproduced, in brief, below.

~~~java
new DJIExecuteResultCallback(){

            @Override
            public void onResult(DJIError mErr)
            {

                String result = "errorCode =" + mErr.errorCode + "\n"+"errorDescription =" + DJIError.getErrorDescriptionByErrcode(mErr.errorCode);
                if (mErr.errorCode == DJIError.RESULT_OK) {
                    // Take a photo!
                } else {
                    handler.sendMessage(handler.obtainMessage(SHOWTOAST, result)); 
                    // Show the error when setting fails
                }

            }

        });
~~~

The callback function takes in a confirmation signal from the drone, in the form of a DJIError object 'mErr'. If the error code given by 'mErr' matches the value DJIError.RESULT_OK, then code to take the photo is carried out. Else, a handler will show an appropriate error message depending on the error code.

Within the callback function we have code to tell the drone to take a photo.

~~~java
CameraCaptureMode photoMode = CameraCaptureMode.Camera_Single_Capture; 
// Set the camera capture mode as Camera_Single_Capture. All the available modes 
// can be seen in DJICameraSettingsTypeDef.java

DJIDrone.getDjiCamera().startTakePhoto(photoMode, new DJIExecuteResultCallback(){

	@Override
    public void onResult(DJIError mErr)
    {
    	String result = "errorCode =" + mErr.errorCode + "\n"+"errorDescription =" + DJIError.getErrorDescriptionByErrcode(mErr.errorCode);
        handler.sendMessage(handler.obtainMessage(SHOWTOAST, result));  // display the returned message in the callback               
    }
}); // Execute the startTakePhoto API if successfully setting the camera mode as
                        // Camera_Capture_Mode
~~~

If this code looks familiar, it's because it follows a structure almost identical to the larger function it is a part of! First we create a CameraCaptureMode enum called 'photoMode'. When the drone takes a photo, this enum instructs the drone whether it should take a single photo, a burst of photos, or a continuous stream of photos. For this example we'll be taking a single photo at a time.

The **startTakePhoto()** method tells the drone's camera to take a photo. Just like the **setCameraMode()** function, it takes in an enum and a callback function. We've just gone over what the enum it takes in is.
The callback function uses a handler to display a message giving an error code and an error description after the drone's camera attempts to take a photo. If a photo has successfully been taken, this message will confirm it.

And that's it! Add a "Capture" button into your app which calls this method, and give it a go!


## Implement the Recording Function

The **recordAction()** method is almost identical to the **captureAction()** method we just implemented, with just a few key differences! Take a quick look at the code below:

~~~java
	 // function for starting recording
    private void recordAction(){
        // Set the cameraMode as Camera_Record_Mode.
        CameraMode cameraMode = CameraMode.Camera_Record_Mode;
        DJIDrone.getDjiCamera().setCameraMode(cameraMode, new DJIExecuteResultCallback(){
		 
            @Override
            public void onResult(DJIError mErr)
            {
                
                String result = "errorCode =" + mErr.errorCode + "\n"+"errorDescription =" + DJIError.getErrorDescriptionByErrcode(mErr.errorCode);
                if (mErr.errorCode == DJIError.RESULT_OK) {
                    
                    //Call the startRecord API
                    DJIDrone.getDjiCamera().startRecord(new DJIExecuteResultCallback(){

                        @Override
                        public void onResult(DJIError mErr)
                        {
                            
                            String result = "errorCode =" + mErr.errorCode + "\n"+"errorDescription =" + DJIError.getErrorDescriptionByErrcode(mErr.errorCode);
                            handler.sendMessage(handler.obtainMessage(SHOWTOAST, result));  // display the returned message in the callback               
                            
                        }
                        
                    }); // Execute the startTakePhoto API
                } else {
                    handler.sendMessage(handler.obtainMessage(SHOWTOAST, result));
                }
                
            }
            
        });
        
    }
~~~

Notice that the cameraMode enum has been set as **Camera_Record_Mode** because this time we want the camera to record.

~~~java
// Set the cameraMode as Camera_Record_Mode.
CameraMode cameraMode = CameraMode.Camera_Record_Mode;
~~~

Additionally, within our callback function, we call **startRecord()** instead of **startTakePhoto()**. **startRecord()** only takes in one parameter, a callback function. It does not take in an enum as **startTakePhoto()** does, as there is only one recording mode.
## Implement the Stopping Recording Function

Once the camera starts recording, we need some way to tell it to stop! That's where **stopRecord()** comes in. The code below should look quite familiar to you by now:

~~~java
	 // function for stopping recording
    private void stopRecord(){
    // Call the API
        DJIDrone.getDjiCamera().stopRecord(new DJIExecuteResultCallback(){

            @Override
            public void onResult(DJIError mErr)
            {
                
                String result = "errorCode =" + mErr.errorCode + "\n"+"errorDescription =" + DJIError.getErrorDescriptionByErrcode(mErr.errorCode);
                handler.sendMessage(handler.obtainMessage(SHOWTOAST, result));

            }
            
        });
    }

~~~

You can now add a 'Record' and 'Stop Recording' button to your app, and have them call **recordAction()** and **stopRecord()** respectively. Build and run the project, and it should look something like the screenshot below:

![recordVideoScreenShot](../../images/Android/FPVDemo/recordVideo.png)

Then congratulations! Your Aerial First Person View Android app is complete, capable of viewing your DJI Drone's video feed, as well as remotely taking picture and videos!

## Viewing your Images

Unfortunately, this tutorial does not include guidance on viewing photos and videos onboard your DJI Drone's SD card. However, if you would like to see the pictures and videos you took through your brand new app, you can download DJI's Pilot App, found here:
<https://play.google.com/store/apps/details?id=dji.pilot&hl=en>
Alternatively, you can search for the app in the Google Play Store under the name 'DJI Pilot'. 

## Summary

You’ve come a long way in this tutorial: you’ve learned how to use the DJI Mobile SDK to show the FPV view of the aircraft's camera and control the camera of a DJI platform. These features, **Capture** and **Record** are the most basic and common features in a typical drone mobile app. However, if you want to create a drone app that is more fancy, you still have a long way to go. More advanced features would include previewing the photo and video in the SD Card, showing the OSD data of the aircraft and so on. Hope you enjoyed this tutorial, stay tuned for our next one!

