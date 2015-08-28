# MapView와 Waypoint 앱 만들기

<!-- toc -->

이 튜토리얼에서는 DJI PC Simulator를 설정하는 법을 배운고 Inspire 1, Phantom 3 Professional 그리고 Phantom 3의 펌웨어를 베타 버전으로 업데이트 한다. 그리고 DJI PC 시뮬레이터로 그라운드 스테이션 API를 테스트하는 방법을 배운다. DJI 그라운드스테이션의 Waypoint 기능을 사용하는 기본 절차를 경험한다. 자 이제 시작해보자.

데모 프로젝트를 다운받자 : <https://github.com/DJI-Mobile-SDK/Android-GSDemo-Part2-GoogleMap.git>

## DJI PC Simulator 사용하기

### 1. 소개

The DJI PC Simulator는 SDK 개발자를 위한 비행 simulator이다. simulator는 가상 3D 환경을 생성하고 UDP 프로토콜을 이용해서 데이터 분석 목적으로 비행 데이터를 PC로 전송한다.

**지원 운영체제**: Windows 7, Windows 8 그리고 Windows 8.1

**지원 DJI 플랫폼**: Matrice 100, Inspire 1, Phantom 3 Professional 그리고 Advanced

### 2. DJI PC 시뮬레이터 설치 및 설정

DJI PC Simulator 설치파일 및 WIN 드라이버 다운받기 : <http://dev.dji.com/en/products/sdk/onboard-sdk/downloads>
- DJI PC Simulator Installer & User Manual V1.0
- WIN Driver Installer

simulator를 설치하기 전에 WIN driver를 설치해야만 한다. Mac 사용자에 대해서는, simulator를 가상환경에서 simulator를 실행하는 것을 추천한다.(VMWare나 Parallels Desktop) DJI_WIN_Driver_Install.exe를 더블클릭해서 windows 드라이버를 설치하자. 화면에 **"Please power on MC and connect it to PC via USB!"** 메시지가 나오면 무시하고 "YES"를 클릭하고 화면의 지시에 따라 설치를 마치자.  

마지막으로 **DJISimulator-Installer.exe** 파일을 더블클릭하고 화면 명령을 따라 설치를 마치자.

### 3. DJI PC Simulator 사용하는 방법
**1.** DJI PC Simulator를 실행하면 Simulator Config 화면이 나타난다. 적당한 위경도 값을 home point로 설정한다. "SN"은 비행체의 시리얼 번호를 뜻한다.

![Config](../../images/Android/GSDemo/simulator_config.png)

---
**주의**: 

- home point가 No Fly Zone 바깥지점이여야 한다.

- "Log Settings" 태그아래 "Show Log Window"를 선택하면 비행 지점이 보인다.:

 ![showLog](../../images/Android/GSDemo/showLog.png)

---

**2**. Micro USB 케이블로 비행체를 PC에 연결하고 비행체와 리모트 컨트롤에 전원을 넣는다. Display Simulator를 클릭한다. 아래와 같은 화면을 볼 수 있다:

 ![display](../../images/Android/GSDemo/display.png)

---
**주의**: 

- DJI PC Simulator가 실행 중일 때는 DJI Pilot app을 실행하지 말라.

- DJI PC Simulator를 사용할 때는 프로펠러를 제거한다.

---

**3**. Start Simulator 버튼을 누르면 simulator 사용을 시작한다. 비행체의 코스를 변경하기 위해서 리모트 컨트롤러를 사용할 수 있다. 혹은 Return-to-Home 기능을 이용하면 돌아오게 할 수도 있다. 모바일이나 onboard 장치에서 제어하기 위해서 API Control를 사용할 수 있다. X, Y, Z는 북-남, 동-성, 위-아래 축을 뜻한다.(북, 동, 위 방향이 양의 방향이다)

**4**. 시야각을 변경하기 위해서 왼쪽 클릭을 누르면서 드래그한다. 줌인/아웃을 위해 스크롤한다.

 ![zoomIn](../../images/Android/GSDemo/zoomIn.png)
 
 ![zoomOut](../../images/Android/GSDemo/zoomout.png)

**5**. simulation을 멈추기 위해서 Stop Simulation을 클릭한다. 사용 후에는 simulator를 닫고 비행체와 리모트 컨트롤의 전원을 내린다.

**중요**: GroundStation Waypoint Mission에서 중간에 멈추기를 원한다면, 우선 **Stop Simulation** 을 눌러야 한다. 그렇지 않으면 다시 시작할 때, simulator는 이전 groundstation mission을 실행할지도 모른다. 혼란을 줄 수 있다.

DJI PC Simulator에 관한 추가 정보는 simulator와 함께 다운받은 **DJI PC Simulator user manual.pdf** 파일을 확인하자.---
**주의**: 

## 비행체 펌웨어 업그레이드

DJI Mobile SDK API 사용하기 전에, 비행체의 베타 버전으로 펌웨어를 다운받고 업그레이드한다.

다음 펌웨어 사이트에서 골라서 다운받도록 하자 : <http://dev.dji.com/en/products/sdk/onboard-sdk/downloads> :

- Phantom 3 Professional Firmware (P3X_FW_V01.01.1003.bin)
- Inspire 1 Firmware (WM610_FW_V01.02.01.02.bin)

리모트 컨트롤러의 펌웨어를 업데이트할 필요는 없다. SD카드에 **bin** 파일을 넣고 카메라에 있는 Micro USB 포트 삽입한다. 이어서 비행체를 재시작하면 업데이트 절차가 자동으로 시작된다. 마치는데 10 ~ 30분 정도 소요된다.

Verify the result via the beeping pattern from the gimbals or the blinking pattern of the camera status indicator. Refer to the table below for the beeping pattern:

- Upgrading: B B B B...
- Upgrade Success: B BB...
- Upgrade Failed: B...
- Critical Error: D D D...

Also, you can check the firmware upgrade status by checking the **txt** file generated during the upgrade process. For Phantom 3 Professional, the txt file is named as **"P3X_FW_RESULT_AB.txt"**, For Inspire 1, it's named as **"WM610_FW_RESULT_AB.txt"**, here are the example contents:

  ![upgradeP3XSuccess](../../images/Android/GSDemo/upgradeP3XSuccess.png)
  
  ![upgradeInspire1Success](../../images/Android/GSDemo/upgradeInspire1Success.png)
  
  
## Map View 설정하기

### 1. map view 생성

waypoint를 표시하기 위해 map view를 사용하고 비행 타스크를 실행할 때 비행체의 경로를 보여준다. 예제로 Google map을 사용한다.

Google의 공식 가이드 문서<https://developers.google.com/maps/documentation/android/config>를 참조해서 Google map을 Android app에 추가하자. 

(1) Android SDK 설치

**Android SDK Manager**를 통해 Android SDK를 설치한다. **Android SDK Manager**는 메뉴에서 "Window"를 클릭해서 진입할 수 있다. 다음으로 "Android SDK Manager"를 클릭한다.

(2) Google Play services SDK를 설치하기 위해 다음을 참조한다 : <https://developers.google.com/android/guides/setup>

**The Google Maps Android API**는 Google Play services SDK에 포함되어 있다. **Android SDK Manager**로 Google Play services SDK를 설치한다.
![installGooglePlayService](../../images/Android/GSDemo/installGooglePlayService.png)

Google Play services SDK를 설치 후, ADT 경로인 "xxx/\<android-sdk>/extras/google/google_play_services/libproject/google-play-services_lib" 아래에 library 프로젝트를 위치시킨다. library 프로젝트를 여러분의 Android app 프로젝트 위치에 복사한다.

library 프로젝트를 Eclipse workspace로 import시킨다. **File>Import**를 클릭하고 **Android>Existing Android Code into Workspace**를 선택한다. import시키기 위해서 library 프로젝트의 복사본에 접근할 수 있다.

Google Play services library는 **Package Explorer** 오른쪽 클릭으로 프로젝트를 참조한다. 다음으로 **Properties>Android**을 클릭한다. **google-play-services_lib**를 추가하기 위해서 **Add...**를 클릭한다.

![addGooglePlayService](../../images/Android/GSDemo/addGooglePlayService.png) 

Google Play services가 프로젝트를 프로젝트의 의존성을 위해 추가한다. **manifest** 파일을 열고 아래와 같이 ** \<application>** element의 자식으로 다음 태그를 추가한다 :

```xml
	<meta-data android:name="com.google.android.gms.version"
		android:value="@integer/google_play_services_version" />
```

이제 프로젝트를 개발하는데 Google Play services를 사용할 수 있게 되었다.

(3) API key를 얻는 것은 다음을 참조하자 : <https://developers.google.com/maps/documentation/android/signup>

1.App 인증 얻기

디버그 인증의 **SHA-1 fingerprint**를 얻거나 **keytool** 명령을 이용해서 인증을 릴리즈할 수 있다. 인증을 디버깅하기 위해서 **SHA-1 fingerprint**를 사용한다. Eclipse를 이용할 때, **Window>Preferences>Android>Build**를 선택해서 **SHA-1 fingerprint**를 얻을 수 있다.

![debugSHA1](../../images/Android/GSDemo/debugSHA1.png)

2.Google Developers Console에서 프로젝트를 등록하고 Google Maps Android API v2를 프로젝트의 service로 추가한다.

Google Developer console <https://console.developers.google.com>에 로그인한다. 다음으로 기존 프로젝트나 프로젝트를 선택한다. 사이드바에서 **APIs & auth**를 확장한다. **APIs**를 선택한다. API 섹션에 있는 **Enabled APIs** link를 선택해서 유효한 모든 API를 탐색가능하다. 유효한 API 리스트에서 Android API를 확인한다. 만약 선택가능한 상태가 아니라면 API 리스트에서 API를 선택하고 Android API를 사용가능하게 하기 위해서 Enable API 버튼을 선택한다. 여러분에게 필요한 API는 **Google Maps Android API**이다.

3.Android API key 생성

프로젝트를 클릭한다. 왼쪽 사이드바에서 **Credentials**를 선택한다. 프로젝트가 **Key for Android applications**를 이미 가지고 있지 않다면, **Create New Key**를 클릭하고 **Android key**를 클릭해서 API key를 생성한다. 결과창에서 app의 SHA-1 fingerprint를 입력하고 세미콜론(;) 다음으로 app의 package 이름을 입력한다. Google Developers Console은 **Key for Android applications**라는 제목의 섹션과 여러분의 API key를 출력한다.
![androidAPIKey](../../images/Android/GSDemo/androidAPIKey.png)

4.API key를 app에 추가히기

AndroidManifest.xml에서 다음 element를 \<application> element의 자식으로 추가한다. 

```xml
	<meta-data
		android:name="com.google.android.geo.API_KEY"
		android:value="API_KEY" />
```
 
여러분의 API key를 값 속성에 있는 API_KEY로 대체한다.

(4) 요청한 설정을 app의 manifest로 추가한다.

1.Android permission 지정 : 여러분의 app에서 필요로 하는 permission을 지정하기 위해서 **AndroidManifest.xml** 파일 내부에 \<manifest> element의 자식으로 **\<uses-permission>** elements를 추가한다. 

```xml
	<uses-permission android:name="android.permission.INTERNET"/>
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
	<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

permission의 상세한 내용을 <https://developers.google.com/maps/documentation/android/config>를 참조한다.

2. OpenGL ES version 2에 대한 요구사항

```xml
	<uses-feature android:glEsVersion="0x00020000" android:required="true" />
```

위에 설정을 완료하면, 이제 Google map을 로딩하기 위해서 다음과 같은 코드를 사용한다.

먼저 map을 로드할 app의 layout에 xml code를 추가한다. (데모에서는 **activity_gsdemo.xml** 파일)

```xml
	...
	<fragment
		android:id="@+id/map"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		class="com.google.android.gms.maps.SupportMapFragment" />
	...
```
 
Second, add the following codes in your Activity.

```java
	import android.os.Bundle;
	import android.support.v4.app.FragmentActivity;
	import com.google.android.gms.maps.CameraUpdateFactory;
	import com.google.android.gms.maps.GoogleMap;
	import com.google.android.gms.maps.OnMapReadyCallback;
	import com.google.android.gms.maps.SupportMapFragment;
	import com.google.android.gms.maps.model.LatLng;
	import com.google.android.gms.maps.model.MarkerOptions;
	
	...
	
	public class GSDemoActivity extends FragmentActivity implements OnMapClickListener, OnMapReadyCallback{
		private GoogleMap aMap;
		
		...
		
		@Override
		protected void onCreate(Bundle savedInstanceState){
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_gsdemo);
			SupportMapFragment mapFragment=(SupportMapFragment) getSupportFragmentManager().findFragmentById(R.id.map);
			mapFragment.getMapAsync(this);
		
		...
		
		}
		
		...
		
		@Override
		public void onMapReady(GoogleMap googleMap){
			if(aMap == null){
				aMap = googleMap;
				...
			}
			
			LatLng Shenzhen = new LatLng(22.5500, 114.1000);
			googleMap.addMarker(new MarkerOptions().position(Shenzhen).title("Marker in Shenzhen"));
			googleMap.moveCamera(CameraUpdateFactory.newLatLng(Shenzhen));
		}
		
		...
		
	}
```

**주의:** As class can only extend one parent class, hence, GSDemoActivity can not extend DemoBaseActivity as it has already extended FragmentActivity. Here we add **ServiceManager.getInstance().pauseService(false);** in GSDemoActivity's callback **onResume**, and add **ServiceManager.getInstance().pauseService(true);** in GSDemoActivity's callback **onPause**.

```java
	 @Override
    protected void onResume(){
        super.onResume();
        DJIDrone.getDjiMC().startUpdateTimer(1000); // Start the update timer for MC to update info
        ServiceManager.getInstance().pauseService(false);
    }
    
    @Override
    protected void onPause(){
        super.onPause();
        DJIDrone.getDjiMC().stopUpdateTimer(); // Stop the update timer for MC to update info
        ServiceManager.getInstance().pauseService(true);
    }
```

Now you can see your app loading a Google map by following the instructions to enable developer options on your Android device, connect your device to the computer through USB cable, build and run your app. The loading of the Google map will require your Android device having installed the Google Play service.
![loadGoogleMap](../../images/Android/GSDemo/loadGoogleMap.png)


 
### 2. Locate the Aircraft

When Google map is successfully loaded. You can then use a marker on the map to show the loacation of the aircraft based on the coordinates provided by **droneLocationLatitude**, **droneLocationLongitude** in **DJIMainControllerSystemState**. Implement **private void updateDroneLocation()** function to update the location of the aircraft.

Implement **public interface DJIMcuUpdateStateCallBack** interface to obtain the location of the aircraft.
We set up this interface in MainController module of DJI SDK using **public void setMcuUpdateStateCallBack(DJIMcuUpdateStateCallBack mCallBack)**. Then we update the marker that shows the aircraft's location on the map. 
Code example are listed as below:

```java
	// Update the drone location based on states from MCU.
    private void updateDroneLocation(){
        // Set the McuUpdateSateCallBack
        mMcuUpdateStateCallBack = new DJIMcuUpdateStateCallBack(){

            @Override
            public void onResult(DJIMainControllerSystemState state) {
                droneLocationLat = state.droneLocationLatitude;
                droneLocationLng = state.droneLocationLongitude;
                Log.e(TAG, "drone lat "+state.droneLocationLatitude);
                Log.e(TAG, "drone lat "+state.homeLocationLatitude);
                Log.e(TAG, "drone lat "+state.droneLocationLongitude);
                Log.e(TAG, "drone lat "+state.homeLocationLongitude);
            }     
        };
        Log.e(TAG,"setMcuUpdateState");
        DJIDrone.getDjiMC().setMcuUpdateStateCallBack(mMcuUpdateStateCallBack);
        
        // The following codes show the aircraft location on the map using a marker
        LatLng pos = new LatLng(droneLocationLat, droneLocationLng);
        //Create MarkerOptions object
        final MarkerOptions markerOptions = new MarkerOptions();
        markerOptions.position(pos);
        markerOptions.icon(BitmapDescriptorFactory.fromResource(R.drawable.aircraft));
        
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                if (droneMarker != null) {
                    droneMarker.remove();
                }
             
                droneMarker = aMap.addMarker(markerOptions);
            }
          });
    } 
```

We will call this function after initiating the SDK APIs and calling **DJIDrone.connectToDrone()**. Please note that when calling the SDK APIs for the aircraft's modules are only possible after the activation is completed. We call the above function after the **checkPermission** returns 0:

```java
	new Thread(){
            public void run() {
                try {
                    DJIDrone.checkPermission(getApplicationContext(), new DJIGerneralListener() {
                        
                        @Override
                        public void onGetPermissionResult(int result) {
                            // TODO Auto-generated method stub
                            if (result == 0) {
                                handler.sendMessage(handler.obtainMessage(SHOWDIALOG, DJIError.getCheckPermissionErrorDescription(result)));
                                updateDroneLocation(); // Obtain the drone's lat and lng from MCU.
                            } else {
                                handler.sendMessage(handler.obtainMessage(SHOWDIALOG, getString(R.string.demo_activation_error)+DJIError.getCheckPermissionErrorDescription(result)+"\n"+getString(R.string.demo_activation_error_code)+result));
                        
                            }
                        }
                    });
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }.start();
```

To update the Main Controller system state, we still need to call the **public boolean startUpdateTimer(int interval)** in the callback **onResume**. We can pause timer for updating by calling **public boolean stopUpdateTimer()** in the callback **onPause**.

```java
    @Override
    protected void onResume(){
        super.onResume();
        DJIDrone.getDjiMC().startUpdateTimer(1000); // Start the update timer for MC to update info
        ServiceManager.getInstance().pauseService(false);
    }
    
    @Override
    protected void onPause(){
        super.onPause();
        DJIDrone.getDjiMC().stopUpdateTimer(); // Stop the update timer for MC to update info
        ServiceManager.getInstance().pauseService(true);
    }
```

When we open the app, the function **updateDroneLocation()** will be called after the key activation.

Meanwhile, we implement a button "Locate" on UI. When users click the button, the function **updateDroneLocation()** will be called again.

To implement the button "Locate", first, add a Button view in the **activity_gsdemo.xml** 

```xml

	...
	
	<LinearLayout
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:orientation="horizontal">
		
		<Button
			android:id="@+id/locate"
			android:layout_width="match_parent"
			android:layout_height="wrap_content"
			android:text="Locate"
			android:layout_weight="1"/>
	
	...
	
	</LinearLayout>
	
	...
	
``` 
Second, add **OnClickListener** Interface in the activity(**GSDemoActivity** in the demo example). The activity implements the interface **public class GSDemoActivity extends ... implements OnClickListener ...**, and implements the code for the method **public void onClick(View v)**,

```
	@Override
	public void onClick(View v) {
		switch(v.getId()){
			case R.id.locate:{
				updateDroneLocation();
				cameraUpdate(); //
				break;
			}
			...
			default:
				break; 
		}	
		
	}	
	
```

Third, register the "Locate" button in the activity

```java
	private Button locate;
	
	...
	
	@Override
	protected void onCreate(Bundle savedInstanceState){
		...
		locate = (Button) findViewById(R.id.locate);
		...
		locate.setOnClickListener(this);
		... 
	}
```

Build and run the app to check the location function. Connect to aircraft to the computer through a USB cable and Launch the DJI PC simulator:

![simulatorLocateAircraft](../../images/Android/GSDemo/simulatorLocateAircraft.png)

Click the "locate" button, the following GUI prompt to show the location of the aircraft in the map.

![locateAircraft](../../images/Android/GSDemo/locateAircraft.png)

### 3. Add and clear the waypoints

It is critical to enable user to add waypoints in Ground Station intuitively. We will then demonstrate how to add waypoints on the map. Place a "Add" button. Whenever we click button "add", a switch variable "isAdd" will turn into "true", and the text of the button shown as "Exit". We have now entered waypoints adding mode: Click and add the desirable location in the map as a waypoint. A marker will be displayed on the map to shows that a new waypoint has been added. Click "Exit" button to exit waypoint adding mode when you have placed all the desirable waypoints in the map. The listener for clicking map is shown as follows:

```java
	 @Override
    public void onMapClick(LatLng point) {
    	if (isAdd == true){
    		markWaypoint(point);
    		DJIGroundStationWaypoint mDJIGroundStationWaypoint = new DJIGroundStationWaypoint(point.latitude, point.longitude);
    		mGroundStationTask.addWaypoint(mDJIGroundStationWaypoint);
    		//Add waypoints to Waypoint arraylist;
    	}else{
    		// Do not add waypoint;
    	}
    		
    }
```

When we click on the map, the codes within the **public void onMapClick(LatLng point)** will be executed. an instance of **DJIGroundStationWaypoint** will be created and added to **mGroundStationTask**.

![addWaypoints](../../images/Android/GSDemo/addWaypoints.png)

We add the "clear" button to clear all the added waypoints. All the markers on the map will be erased and all waypoints will be removed from **mGroundStationTask**.

```java
	 @Override
    public void onClick(View v) {
        // TODO Auto-generated method stub
        switch (v.getId()) {
			  
			  ...
            
            case R.id.clear:{
                runOnUiThread(new Runnable(){
                    @Override
                    public void run() {
                        aMap.clear();
                    }
                    
                });            
                mGroundStationTask.RemoveAllWaypoint(); // Remove all the waypoints added to the task
                break;
            }
			  
			  ...
			  
        }
    }

```

![clearWaypoints](../../images/Android/GSDemo/clearWaypoints.png)

## Configure the Navigation Task

As you see, the project's code structure was simple and not robust. In order to develop it further in this tutorial, it will need to be re-factored and we will need to add more UI elements. 

Here, as a simple example, we just show how to configure the waypoints. Users can developer their own codes to set up the waypoint sequentially. 

We first add a new button "Config". When users click the button, a new configuration dialog will be popped up. 

![configButton](../../images/Android/GSDemo/configButton.png)

We use a dialog to load the configuration setting view. The settings include the altitude of waypoints, whether to repeat the task, the horizontal speed of the aircraft during the task, the action after the task finished, the heading of the aircraft during the task (altitude and speed is a field of class **DJIGroundStationWaypoint**. Here we set all the waypoints' altitude or speed the same as a simple example. Users can also set different values for different waypoints' altitude or speed). We do not show the codes for the dialog here. When users click "Finish" after inputting a value or selecting an option for each item, the following function will be called to set the fields of **DJIGroundStationTask** and **DJIGroundStationWaypoint**:

```java
	    private void configGroundStationTask(){
        mGroundStationTask.isLoop = repeatGSTask;
        mGroundStationTask.finishAction=actionAfterFinishTask;
        mGroundStationTask.movingMode = heading;
        for (int i=0; i<mGroundStationTask.wayPointCount; i++){
            mGroundStationTask.getWaypointAtIndex(i).speed = speedGSTask;
            mGroundStationTask.getWaypointAtIndex(i).altitude = altitude;
        }
    }
```

## Upload the Task to the Aircraft

We have configured the **DJIGroundStationTask**. However, the task is currently stored on the mobile device. We need to upload the task to the aircraft before imitate the task. Click "Upload" button to upload the task to aircraft. The following function will be executed when "Upload" button is clicked:

```java
	   private void uploadGroundStationTask(){
        DJIDrone.getDjiGroundStation().openGroundStation(new DJIGroundStationExecuteCallBack(){

            @Override
            public void onResult(GroundStationResult result) {
                // TODO Auto-generated method stub
                String ResultsString = "return code =" + result.toString();
                handler.sendMessage(handler.obtainMessage(SHOWTOAST, ResultsString));
                
                if (result == GroundStationResult.GS_Result_Success) {
                    DJIDrone.getDjiGroundStation().uploadGroundStationTask(mGroundStationTask, new DJIGroundStationExecuteCallBack(){
    
                        @Override
                        public void onResult(GroundStationResult result) {
                            // TODO Auto-generated method stub
                            String ResultsString = "return code =" + result.toString();
                            handler.sendMessage(handler.obtainMessage(SHOWTOAST, ResultsString));
                        }
                        
                    });
                }
            }
            
        });
    }
```

We first need to call **public void openGroundStation(final DJIGroundStationExecuteCallBack mCallBack)** to initialized the Ground Station function, and when the Ground Station function is initialized successfully (the callback returns **GroundStationResult.GS_Result_Success**), then the function **public void uploadGroundStationTask(final DJIGroundStationTask task, final DJIGroundStationExecuteCallBack mCallBack)** will be executed.


**Important:** It is possible that you will come across **GroundStationResult.GS_Result_Rc_Control_Mode_Error** when using the Ground Station. The root cause is that the flight mode switch on the remote contorller is not set to the "F" position. You will then need to toggle the switch to "F" position and upload the waypoints agin before using the Ground Station.  

If the flight mode switch is at "F" position when the aircraft is powered on, the user must toggle back and forth between F and another position and then have an uploading of waypoints.

![switchFlightMode](../../images/Android/GSDemo/switchFlightMode.png)

When all waypoints are uploaded successfully , the DJI PC Simulator log will prompt "received mission length xx from app". 

![uploadwaypointsLog](../../images/Android/GSDemo/uploadwaypointsLog.png)

## Start and Stop the DJIGroundStationTask

After uploading the task to the aircraft, execute the **DJIGroundStationTask**. When the  "Start" button is clicked, the following codes will be executed, in which the function **public void startGroundStationTask(final DJIGroundStationExecuteCallBack mCallBack)** will be called.

```java
	    private void startGroundStationTask(){
        DJIDrone.getDjiGroundStation().startGroundStationTask(new DJIGroundStationExecuteCallBack(){

            @Override
            public void onResult(GroundStationResult result) {
                // TODO Auto-generated method stub
                String ResultsString = "return code =" + result.toString();
                handler.sendMessage(handler.obtainMessage(SHOWTOAST, ResultsString));
            }
        });
    }
```

![startGSTask](../../images/Android/GSDemo/startGSTask.png)

The DJIGroundStationTask can be stopped during execution by calling function **public void pauseGroundStationTask(final DJIGroundStationExecuteCallBack mCallBack)** to pause the task, after the pausing the task, use **public void closeGroundStation(final DJIGroundStationExecuteCallBack mCallBack)** to close the Ground Station function.

```java
	    private void stopGroundStationTask(){
        DJIDrone.getDjiGroundStation().pauseGroundStationTask(new DJIGroundStationExecuteCallBack(){

            @Override
            public void onResult(GroundStationResult result) {
                // TODO Auto-generated method stub
                String ResultsString = "return code =" + result.toString();
                handler.sendMessage(handler.obtainMessage(SHOWTOAST, ResultsString));
                
                DJIDrone.getDjiGroundStation().closeGroundStation(new DJIGroundStationExecuteCallBack(){

                    @Override
                    public void onResult(GroundStationResult result) {
                        // TODO Auto-generated method stub
                        String ResultsString = "return code =" + result.toString();
                        handler.sendMessage(handler.obtainMessage(SHOWTOAST, ResultsString));
                    }

                });
            }
        });
        mGroundStationTask.RemoveAllWaypoint();
    }
``` 

## Summary

In this tutorial, You have mastered how to setup and use the DJI PC Simulator to test your Ground Station app, how to upgrade your  firmware to the developer version, and how to use the DJI Mobile SDK to create a simple MapView, modify annotations of map view, checking the aircraft on the map view by simulate the GPS data from DJI PC Simulator, etc. That covers much of the ground. 

Moreover, you have learned how to configure both **DJIGroundStationWaypoint** and **DJIGroundStationTask**. Moreover, you have now mastered how to manipulate waypoints and task by using **DJIGroundStationTask** and **DJIInspireGroundStation**.

Congratulations! Now that you have finished the demo project, you can build on what you've learned and start to build your own ground station application. You can improve the way waypoints that are added(such as drawing a line on the map and generating waypoints automatically), play around with the properties of a waypoint (such as heading, horizontal speed, etc.), and adding more functionality. In order to make a cool ground station application, you still have a long way to go. Good luck and hope you that enjoy this.


