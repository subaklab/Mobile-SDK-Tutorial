# MapView와 Waypoint App 만들기

<!-- toc -->

*주의: 이 튜토리얼의 코드는 iPad에서 실행되도록 작성하였다. 따라서 iPad나 iPad simulator에서 이 코드를 실행하시오.*

이 튜토리얼엣어 DJI PC Simulator를 설정하는 방법을 배우고 베타 버전으로 Inspire 1, Phantom 3 Professional과 Phantom 3 Advanced 펌웨어를 업그레이드해서 DJI PC Simulator와 GroundStation API를 테스트하는 방법을 배워보자. DJI GroundStation Waypoint 기능을 이용하는 것에 익숙해 질 것이다. 자 이제 시작해보자!

   이 튜토리얼의 전체 프로젝트를 다운받을 수 있다 : <https://github.com/DJI-Mobile-SDK/iOS-GSDemo.git>

## DJI PC Simulator 사용하기

### 1. 소개

DJI PC Simulator는 SDK 개발자를 위해 설계된 비행 simulator이다. simulator는 가상 3D 환경을 만들고 UDP 프로토콜을 통해 PC로 전달된 비행 데이터를 분석을 제공한다.

**지원하는 운영체제**: Windows 7, Windows 8 그리고 Windows 8.1

**지원하는 DJI 플랫폼**: Matrice 100, Inspire 1, Phantom 3 Professional, 그리고 Phantom 3 Advanced

### 2. DJI PC Simulator 설치 및 설정

먼저 DJI PC Simulator와 WIN Driver 다운받는다 : <http://dev.dji.com/en/products/sdk/onboard-sdk/downloads> :

- DJI PC Simulator Installer & User Manual V1.0
- WIN Driver Installer

simulator를 실행하기 전에 driver 설치해야만 한다. simulator는 윈도우만 지원하기 때문에 Mac에서 윈도우를 실행할려면 가상 머신(VMWare나 Parallels Desktop)을 설치해야 한다. **DJI_WIN_Driver_Installer.exe** 을 설치하기 위해서 더블 클릭한다. 다이얼로그 박스에 **"Please power on MC and connect it to PC via USB!"** 나오면 무시하고 그냥 **"YES"**를 클릭한다. 다음으로 driver를 설치하기 위해 다음과 같은 절차를 따른다.

다음으로 **DJISimulator-Installer.exe**를 더블 클릭해서 설치하고 설치를 위해 다음 절차를 따른다.

### 3. DJI PC Simulator 사용하는 방법

**1**. DJI PC Simulator를 실행하면 **Simulator Config** 창이 한 번 나타난다. 원하는 값으로 **Latitude** 와 **Longitude** 값을 설정한다. **SN**은 연결한 비행체의 시리얼 넘버를 뜻한다. **SN**을 찾지 못했다면, 여러분의 비행체가 PC에 제대로 연결이 안되었거나 펌웨어와 문제가 있을 수 있다.

 ![Config](../../images/iOS/GSDemo/simulator_config.png)

---
**주의**: 

- 비행금지 구역([No Fly Zone](http://flysafe.dji.com/no-fly))에 가까운 값으로 비행체의 고도와 위도를 설정하면 이륙하지 않는다.

- Log Settings 탭 아래 **show log window**를 선택하여 log 창을 볼 수 있다 :

 ![showLog](../../images/iOS/GSDemo/showLog.png)

---

**2**. 마이크로 USB 케이블을 통해 비행체를 PC로 연결하고 리모트 컨트롤러을 켜고 마지막으로 비행체를 켠다. **Display Simulator**를 클릭하고 스크린샷이 보인다.(사용하고 있는 비행체에 관계 없이 보인다) 아래 경우는 Inspire 1인 경우 :

 ![display](../../images/iOS/GSDemo/display.png)

---
**주의**: 

- DJI PC Simulator가 실행되고 있을 때, DJI Pilot app을 실행하지 마라.

- DJI PC Simulator가 실행 중일 때, 비행체에 프로펠러를 붙착하지 말라. 사고로 모터가 동작할 수 있기 때문이다. 이는 매우 위험할 수 있다.

---

**3**. **Start Simulation**를 클릭하면 시뮬레이션이 시작된다. control mode를 **P**로 변경하면 리모트 컨트롤러를 이용해서 비행체의 경로를 변경할 수 있다. 혹은 리모트 컨트롤러의 **Return-to-Home** 버튼을 누르면 되돌아오게 할 수 있다.

왼쪽 아래 모서리에 있는 데이터를 확인하라. X, Y, Z는 북남(North-South), 동서(East-West), 위아래(Up-Down)축을 뜻한다.(북, 동, 위 방향이 양수 값이다.)

**4**. view angle을 변경하기 위해서 왼쪽 클릭과 드래, 스크롤을 위해 zoom in/out

 ![zoomIn](../../images/iOS/GSDemo/zoomIn.png)
 
 ![zoomOut](../../images/iOS/GSDemo/zoomout.png)

**5**. 시뮬레이션을 중지할려면 **Stop Simulation**을 클릭한다. simulator를 듣고 비행체 전원을 끈다. 사용이 끝났으면 리모트 컨트롤러의 전원을 내린다.

**중요**: GroundStation Waypoint Mission을 통해 midway를 멈추고자 한다면, **Stop Simulation**을 클릭하고 실행하고 있는 장치의 mission을 중지시킨다. 이렇게 하지 않으면 simulator를 다시 시작할 때, 이전 groundstation mission을 실행할 수도 있다. 

DJI PC Simulator에 대한 추가 정보는 **DJI PC Simulator user manual.pdf** 파일을 참고하라. 이 파일은 다운받은 simulator에 포함되어 있다.

## 비행체 펌웨어 업그레이드

다음 단계를 진행하기 전에, 여러분의 비행체의 펌웨어가 DJI Mobile SDK를 지원하는지 확인해야만 한다. 여러분의 가지고 있는 비행체에 맞는 베타 버전 펌웨어를 다운받도록 하자: <http://dev.dji.com/en/products/sdk/onboard-sdk/downloads> :

- Phantom 3 Professional Firmware
- Phantom 3 Advanced Firmware
- Inspire 1 Firmware

리모트 컨트롤러의 펌웨어를 업그레이드할 필요는 없다. SD카드에 다운받은 **bin** 파일을 넣은 후에 비행체 카메라에 삽입하고 다시시작하면 업그레이드 된다. 업그레이드를 마치는데 10 ~ 30분 가량 소요된다.

Inspire 1의 경우 비행체의 소리 패턴에 따라 펌웨어의 업그레이드 상태를 검사할 수 있다:

- Upgrading: D D D D ... (fast) 
- Upgrade Success: D DD ...
- Upgrade Failed or Error: long beep sound

Phantom 3의 경우 카메라 상태 표시등을 보면 펌웨어 업그레이드의 상태를 검사할 수 있다:

- Upgrading: 카메라 상태 표시등이 녹색/빨간색으로 깜빡거린다.
- Upgrade Success: 카메라 상태 표시등이 깜빡거림을 멈춘다.
- Upgrade Failed or Error: 카메라 상태 표시등이 강한 빨간색을 표시한다.

비행체와 무관하게 펌웨어 업그레이드를 검사하는 방법은 업그레이드 과정에서 생성하는 **txt** 파일을 통해서 가능하다. Phantom 3 Professional의 경우 txt 파일 이름은 **"P3X_FW_RESULT_AB.txt"**이고 Inspire 1은 **"WM610_FW_RESULT_AB.txt"** 이다. **txt** 파일 내부 내용은 아래와 같다 :

  ![upgradeP3XSuccess](../../images/iOS/GSDemo/upgradeP3XSuccess.png)
  
  ![upgradeInspire1Success](../../images/iOS/GSDemo/upgradeInspire1Success.png)

## Map View 설정

### 1. 프레임워크와 라이브러리 Import하기

위에 과정을 마치고 나면, application을 시작할 수 있다. 이전 튜토리얼([**Creating a Camera Application**](../../iOS/FPVDemo/FPVDemo_en.md))에서 DJI Mobile SDK를 Xcode 프로젝트로 import하고 활성화 시키는지 배웠다. 이전 자료를 보지 못했다면, 한 번 살펴보도록 하자. 이런 과정을 다 수행했다면 map view를 설정을 진행한다. 

**1**.  Xcode에 새로운 프로젝트를 생성하고 "**GSDemo**"라고 이름 붙인다. **DJISDK.framework**를 Xcode 프로젝트 폴더에 복사한다. 다음으로 project target을 선택하고 Build Phases -> Link Binary With Libraries로 이동한다. 밑에 있는 "+" 버튼을 클릭하고 2개 라이브러리를 프로젝트에 추가한다.(libstdc++.6.0.9.dylib 과 libz.dylib) 스크린샷은 아래와 같다:

  ![framework](../../images/iOS/GSDemo/framework.png)

### 2. Map View 생성
**ViewController.h**와 **ViewController.m** 파일을 삭제하자. 프로젝트를 생성할 때, Xcode에서 자동으로 생성한 것이다. "**DJIRootViewController**"라는 viewController를 생성하고 Main.storyboard에 **Root View Controller**로 설정한다. Object Library에서 **MKMapView**를 **DJIRootViewController**로 드래그한다. 여기서 AutoLayout constraints를 설정하고 delegate로 **DJIRootViewController**를 지정한다. 아래와 같다:
   
![mkMapView](../../images/iOS/GSDemo/mkMapView.png)

이후, **DJIRootViewController.h** 파일을 열고 MKMapView을 위해 IBOutlet를 생성하고 "**mapView**"라고 이름 붙이고 **Main.storyboard**내부에 MKMapView에 링크를 걸어준다. 아래 header 파일을 import하고 MKMapView의 delegate 메소드를 구현한다:

~~~objc
#import <DJISDK/DJISDK.h>
#import <MapKit/MapKit.h>

@interface DJIRootViewController : UIViewController<MKMapViewDelegate>

@property (weak, nonatomic) IBOutlet MKMapView *mapView;

@end
~~~

자 이제, 프로젝트를 빌드하고 실행해보자. 만약 제대로 잘 따라왔다면 아래와 같은 화면을 보게 된다:

![mapView](../../images/iOS/GSDemo/mapView.png)

### 3. MapView에 Annotation 추가하기

현재까지 map view는 단순했다. 재밌는 것을 추가해 보도록 하자. **DJIMapController**이라는 새로운 **NSObject** 파일을 만들자. 이것으로 지도 상에서 logic을 MKAnnotation으로 처리하는데 사용된다. **DJIMapController.h** 파일을 열고 다음과 같은 코드를 추가한다 :

~~~objc
#import <UIKit/UIKit.h>
#import <MapKit/MapKit.h>

@interface DJIMapController : NSObject

@property (strong, nonatomic) NSMutableArray *editPoints;

/**
 *  Add Waypoints in Map View
 */
- (void)addPoint:(CGPoint)point withMapView:(MKMapView *)mapView;

/**
 *  Clean All Waypoints in Map View
 */
- (void)cleanAllPointsWithMapView:(MKMapView *)mapView;

/**
 *  Current Edit Points
 *
 *  @return Return an NSArray contains multiple CCLocation objects
 */
- (NSArray *)wayPoints;

@end
~~~

여기서 waypoint 객체를 저장하기 위해서 **editPoints**라는 NSMutableArray를 생성한다. **Add** 와 **Remove** waypoints를 구현하기 위해서 2개 메소드를 추가한다. 마지막 메소드는 array내에서 지도상에 있는 현재 waypoint 객체를 반환하는데 사용할 것이다.

**DJIMapController.m** 파일로 가서 원래 코드를 다음 코드로 교체하자:

~~~objc
#import "DJIMapController.h"

@implementation DJIMapController

- (instancetype)init
{
    if (self = [super init]) {
        self.editPoints = [[NSMutableArray alloc] init];
    }
    return self;
}

- (void)addPoint:(CGPoint)point withMapView:(MKMapView *)mapView
{
    CLLocationCoordinate2D coordinate = [mapView convertPoint:point toCoordinateFromView:mapView];
    CLLocation *location = [[CLLocation alloc] initWithLatitude:coordinate.latitude longitude:coordinate.longitude];
    [_editPoints addObject:location];
    MKPointAnnotation* annotation = [[MKPointAnnotation alloc] init];
    annotation.coordinate = location.coordinate;
    [mapView addAnnotation:annotation];
}

- (void)cleanAllPointsWithMapView:(MKMapView *)mapView
{
    [_editPoints removeAllObjects];
    NSArray* annos = [NSArray arrayWithArray:mapView.annotations];
    for (int i = 0; i < annos.count; i++) {
        id<MKAnnotation> ann = [annos objectAtIndex:i];
        [mapView removeAnnotation:ann];
    }   
}

- (NSArray *)wayPoints
{
    return self.editPoints;
}

@end
~~~
먼저, **init** 메소드에서 **editPoints** array를 초기화한다.그런 다음 **CGPoint**로부터 **MKPointAnnotation** 객체를 생성하고 **mapView**에 추가한다. 마지막으로 **cleanAllPointsWithMapView**을 구현해서 **eidtPoints** array와 mapview에 있는 annotation을 정리(cleanup)한다.

**DJIRootViewController.h** 파일로 돌아가서, DJIMapController.h 헤더 파일을 import하고 **mapController**이름의 **DJIMapController** property를 생성한다.map을 두드려서 annotation 핀을 추가하려고 하므로 "**tapGesture**"라는 이름의 **UITapGestureRecognizer**를 생성한다. 마지막으로 **UIButton**를 **Main.storyboard**내부에 있는 **DJIRootViewController** scene에 추가한다. "**editBtn**"라는 이름으로 IBOutlet을 설정한다. "**editBtnAction**" 이름의 IBAction 메소드를 추가한다. 아래와 같다:

~~~objc
@property (nonatomic, strong) DJIMapController *mapController;
@property (nonatomic, strong) UITapGestureRecognizer *tapGesture;
@property (weak, nonatomic) IBOutlet UIButton *editBtn;

- (IBAction)editBtnAction:(id)sender;
~~~

![editButton](../../images/iOS/GSDemo/editButton.png)

일단 완료하면, **DJIMapController.m** 파일을 열고  **mapController** 와 **tapGesture** 변수를 초기화한다. **tapGesture**를 mapView에 추가해서 waypoint를 추가할 수 있다. 추가로 "**isEditingPoints**"라는 boolean 변수가 필요하다. 수정한 waypoint 상태를 저장하는 용도이며, **editBtn**의 타이틀을 변경할 것이다. 마지막으로 tapGesture의 action 메소드 **addWayPoints**를 구현한다. 아래와 같다 :

~~~objc
#import "DJIRootViewController.h"

@interface DJIRootViewController ()
@property (nonatomic, assign)BOOL isEditingPoints;
@end

@implementation DJIRootViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    
    self.mapController = [[DJIMapController alloc] init];
    self.tapGesture = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(addWaypoints:)];
    [self.mapView addGestureRecognizer:self.tapGesture];
    
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(registerAppSuccess:) name:@"RegisterAppSuccess" object:nil];
}

#pragma mark Custom Methods

- (void)registerAppSuccess:(NSNotification *)notification
{

}

- (void)addWaypoints:(UITapGestureRecognizer *)tapGesture
{
    CGPoint point = [tapGesture locationInView:self.mapView];
    
    if(tapGesture.state == UIGestureRecognizerStateEnded){

        if (self.isEditingPoints) {
            [self.mapController addPoint:point withMapView:self.mapView];
        }
    }
}

- (IBAction)editBtnAction:(id)sender {

    if (self.isEditingPoints) {
        [self.mapController cleanAllPointsWithMapView:self.mapView];
        [self.editBtn setTitle:@"Edit" forState:UIControlStateNormal];
    }else
    {
        [self.editBtn setTitle:@"Reset" forState:UIControlStateNormal];
    }
    
    self.isEditingPoints = !self.isEditingPoints;
    
}

#pragma mark MKMapViewDelegate Method
- (MKAnnotationView *)mapView:(MKMapView *)mapView viewForAnnotation:(id <MKAnnotation>)annotation
{
    if ([annotation isKindOfClass:[MKPointAnnotation class]]) {
        MKPinAnnotationView* pinView = [[MKPinAnnotationView alloc] initWithAnnotation:annotation reuseIdentifier:@"Pin_Annotation"];
        pinView.pinColor = MKPinAnnotationColorPurple;
        return pinView;
        
    }
    
    return nil;
}

~~~

위 코드에서 NSNotification observer를 추가해서 DJI Mobile SDK 상태를 검사해서 성공적으로 등록되었는지 확인한다. 동시에 DJIMapController를 호출해서 **addWaypoints** gesture action을 구현한다. 

     - (void)addPoint:(CGPoint)point withMapView:(MKMapView *)mapView
map에 waypoint를 추가하는 메소드. 다음으로 IBAction 메소인 **editBtn**를 구현하여 button의 title을 업데이트하고 **isEditingPoints** 기반으로 waypoint를 지운다. 마지막으로 pin색상을 보라색으로 바꾸기 위해서 MKMapViewDelegate의 메소드를 구현한다.

위에 있는 모든 단계를 마쳤으면 프로젝트를 빌드하고 실행해서 waypoint를 map에 추가해 보자. 모든 것이 정상이라면 다음과 같이 보일 것이다 :

![addWaypoint](../../images/iOS/GSDemo/addWaypoint.gif)


### 4. Focus The MKMapView

지도상의 위치가 현재 여러분의 위치와 차이가 나는지, 지도상에서 여러분의 위치를 찾는게 왜 어려운지 궁금할 것이다. 현재 위치를 지도상에 빠르게 포커싱하는 것은 app에 도움이 된다. 이런 특징을 구현하기 위해서 **CLLocationManager**를 사용해야 한다.

**DJIRootViewController.h** 파일을 열고 CoreLocation의 헤더 파일을 import시킨다. **CLLocationManager** 속성에 "**locationManager**" 이름을 붙인다. **CLLocationCoordinate2D** 속성에는 "**userLocation**"라고 이름 붙이며 사용자의 위치 데이터를 저장한다. 다음으로 아래와 같이 **CLLocationManager**의 **CLLocationManagerDelegate** 프로토콜을 구현한다. 

~~~objc
#import <DJISDK/DJISDK.h>
#import <MapKit/MapKit.h>
#import <CoreLocation/CoreLocation.h>

@interface DJIRootViewController : UIViewController<MKMapViewDelegate, CLLocationManagerDelegate>

@property (nonatomic, weak) IBOutlet MKMapView *mapView;
@property (nonatomic, strong) CLLocationManager* locationManager;
@property (nonatomic, assign) CLLocationCoordinate2D userLocation;
@property (nonatomic, strong) UITapGestureRecognizer *tapGesture;
@property (nonatomic, weak) IBOutlet UIButton *editBtn;

- (IBAction)editBtnAction:(id)sender;
- (IBAction)focusMapAction:(id)sender;

@end
~~~

위 코드와 같이, **Main.storyboard**에서 DJIRootViewController의 "**Focus Map**" 이름의 UIButton을 추가하고 "**focusMapAction**"라는 IBAction 메소드를 추가한다. **Main.storyboard**의 스크린샷은 아래와 같다:

![focusMap](../../images/iOS/GSDemo/focusMap.png)

일단 완료하면 **DJIRootViewController.m** 파일로 가서 다음 코드를 추가한다:

~~~objc
- (void)viewWillAppear:(BOOL)animated
{
    [super viewWillAppear:animated];
    [self startUpdateLocation];
}

- (void)viewWillDisappear:(BOOL)animated
{
    [super viewWillDisappear:animated];
    [self.locationManager stopUpdatingLocation];
}

- (void)viewDidLoad {
    [super viewDidLoad];
    
    self.userLocation = kCLLocationCoordinate2DInvalid;
    
    self.mapController = [[DJIMapController alloc] init];
    self.tapGesture = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(addWaypoints:)];
    [self.mapView addGestureRecognizer:self.tapGesture];
    
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(registerAppSuccess:) name:@"RegisterAppSuccess" object:nil];
    
}

#pragma mark CLLocation Methods
-(void) startUpdateLocation
{
    if ([CLLocationManager locationServicesEnabled]) {
        if (self.locationManager == nil) {
            self.locationManager = [[CLLocationManager alloc] init];
            self.locationManager.delegate = self;
            self.locationManager.desiredAccuracy = kCLLocationAccuracyBest;
            self.locationManager.distanceFilter = 0.1;
            if ([self.locationManager respondsToSelector:@selector(requestAlwaysAuthorization)]) {
                [self.locationManager requestAlwaysAuthorization];
            }
            [self.locationManager startUpdatingLocation];
        }
    }else
    {
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Location Service is not available" message:@"" delegate:self cancelButtonTitle:@"OK" otherButtonTitles:nil];
        [alert show];
    }
}

- (IBAction)focusMapAction:(id)sender {
{
    if (CLLocationCoordinate2DIsValid(self.userLocation)) {
        MKCoordinateRegion region = {0};
        region.center = self.userLocation;
        region.span.latitudeDelta = 0.001;
        region.span.longitudeDelta = 0.001;
        
        [self.mapView setRegion:region animated:YES];
    }
}

#pragma mark - CLLocationManagerDelegate
- (void)locationManager:(CLLocationManager *)manager didUpdateLocations:(NSArray *)locations
{
    CLLocation* location = [locations lastObject];
    self.userLocation = location.coordinate;
}

~~~

먼저 **userLocation** 데이터 viewDidLoad 메소드에 있는 kCLLocationCoordinate2DInvalid로 초기화한다. **locationManger**를 추가하기 위해, **startUpdateLocation** 이름의 새로운 메소드를 추가한다. 속성을 설정하고 위치를 업데이트한다. 만약 Location Service가 불가능하면 알림을 위해 UIAlertView를 추가한다. **startUpdateLocation**는 **viewWillAppear** 메소드에서 호출되고 **viewWillDisappear**에서 멈추게 된다. **userLocation** 속성을 업데이트 하기 위해서는 CLLocationManagerDelegate 메소드를 구현해야한다. 마지막으로 **mapView**에 사용자의 현재 위치를 포커스 시키기 위해 **focusMapAction** 메소드를 구현한다.

iOS8에서 먼저 **locationManager**의 **requestAlwaysAuthorization**를 호출해야만 한다. 이는 **startUpdateLocation**에서 실행된다. 다음으로 UIAlert이 app에서 위치를 사용하는 것을 허용할지 말지를 물어볼 때, NSLocationAlwaysUsageDescription나 NSLocationWhenInUseUsageDescription key를 여러분의 프로젝트에 있는 Info.plist에 추가한다. 메시지를 비워도록 설정한다 :

![infoPlist](../../images/iOS/GSDemo/infoPlist.png)

포커스 지도 기능을 검사하기 위해 프로젝트를 빌드하고 실행하는 때가 되었다. 처음으로 app을 실행할 때, 여러분의 위치에 접근을 허용하는 것을 물어보는 팝업이 나타난다. **Allow**를 선택하고 **Focus Map** 버튼을 누른다. 만약 아래 애니메이션과 같이 map view가 여러분의 현재 위치로 움직이는 화면이 나타나면 성공이다. 지금까지 **Focus Map** 기능을 마쳤다.

![focusMap](../../images/iOS/GSDemo/focusMap.gif)


### 5. Map View에 Aircraft 보여주기

이제 현재 위치에 mapView를 포커스할 수 있게 되었다. 괜찮은 출발이다. 하지만 좀더 흥미로운 것을 해보자. DJI PC Simulator를 이용해서 비행체의 GPS 위치를 시뮬레이션 해볼 것이다. 그리고 이를 map view로 보여주자.

여러분은 DJI PC Simulator를 설정하는 방법은 기본이라 이미 알고 있을 것이다. 만약 비행체를 Map View에 현재 GPS 위치로 옮기려면, **Simulator Config**에 위도, 경도 값을 설정한다. 다음 예제에서 시뮬레이터의 초기 값을 가져올 수 있다. 

코드로 돌아와서, "DJIAircraftAnnotationView" 이름을 가진 **MKAnnotationView**의 서브클래스를 만들고, **DJIAircraftAnnotation** 이름의 **NSObject** 서브 클래스를 만든다. 아래와 같다 :

- DJIAircraftAnnotationView.h

~~~objc
#import <MapKit/MapKit.h>

@interface DJIAircraftAnnotationView : MKAnnotationView

-(void) updateHeading:(float)heading;

@end
~~~

- DJIAircraftAnnotationView.m

~~~objc
#import "DJIAircraftAnnotationView.h"

@implementation DJIAircraftAnnotationView

- (instancetype)initWithAnnotation:(id <MKAnnotation>)annotation reuseIdentifier:(NSString *)reuseIdentifier
{
    self = [super initWithAnnotation:annotation reuseIdentifier:reuseIdentifier];
    if (self) {
        self.enabled = NO;
        self.draggable = NO;
        self.image = [UIImage imageNamed:@"aircraft.png"];
    }
    
    return self;
}

-(void) updateHeading:(float)heading
{
    self.transform = CGAffineTransformIdentity;
    self.transform = CGAffineTransformMakeRotation(heading);
}

@end

~~~

위 코드에서 비행체를 위해 MKAnnotationView 생성하고 비행체의 rotation을 변경하기 위해 **updateHeading**라는 메소드를 추가하고 이미지를 "**aircraft.png**"(튜토리얼의 데모 프로젝트에서 이미지 가져온다)로 설정한다. DJIAircraftAnnotationView의 draggable 속성은 비활성화 시킨다. 아래 코드를 보자 :

- DJIAircraftAnnotation.h

~~~objc
#import <MapKit/MapKit.h>
#import "DJIAircraftAnnotationView.h"

@interface DJIAircraftAnnotation : NSObject<MKAnnotation>

@property(nonatomic, readonly) CLLocationCoordinate2D coordinate;
@property(nonatomic, weak) DJIAircraftAnnotationView* annotationView;

-(id) initWithCoordiante:(CLLocationCoordinate2D)coordinate;

-(void)setCoordinate:(CLLocationCoordinate2D)newCoordinate;

-(void) updateHeading:(float)heading;

@end
~~~

- DJIAircraftAnnotation.m

~~~objc
#import "DJIAircraftAnnotation.h"

@implementation DJIAircraftAnnotation

-(id) initWithCoordiante:(CLLocationCoordinate2D)coordinate
{
    self = [super init];
    if (self) {
        _coordinate = coordinate;
    }   
    return self;
}

- (void)setCoordinate:(CLLocationCoordinate2D)newCoordinate
{
    _coordinate = newCoordinate;
}

-(void)updateHeading:(float)heading
{
    if (self.annotationView) {
        [self.annotationView updateHeading:heading];
    }
}
@end
~~~

**DJIAircraftAnnotation** 클래스는  **MKAnnotation** 프로토콜을 구현한다. CLLocationCoordinate2D 속성을 저장하고 업데이트한다. **updateHeading** 메소드와 함께 DJIAircraftAnnotationView의 앞부분을 업데이트한다. 

일단 이것이 완료되면, **DJIMapController.h** 파일을 열고 **DJIAircraftAnnotation.h** 파일을 import한다:

~~~objc
#import "DJIAircraftAnnotation.h"
~~~

DJIAircraftAnnotation 인스턴스의 속성을 생성하고 **aircraftAnnotation**라고 이름 붙인다. 

~~~objc
@property (nonatomic, strong) DJIAircraftAnnotation* aircraftAnnotation;
~~~

2개 메소드를 추가해서 비행체의 위치와 heading을 업데이트한다.

~~~objc
/**
 *  Update Aircraft's location in Map View
 */
-(void)updateAircraftLocation:(CLLocationCoordinate2D)location withMapView:(MKMapView *)mapView;

/**
 *  Update Aircraft's heading in Map View
 */
-(void)updateAircraftHeading:(float)heading;
~~~

다음으로 **DJIMapController.m** 파일로 돌아가서 금방 추가한 2개 메소드를 구현한다:

~~~objc
-(void)updateAircraftLocation:(CLLocationCoordinate2D)location withMapView:(MKMapView *)mapView
{
    if (self.aircraftAnnotation == nil) {
        self.aircraftAnnotation = [[DJIAircraftAnnotation alloc] initWithCoordiante:location];
        [mapView addAnnotation:self.aircraftAnnotation];
    }
    
    [self.aircraftAnnotation setCoordinate:location];
}

-(void)updateAircraftHeading:(float)heading
{
    if (self.aircraftAnnotation) {
        [self.aircraftAnnotation updateHeading:heading];
    }
}
~~~

DJIMapController.m 파일에 **cleanAllPointsWithMapView** 메소드에서 제거한 **aircraftAnnotation**가 필요없으므로 아래와 같이 수정해야 한다 :

~~~objc
- (void)cleanAllPointsWithMapView:(MKMapView *)mapView
{
    [_editPoints removeAllObjects];
    NSArray* annos = [NSArray arrayWithArray:mapView.annotations];
    for (int i = 0; i < annos.count; i++) {
        id<MKAnnotation> ann = [annos objectAtIndex:i];
        if (![ann isEqual:self.aircraftAnnotation]) {
            [mapView removeAnnotation:ann];
        }
       
    }   
}
~~~
map view의 annotation이 **aircraftAnnotation** 속성과 같은지를 확인하기 위해 if 절을 추가한다. 그리고 그렇지 않으면 이를 제거한다. 이렇게 함으로써 비행체의 annotation이 삭제되는 것을 막을 수 있다.

더 나은 UX를 제공하기 위해서, 비행체의 비행 모드 타입, 현재 GPS 위성 카운트, 수직/수평 비행 속도, 비행 고도 등을 보여주기 위해서 mapView의 윗부분에 status view를 추가한다. **Main.storyboard**에 RootViewController UI를 추가하자. 아래와 같다 :

![statusView](../../images/iOS/GSDemo/statusView.png)

여기까지 하고 나면 **DJIRootViewController.h** 파일을 열고 위에 UI element를 위해 IBOutlet를 생성하고 DJISDK 헤더 파일을 import하고 **DJIDroneDelegate** 와 **DJIMainControllerDelegate** protocol을 구현한다. 여기서 데모로 Inspire 1을 이용하고 2개 속성을 생성하는데 하나는 **DJIDrone**과 **DJIInspireMainController** 속성이다. **droneLocation** 이름의 **CLLocationCoordinate2D** 속성을 생성해서 비행체의 위치를 기록한다. 아래와 같다 :

~~~objc
#import <DJISDK/DJISDK.h>
@interface DJIRootViewController : UIViewController<MKMapViewDelegate, CLLocationManagerDelegate, DJIDroneDelegate, DJIMainControllerDelegate>

@property(nonatomic, strong) IBOutlet UILabel* modeLabel;
@property(nonatomic, strong) IBOutlet UILabel* gpsLabel;
@property(nonatomic, strong) IBOutlet UILabel* hsLabel;
@property(nonatomic, strong) IBOutlet UILabel* vsLabel;
@property(nonatomic, strong) IBOutlet UILabel* altitudeLabel;

@property(nonatomic, strong) DJIDrone* inspireDrone;
@property(nonatomic, strong) DJIInspireMainController* inspireMainController;
@property(nonatomic, assign) CLLocationCoordinate2D droneLocation;
~~~

이제 **DJIRootViewController.m**로 이동해서 **initUI** 메소드내에서 UI Element 값을 초기화한다. 또 **inspireDrone**에 대한 속성을 초기화하기 위해서 **initDrone** 메소드를 추가한다. **viewDidLoad** 메소드내에서 **initUI** 와 **initDrone** 메소드를 호출한다:

~~~objc

-(void) initUI
{
    self.modeLabel.text = @"N/A";
    self.gpsLabel.text = @"0";
    self.vsLabel.text = @"0.0 M/S";
    self.hsLabel.text = @"0.0 M/S";
    self.altitudeLabel.text = @"0 M";
}

- (void)initDrone
{
    self.inspireDrone = [[DJIDrone alloc] initWithType:DJIDrone_Inspire];
    self.inspireMainController = (DJIInspireMainController*)self.inspireDrone.mainController;
    self.inspireDrone.delegate = self;
    self.inspireMainController.mcDelegate = self;
}
~~~

**DJIMainController**에서 나온 **DJIInspireMainController** 서브클래스는 비행체를 제어하기 위한 mainController이고 이륙 및 착륙하는 동안 DJIMCSystemState를 얻을 수 있다. SDK의 헤더 파일을 보면 더 많은 정보를 얻을 수 있다. **initDrone** 메소드 내에서 **inspireDrone** 와 **inspireMainController** 자기 자신을 delegate한다.

이전에 추가한 **registerAppSuccess** 메소드를 기억하는가? 살펴보자:

~~~objc
#pragma mark NSNotification Selector Method
- (void)registerAppSuccess:(NSNotification *)notification
{
    [self.inspireDrone connectToDrone];
    [self.inspireMainController startUpdateMCSystemState];
}
~~~

위 메소드에서 app이 성공적으로 등록되면 DJIDrone의 **connectToDrone** 메소드를 호출해서 비행체에 연결하고 **startUpdateMCSystemState** 메소드를 호출해서 비행체 시스템 상태를 업데이트할 수 있다. 이는 비행체의 위치나 heading을 업데이트해야하는 정보이다. **viewWillDisappear** 메소드에서 드론과 연결을 해제한다. 아래와 같다 :

~~~objc
- (void)viewWillDisappear:(BOOL)animated
{
    [super viewWillDisappear:animated];

    [self.locationManager stopUpdatingLocation];
    [self.inspireDrone disconnectToDrone];
}
~~~

**viewDidLoad** 메소드에서 **droneLocation** 속성 값을 kCLLocationCoordinate2DInvalid으로 설정한다.

~~~objc
    self.droneLocation = kCLLocationCoordinate2DInvalid;
~~~

**droneLocation**를 map view 영역의 중심으로 설정하기 위해서 **focusMapAction** 메소드를 업데이트한다. 아래와 같다:

~~~objc
- (IBAction)focusMapAction:(id)sender {

    if (CLLocationCoordinate2DIsValid(self.droneLocation)) {
        MKCoordinateRegion region = {0};
        region.center = self.droneLocation;
        region.span.latitudeDelta = 0.001;
        region.span.longitudeDelta = 0.001;
        [self.mapView setRegion:region animated:YES];
    }

}
~~~

다음은 아래 보는 것과 같이 **MKMapViewDelegate** 메소드를 수정한다. annotation 변수의 클래스를 검사하고 annotationView를 **DJIAircraftAnnotationView** 클래스 타입 객체로 설정한다:

~~~objc
- (MKAnnotationView *)mapView:(MKMapView *)mapView viewForAnnotation:(id <MKAnnotation>)annotation
{
    if ([annotation isKindOfClass:[MKPointAnnotation class]]) {
        MKPinAnnotationView* pinView = [[MKPinAnnotationView alloc] initWithAnnotation:annotation reuseIdentifier:@"Pin_Annotation"];
        pinView.pinColor = MKPinAnnotationColorPurple;
        return pinView;
        
    }else if ([annotation isKindOfClass:[DJIAircraftAnnotation class]])
    {
        DJIAircraftAnnotationView* annoView = [[DJIAircraftAnnotationView alloc] initWithAnnotation:annotation reuseIdentifier:@"Aircraft_Annotation"];
        ((DJIAircraftAnnotation*)annotation).annotationView = annoView;
        return annoView;
    }
    
    return nil;
}
~~~

Furthermore, let's implement the **DJIMainControllerDelegate** method:

~~~objc
#pragma mark - DJIMainControllerDelegate Method

-(void) mainController:(DJIMainController*)mc didUpdateSystemState:(DJIMCSystemState*)state
{
    self.droneLocation = state.droneLocation;
    DJIInspireMCSystemState* inspireState = (DJIInspireMCSystemState*)state;
    if (!inspireState.canIOCWork) {
        [self.inspireMainController setFlightModeSwitchable:YES withResult:nil];
    }
    if (inspireState.isIOCWorking) {
        [self.inspireMainController setIOCWorking:NO withResult:nil];
    }
    
     self.modeLabel.text = inspireState.flightModeString;
    self.gpsLabel.text = [NSString stringWithFormat:@"%d", inspireState.satelliteCount];
    self.vsLabel.text = [NSString stringWithFormat:@"%0.1f M/S",inspireState.velocityZ];
    self.hsLabel.text = [NSString stringWithFormat:@"%0.1f M/S",(sqrtf(inspireState.velocityX*inspireState.velocityX + inspireState.velocityY*inspireState.velocityY))];
    self.altitudeLabel.text = [NSString stringWithFormat:@"%0.1f M",inspireState.altitude];
        
    [self.mapController updateAircraftLocation:self.droneLocation withMapView:self.mapView];
    double radianYaw = (state.attitude.yaw * M_PI / 180.0);
    [self.mapController updateAircraftHeading:radianYaw];
    
}
~~~

먼저 **droneLocation**를 비행체의 현재 위치로 업데이트한다. 다음으로 inspireMainController의 IOC 함수를 비활성화 시킨다.

***
**중요**: DJI Mobile SDK의 GroundStation과 IOC 사이에 충돌이 있기 때문에 동시에 이 둘을 사용할 수 없다. GroundStation를 사용하기 전에 IOC 기능을 닫았는지 확인하자. 그렇지 않으면 error가 발생한다
***

다음은 DJIMCSystemState로부터 status label의 text를 업데이트한다. 비행체의 위치와 heading은 **DJIMapController**의 관련 메소드를 호출해서 업데이트한다.

마지막으로 DJIDroneDelegate 메소드를 구현하자. 아래와 같다:

~~~objc
#pragma mark - DJIDroneDelegate Method
-(void) droneOnConnectionStatusChanged:(DJIConnectionStatus)status
{
    if (status == ConnectionSuccessed) {
        [self.inspireMainController enterNavigationModeWithResult:^(DJIError *error) {
            if (error.errorCode != ERR_Successed) {
                NSString* message = [NSString stringWithFormat:@"Enter navigation mode failed:%@", error.errorDescription];
                UIAlertView* alertView = [[UIAlertView alloc] initWithTitle:@"Enter Navigation Mode" message:message delegate:self cancelButtonTitle:@"Cancel" otherButtonTitles:@"Retry", nil];
                alertView.tag = kEnterNaviModeFailedAlertTag;
                [alertView show];
            }
        }];
    }
    
}
~~~

비행체에 연결하기가 성공하면 **DJIInspireMainController**의 **enterNavigationModeWithResult** 메소드를 호출해서 비행체가 네비게이션 모드로 성공적으로 들어갔는지 검사한다. 만약 그렇지 않다면 UIAlertView을 사용자에게 보여준다. UIAlertView의 delegate 메소드는 아래와 같다:

~~~objc
#pragma mark - UIAlertViewDelegate

- (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex
{
    if (alertView.tag == 1002) {
        if (buttonIndex == 1) {
            [self.inspireMainController enterNavigationModeWithResult:^(DJIError *error) {
                if (error.errorCode != ERR_Successed) {
                    NSString* message = [NSString stringWithFormat:@"Enter navigation mode failed:%@", error.errorDescription];
                    UIAlertView* alertView = [[UIAlertView alloc] initWithTitle:@"Enter Navigation Mode" message:message delegate:self cancelButtonTitle:@"Cancel" otherButtonTitles:@"Retry", nil];
                    alertView.tag = kEnterNaviModeFailedAlertTag;
                    [alertView show];
                }
            }];
        }
    }
}
~~~

app 테스트를 진행해보자.
프로젝트를 빌드 및 실행하고 app을 모바일 장치에 설치한다. 이렇게 한 이후, 비행체를 Micro USB케이블을 이용해서 PC나 윈도우가 실행되는 가상머신에 연결한다. 그런 다음 비행체와 리모트 컨트롤러에 전원을 넣는다. Display Simulator를 클릭한다. Simulator Config에서 현재 위치의 위도/경도 데이터를 입력한다.

![simulatorPreview](../../images/iOS/GSDemo/simulator_preview.png)

app을 실행하고 모바일 장치를 Apple의 라이트링 케이블을 이용해서 리모트 컨트롤러에 연결한다. 다음과 같은 스크린샷을 볼 수 있다:

![enterNaviModeFailed](../../images/iOS/GSDemo/enterNaviModeFailed.jpg)

**중요**: 이 문제를 해결하기 위해서, 리모트 컨트롤러의 모드를 **F**에 위치 시키고 **Retry** 버튼을 누른다. 만약 모드 선택 바가 F 위치에 있따면 autopilot는 전원이 켜지고 사용자는 반드시 **F**와 다른 위치 사이를 왔다갔다한 이후에 **Retry** 버튼을 다시 누른다.

DJI Mobile SDK에서 Ground Station, Hotpoint와 죠이스틱 기능을 사용할 때, **F** 위치에 둬야한다.

![switchFlightMode](../../images/iOS/GSDemo/switchFlightMode.png)

다음으로, PC에 DJI PC Simulator으로 가서 **Start Simulation** 버튼을 누른다. 만약 app을 검사하면 작고 붉은 비행체가 지도에 나타날 것이다:

![aircraftOnMap1](../../images/iOS/GSDemo/aircraftOnMap1.jpg)

비행체가 보이지 않느다면 "**Focus Map**" 버튼을 누르고 map view는 map view 영역의 중앙에 비행체를 중간에 위치시킥 위해서 줌인이 될 것이다:

![focusAircraft](../../images/iOS/GSDemo/focusAircraft.gif)

이제 Simulator Config에서 **Stop Simulation** 버튼을 누르면, 비행체는 지도상에서 사라지는데 이것은 simulator가 비행체로 GPS 데이터를 보내는 것을 멈췄기 때문이다.

## UI 리팩터링하기

본것과 같이, 프로젝트의 코드 구조는 간단하지만 잘 짜여지지는 않았다. 이 튜토리얼에서 더 개발하고자 한다면 리팩터링이 필요하고 UI element를 더 추가해야 한다.

### 1. 새로운 UIButton 추가와 처리
먼저  **UIViewController**의 서브클래스로 **DJIGSButtonController** 이름의 새로운 파일을 생성한다. 파일 생성시에 체크 박스에서 **Also create XIB file** 항목을 선택한다. 다음으로 **DJIGSButtonController.xib** 파일을 열고,  **Simulated Metrics**내에 **Size** 드롭다운 아래에 **Freeform**으로 size를 설정한다. view 섹션에서 **110**로 폭을 변경하고 높이는 **260**로 변경한다. 아래와 같이 변경한다 :

![freeform](../../images/iOS/GSDemo/freeform.png)
![changeSize](../../images/iOS/GSDemo/changeSize.png)

다음으로 6개 UIButton을 view로 드래그하고 각각 이름을 **Edit**, **Back**, **Clear**, **Focus Map**, **Start**, **Stop**으로 정한다. **Edit**은 **Back**위에 두고 **Focus Map**은 **Clear** 위에 둔다. **Back**과 **Clear**, **Start**, **Stop** 버튼은 숨긴다.

![gsButtons](../../images/iOS/GSDemo/gsButtons.png)

 **DJIGSButtonViewController.h** 파일에 6개 버튼 각각에 대해서 IBOutlets와 IBActions를 추가한다. app이 할 수 있는 2가지 다른 모드로 **DJIGSViewMode** 이름의 Enum을 추가한다. 다음으로 버튼에 대해서 IBAction 메소드가 트리거될 때, delegate viewcontroller로 구현할 여러 delegate 메소드를 추가한다. 마지막으로 **- (void)switchToMode:(DJIGSViewMode)mode inGSButtonVC:(DJIGSButtonViewController *)GSBtnVC;** 메소드를 추가해서 **DJIGSViewMode**가 변경될 때, 버튼의 상태가 업데이트 되도록 한다. 아래 코드와 같다:
 
 ~~~objc
 #import <UIKit/UIKit.h>
 
 typedef NS_ENUM(NSUInteger, DJIGSViewMode) {
    DJIGSViewMode_ViewMode,
    DJIGSViewMode_EditMode,
};

@class DJIGSButtonViewController;

@protocol DJIGSButtonViewControllerDelegate <NSObject>

- (void)stopBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC;
- (void)clearBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC;
- (void)focusMapBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC;
- (void)startBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC;

- (void)switchToMode:(DJIGSViewMode)mode inGSButtonVC:(DJIGSButtonViewController *)GSBtnVC;
@end

@interface DJIGSButtonViewController : UIViewController

@property (weak, nonatomic) IBOutlet UIButton *backBtn;
@property (weak, nonatomic) IBOutlet UIButton *stopBtn;
@property (weak, nonatomic) IBOutlet UIButton *clearBtn;
@property (weak, nonatomic) IBOutlet UIButton *focusMapBtn;
@property (weak, nonatomic) IBOutlet UIButton *editBtn;
@property (weak, nonatomic) IBOutlet UIButton *startBtn;

@property (assign, nonatomic) DJIGSViewMode mode;
@property (weak, nonatomic) id <DJIGSButtonViewControllerDelegate> delegate;

- (IBAction)backBtnAction:(id)sender;
- (IBAction)stopBtnAction:(id)sender;
- (IBAction)clearBtnAction:(id)sender;
- (IBAction)focusMapBtnAction:(id)sender;
- (IBAction)editBtnAction:(id)sender;
- (IBAction)startBtnAction:(id)sender;

@end
 ~~~
 
 **DJIGSButtonViewController.m** 파일을 열어서 아래 코드로 파일에 내에 모든 코드를 대체한다:
 
 ~~~objc
 #import "DJIGSButtonViewController.h"

@implementation DJIGSButtonViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    [self setMode:DJIGSViewMode_ViewMode];
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

#pragma mark - Property Method
- (void)setMode:(DJIGSViewMode)mode
{
    _mode = mode;
    [_editBtn setHidden:(mode == DJIGSViewMode_EditMode)];
    [_focusMapBtn setHidden:(mode == DJIGSViewMode_EditMode)];
    [_backBtn setHidden:(mode == DJIGSViewMode_ViewMode)];
    [_clearBtn setHidden:(mode == DJIGSViewMode_ViewMode)];
    [_startBtn setHidden:(mode == DJIGSViewMode_ViewMode)];
    [_stopBtn setHidden:(mode == DJIGSViewMode_ViewMode)];
}

#pragma mark - IBAction Methods
- (IBAction)backBtnAction:(id)sender {
    [self setMode:DJIGSViewMode_ViewMode];
    if ([_delegate respondsToSelector:@selector(switchToMode:inGSButtonVC:)]) {
        [_delegate switchToMode:self.mode inGSButtonVC:self];
    }
}

- (IBAction)stopBtnAction:(id)sender {
 
    if ([_delegate respondsToSelector:@selector(stopBtnActionInGSButtonVC:)]) {
        [_delegate stopBtnActionInGSButtonVC:self];
    }
}

- (IBAction)clearBtnAction:(id)sender {
    
    if ([_delegate respondsToSelector:@selector(clearBtnActionInGSButtonVC:)]) {
        [_delegate clearBtnActionInGSButtonVC:self];
    }
}

- (IBAction)focusMapBtnAction:(id)sender {
    
    if ([_delegate respondsToSelector:@selector(focusMapBtnActionInGSButtonVC:)]) {
        [_delegate focusMapBtnActionInGSButtonVC:self];
    }
}

- (IBAction)editBtnAction:(id)sender {
    
    [self setMode:DJIGSViewMode_EditMode];
    if ([_delegate respondsToSelector:@selector(switchToMode:inGSButtonVC:)]) {
        [_delegate switchToMode:self.mode inGSButtonVC:self];
    }
}

- (IBAction)startBtnAction:(id)sender {
    
    if ([_delegate respondsToSelector:@selector(startBtnActionInGSButtonVC:)]) {
        [_delegate startBtnActionInGSButtonVC:self];
    }
}

@end
 ~~~
 
 위와 같이 변경해서 코드 구조는 더 깔끔해졌다. 나중에 유지보수를 하는데 유리하다.

 이제 **DJIRootViewController.h** 파일로가서 **editButton** IBOutlet, **resetPointsAction** 메소드, **focusMapAction** 메소드를 삭제한다. 삭제 후에, **Main.storyboard**의 RootViewController에 링크를 건다:
 
 ![topBarView](../../images/iOS/GSDemo/topBarView.png)
 
 **DJIRootViewController.m** 파일을 열고 **DJIGSButtonViewController.h** 파일을 import한다.  **gsButtonVC**라는 이름의 **DJIGSButtonViewController** 타입의 속성을 생성하고 **DJIGSButtonViewController**의 **DJIGSButtonViewControllerDelegate** protocol을 구현한다. 아래와 같다 :
 
~~~objc
#import "DJIRootViewController.h"
#import "DJIGSButtonViewController.h"

#define kEnterNaviModeFailedAlertTag 1001

@interface DJIRootViewController ()<DJIGSButtonViewControllerDelegate>
@property (nonatomic, assign)BOOL isEditingPoints;
@property (nonatomic, strong)DJIGSButtonViewController *gsButtonVC;
@end
~~~

Furthermore, initialize the **gsButtonVC** property in the **initUI** method and move the original **focusMapAction** method's content to a new method named **focusMap**, as shown below:

~~~objc
self.gsButtonVC = [[DJIGSButtonViewController alloc] initWithNibName:@"DJIGSButtonViewController" bundle:[NSBundle mainBundle]];
[self.gsButtonVC.view setFrame:CGRectMake(0, self.topBarView.frame.origin.y + self.topBarView.frame.size.height, self.gsButtonVC.view.frame.size.width, self.gsButtonVC.view.frame.size.height)];
self.gsButtonVC.delegate = self;
[self.view addSubview:self.gsButtonVC.view];
~~~

~~~objc
- (void)focusMap
{
    if (CLLocationCoordinate2DIsValid(self.droneLocation)) {
        MKCoordinateRegion region = {0};
        region.center = self.droneLocation;
        region.span.latitudeDelta = 0.001;
        region.span.longitudeDelta = 0.001;
        
        [self.mapView setRegion:region animated:YES];
    }
}
~~~

마지막으로 **DJIGSButtonViewController**의 delegate 메소드를 구현한다. 아래와 같다 :

~~~objc
#pragma mark - DJIGSButtonViewController Delegate Methods
- (void)stopBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC
{
}

- (void)clearBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC
{
    [self.mapController cleanAllPointsWithMapView:self.mapView];
}

- (void)focusMapBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC
{
    [self focusMap];
}

- (void)startBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC
{
}

- (void)switchToMode:(DJIGSViewMode)mode inGSButtonVC:(DJIGSButtonViewController *)GSBtnVC
{
    if (mode == DJIGSViewMode_EditMode) {
         self.isEditingPoints = YES;
        [self focusMap];
    }else
    {
         self.isEditingPoints = No;
    }
}
~~~

**- (void)switchToMode:(DJIGSViewMode)mode inGSButtonVC:(DJIGSButtonViewController *)GSBtnVC** delegate 메소드에서 **focusMap** 메소드를 호출한다. 이렇게 함으로써 edit 버튼이 눌러졌을 때 비행체의 위치로 map view를 포커스시킨다. 이렇게 하면 사용자가 edit하기 위해 zoom in하는 동작을 막을 수 있다. app이 edit 모드인 경우에 **isEditingPoints** 속성 값이 **YES**로 설정된다. 

이제 프로젝트를 빌드하고 실행하자. 그리고 **Edit**와 **Back** 버튼을 눌러보자. 누를 때 애니메이션은 아래와 같다:

![pressEditBtn](../../images/iOS/GSDemo/pressEditBtn.gif)

## 네비게이션 태스크 설정

### 1. DJIGroundStationWaypoint

**DJIGroundStationWaypoint.h** 파일로 가서 확인해보자. 예제로 사용할 수 있다 :

~~~objc
-(id) initWithCoordinate:(CLLocationCoordinate2D)coordinate;
~~~
to create a waypoint object with a specific coordinate. Once you create a waypoint, you can add a **DJIWaypointAction** to it by calling:

~~~objc
-(BOOL) addWaypointAction:(DJIWaypointAction*)action;
~~~

waypoint를 가지고 좌표, 고도, heading, 수평속도 등을 설정할 수 있다. 자세한 내용은 **DJIGroundStationWaypoint.h** 헤더 파일을 참조하자.

### 2. DJIGroundStationTask

**DJIGroundStationTask**는 GroundStation Waypoint 태스크를 사용하기 원할 때 이용한다. 새로운 태스크를 생성하기 위해서 클래스 메소드인 **+(id) newTask;**을 직접 호출할 수 있다. 태스크를 생성하면 아래 메소드를 이용해서 **DJIGroundStationWaypoint** 타입의 waypoint을 추가할 수 있다:

~~~objc
-(void) addWaypoint:(DJIGroundStationWaypoint*)waypoint;
~~~

반대로 아래 메소드를 이용해서 태스크에서 waypoint를 삭제할 수 있다:

~~~objc
-(void) removeWaypoint:(DJIGroundStationWaypoint*)waypoint;
~~~
 method.
 
 **DJIGroundStationTask**의 **isLoop** 속성을 설정하는데 이는 태크크 루프를 실행할지 여부를 결정한다. 또 태스크를 마쳤을 때 비행체가 하는 것을 설정하기 위해서 **DJIGSTaskFinishedAction** enum 타입의 **finishedAction**를 설정할 수 있다. 마지막으로 태스크를 실행하는 동안 비행체의 헤딩을 설정하는 **DJIGSHeadingMode** enum 타입의 **headingMode** 속성을 설정할 수 있다. 아래에서 헤더파일의 일부를 볼 수 있다 :
 
~~~objc
/**
 *  Action of task when finished
 */
typedef NS_ENUM(NSUInteger, DJIGSTaskFinishedAction){
    /**
     *  No action. aircraft will stay at the last waypoint
     */
    GSTaskFinishedNoAction,
    /**
     *  Aircraft will go home
     */
    GSTaskFinishedGoHome,
    /**
     *  Aircraft will auto landing
     */
    GSTaskFinishedAutoLanding,
    /**
     *  Aircraft will go to the first waypoint
     */
    GSTaskFinishedGoFirstWaypoint
};

/**
 *  Heading mode
 */
typedef NS_ENUM(NSUInteger, DJIGSHeadingMode){
    /**
     *  Aircraft's heading toward to the next waypoint
     */
    GSHeadingTowardNexWaypoint,
    /**
     *  Aircraft's heading using the initial direction
     */
    GSHeadingUsingInitialDirection,
    /**
     *  Aircraft's heading control by the remote controller
     */
    GSHeadingControlByRemoteController,
    /**
     *  Aircraft's heading using the waypoint's heading value
     */
    GSHeadingUsingWaypointHeading,
};

/**
 *  Whether execute task looply. Default is NO
 */
@property(nonatomic, assign) BOOL isLoop;

/**
 *  Action for the aircraft while the task finished
 */
@property(nonatomic, assign) DJIGSTaskFinishedAction finishedAction;

/**
 *  How the aircraft heading while executing task
 */
@property(nonatomic, assign) DJIGSHeadingMode headingMode;

/**
 *  Create new task
 *
 */
+(id) newTask;

/**
 *  Add waypoint
 *
 *  @param waypoint
 */
-(void) addWaypoint:(DJIGroundStationWaypoint*)waypoint;

/**
 *  Remove one waypoint
 *
 *  @param waypoint Waypoint will be removed
 */
-(void) removeWaypoint:(DJIGroundStationWaypoint*)waypoint;

~~~

좀더 상세한 내용은 DJI Mobile SDK내에 있는 **DJIGroundStationTask.h** 헤더 파일을 참고하자.

### 3. DJIWaypointConfigViewController 생성

이 데모에서 map view에 추가한 각 waypoint의 인자는 동일하다.

사용자가 waypoint의 인자를 설정하기 위해서 새로운 ViewController를 생성하자. Xcode의 프로젝트 네비게이터로 이동해서 **GSDemo** 폴더에서 오른쪽 클릭하고 **New File...**를 선택하고 **UIViewController**를 서브클래스를 설정한다. **DJIWaypointConfigViewController**라고 이름 붙이고 "Also create XIB file"가 선택되었는지 확인한다. 다음으로  **DJIWaypointConfigViewController.xib** 파일을 열고 UI를 구현한다. 아래와 같다:

![wayPointConfig](../../images/iOS/GSDemo/wayPointConfig.png)

Waypoint Configuration ViewController에서 UITextField를 사용해서 사용자가 **DJIGroundStationWaypoint** 객체의 **altitude** 속성을 설정하게 한다. **DJIGroundStationTask** 객체의 **isLoop** 속성을 설정하는 UISwitcher이 있다. 이것을 통해 태스크가 반복되는 것을 켜고/끄기가 가능하다. **DJIGroundStationWaypoint**의 **horizontalVelocity**,  **DJIGroundStationTask**의 **finishedAction** 속성, **DJIGroundStationTask**의 **headingMode**속성을 구성하기 위한 3개 UISegmentedControls가 있다.

밑에 **Cancel**과 **Finish** 동작을 위해서 UIButtons을 추가한다. 설정(프레임의 위치, 크기, UI element의 배경색상 등)에 관한 상세한 내용은 다운받은 프로젝트 소스코드에서 **DJIWaypointConfigViewController.xib** 파일을 살펴보자.

**DJIWaypointConfigViewController.h** 파일 내부에서 UI element 각각의 IBOutlets 와 IBActions를 생성하자. 아래와 같다 :

~~~objc
#import <UIKit/UIKit.h>

@class DJIWaypointConfigViewController;

@protocol DJIWaypointConfigViewControllerDelegate <NSObject>

- (void)cancelBtnActionInDJIWaypointConfigViewController:(DJIWaypointConfigViewController *)waypointConfigVC;
- (void)finishBtnActionInDJIWaypointConfigViewController:(DJIWaypointConfigViewController *)waypointConfigVC;

@end

@interface DJIWaypointConfigViewController : UIViewController

@property (weak, nonatomic) IBOutlet UITextField *altitudeTextField;
@property (weak, nonatomic) IBOutlet UISwitch *repeatSwitcher;
@property (weak, nonatomic) IBOutlet UISegmentedControl *speedSegmentedControl;
@property (weak, nonatomic) IBOutlet UISegmentedControl *actionSegmentedControl;
@property (weak, nonatomic) IBOutlet UISegmentedControl *headingSegmentedControl;

@property (weak, nonatomic) id <DJIWaypointConfigViewControllerDelegate>delegate;

- (IBAction)cancelBtnAction:(id)sender;
- (IBAction)finishBtnAction:(id)sender;

@end
~~~

여기서 **Cancel**과 **Finish** 버튼이 눌러질 때 호출되는 **DJIWaypointConfigViewControllerDelegate** delegate 메소드를 생성했다.

** DJIWaypointConfigViewController.m** 파일에 있는 코드를 아래 코드로 대체하자:

~~~objc
#import "DJIWaypointConfigViewController.h"

@interface DJIWaypointConfigViewController ()

@end

@implementation DJIWaypointConfigViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    [self initUI];   
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

- (void)initUI
{
    self.altitudeTextField.text = @"50";
    [self.repeatSwitcher setOn:NO animated:YES]; //Turn off it to cancel execute GroundStation Task looply
    [self.speedSegmentedControl setSelectedSegmentIndex:1]; //Set the horizontal speed to Mid
    [self.actionSegmentedControl setSelectedSegmentIndex:1]; //Set the finishAction to GSTaskFinishedGoHome
    [self.headingSegmentedControl setSelectedSegmentIndex:0]; //Set the headingMode to GSHeadingTowardNexWaypoint
    
}

- (IBAction)cancelBtnAction:(id)sender {
 
    if ([_delegate respondsToSelector:@selector(cancelBtnActionInDJIWaypointConfigViewController:)]) {
        [_delegate cancelBtnActionInDJIWaypointConfigViewController:self];
    }
}

- (IBAction)finishBtnAction:(id)sender {
    
    if ([_delegate respondsToSelector:@selector(finishBtnActionInDJIWaypointConfigViewController:)]) {
        [_delegate finishBtnActionInDJIWaypointConfigViewController:self];
    }
}

@end
~~~

위 코드에서 **initUI** 메소드를 만들고 이것은  **viewDidload**에서 호출되어 기본 데이터 값으로 UI control을 초기화한다. 예를 들자면, **altitudeTextField**가 **50**으로 기본 값을 가지도록 해서, 사용자가 app이 처음 실행될 때, textField에 있는 고도값을 입력할 필요가 없도록 한다. 시작하기 전에 설정을 변경하는 대신에 바로 **Finishi** 버튼을 누를 수 있다.

## GroundStation 태스크 설정

### 1. DJIWaypointConfigViewController를 DJIRootViewController에 추가하기

이제 **DJIRootViewController.m** 파일로 가보자. 맨 위에 **DJIWaypointConfigViewController.h** 헤더 파일을 추가하고 **waypointConfigVC** 이름으로 **DJIWaypointConfigViewController** 타입의 속성을 생성한다. 다음으로 **DJIWaypointConfigViewControllerDelegate** protocol를 구현한다. 아래와 같다:

~~~objc
#import "DJIRootViewController.h"
#import "DJIGSButtonViewController.h"
#import "DJIWaypointConfigViewController.h"

#define kEnterNaviModeFailedAlertTag 1001

@interface DJIRootViewController ()<DJIGSButtonViewControllerDelegate, DJIWaypointConfigViewControllerDelegate>
@property (nonatomic, assign)BOOL isEditingPoints;
@property (nonatomic, strong)DJIGSButtonViewController *gsButtonVC;
@property (nonatomic, strong)DJIWaypointConfigViewController *waypointConfigVC;
@end
~~~

**waypointConfigVC** 인스턴스 변수를 초기화하는 코드를 추가해보고 **initUI** 밑에 **DJIRootViewController**를 delegate로 설정하자:

~~~objc
-(void) initUI
{
    self.modeLabel.text = @"N/A";
    self.gpsLabel.text = @"0";
    self.vsLabel.text = @"0.0 M/S";
    self.hsLabel.text = @"0.0 M/S";
    self.altitudeLabel.text = @"0 M";
    
    self.gsButtonVC = [[DJIGSButtonViewController alloc] initWithNibName:@"DJIGSButtonViewController" bundle:[NSBundle mainBundle]];
    [self.gsButtonVC.view setFrame:CGRectMake(0, self.topBarView.frame.origin.y + self.topBarView.frame.size.height, self.gsButtonVC.view.frame.size.width, self.gsButtonVC.view.frame.size.height)];
    self.gsButtonVC.delegate = self;
    [self.view addSubview:self.gsButtonVC.view];
    
    self.waypointConfigVC = [[DJIWaypointConfigViewController alloc] initWithNibName:@"DJIWaypointConfigViewController" bundle:[NSBundle mainBundle]];
    self.waypointConfigVC.view.alpha = 0;
    self.waypointConfigVC.view.center = self.view.center;
    self.waypointConfigVC.delegate = self;
    [self.view addSubview:self.waypointConfigVC.view];
    
}
~~~

위 코드에서 **waypointConfigVC** view의 **alpha** 속성을 0으로 설정해서 처음에 감춰지도록 설정한다. 다음으로 **DJIRootViewController** view의 중간에 위치하도록 한다.

**DJIWaypointConfigViewControllerDelegate** 메소드를 구현하면 아래와 같다:

~~~objc
#pragma mark - DJIWaypointConfigViewControllerDelegate Methods

- (void)cancelBtnActionInDJIWaypointConfigViewController:(DJIWaypointConfigViewController *)waypointConfigVC
{
    __weak DJIRootViewController *weakSelf = self;
    
    [UIView animateWithDuration:0.25 animations:^{
        weakSelf.waypointConfigVC.view.alpha = 0;
    }];
}

- (void)finishBtnActionInDJIWaypointConfigViewController:(DJIWaypointConfigViewController *)waypointConfigVC
{
    __weak DJIRootViewController *weakSelf = self;
    
    [UIView animateWithDuration:0.25 animations:^{
        weakSelf.waypointConfigVC.view.alpha = 0;
    }];

}
~~~

처음 delegate 메소드에서 UIView에서 클래스 메소드를 사용해서 **waypointConfigVC**의 **alpha** 값을 변경하여 움직이도록 한다.:

~~~objc
+ (void)animateWithDuration:(NSTimeInterval)duration animations:(void (^)(void))animations NS_AVAILABLE_IOS(4_0);
~~~

2번째 delegate 메소드에서 첫번째 delegate 메소드에서 했던 것과 동일한 것을 한다.

마지막으로 코드를 교체하자

~~~objc
- (void)startBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC;
~~~
method with the following code to show the **waypointConfigVC**'s view when the user presses the **start** button:

~~~objc
- (void)startBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC
{
    __weak DJIRootViewController *weakSelf = self;

    [UIView animateWithDuration:0.25 animations:^{
        weakSelf.waypointConfigVC.view.alpha = 1.0;
    }];
        
}
~~~

완성하면, 프로젝트를 빌드하고 실행하자. **Edit**버튼과 **Start**버튼을 눌러서 **waypointConfigVC**의 view를 보여준다:

![waypointConfigView](../../images/iOS/GSDemo/waypointConfigView.png)

### 2. GroundStation 태스크 처리

**DJIRootViewController.h** 파일로 돌아가보자. 먼저 인터페이스에서 **GroundStationDelegate** 와 **DJINavigationDelegate** protocols를 구현한다. **gsTask**라는 이름의 **DJIGroundStationTask** 타입 속성을 생성한다. **uploadProgressView**라는 이름의 **UIAlertView** 타입의 속성을 생성한다. UIAlertView는 GroundStation 태스크의 상태를 보여주기 위해서 사용한다. **DJIRootViewController**의 헤더파일을 완성하면 아래와 같다:

~~~objc
#import <UIKit/UIKit.h>
#import <DJISDK/DJISDK.h>
#import "DJIMapController.h"
#import <MapKit/MapKit.h>
#import <CoreLocation/CoreLocation.h>

@interface DJIRootViewController : UIViewController<MKMapViewDelegate, CLLocationManagerDelegate, DJIDroneDelegate, DJIMainControllerDelegate, GroundStationDelegate, DJINavigationDelegate>

@property (nonatomic, strong) DJIMapController *mapController;
@property (weak, nonatomic) IBOutlet MKMapView *mapView;
@property(nonatomic, strong) CLLocationManager* locationManager;
@property(nonatomic, assign) CLLocationCoordinate2D userLocation;
@property(nonatomic, assign) CLLocationCoordinate2D droneLocation;
@property (nonatomic, strong) UITapGestureRecognizer *tapGesture;

@property (weak, nonatomic) IBOutlet UIView *topBarView;

@property(nonatomic, strong) IBOutlet UILabel* modeLabel;
@property(nonatomic, strong) IBOutlet UILabel* gpsLabel;
@property(nonatomic, strong) IBOutlet UILabel* hsLabel;
@property(nonatomic, strong) IBOutlet UILabel* vsLabel;
@property(nonatomic, strong) IBOutlet UILabel* altitudeLabel;

@property(nonatomic, strong) DJIDrone* inspireDrone;
@property(nonatomic, strong) DJIInspireMainController* inspireMainController;

@property(nonatomic, strong) DJIGroundStationTask* gsTask;
@property(nonatomic, strong) UIAlertView* uploadProgressView;

@end
~~~

다음으로 **DJIRootViewController.m** 파일로 가서 **inspireMainController** 인스턴스 변수의 **groundStationDelegate** 와 **navigationDelegate**를 **DJIRootViewController**로 설정한다. 아래와 같다 :

~~~objc
- (void)initDrone
{
    self.inspireDrone = [[DJIDrone alloc] initWithType:DJIDrone_Inspire];
    self.inspireMainController = (DJIInspireMainController*)self.inspireDrone.mainController;
    self.inspireDrone.delegate = self;
    self.inspireMainController.mcDelegate = self;
    self.inspireMainController.groundStationDelegate = self;
    self.inspireMainController.navigationDelegate = self;
}
~~~

다음 코드를 **startBtnActionInGSButtonVC** delegate 메소드의 밑에 추가한다:

~~~objc
- (void)startBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC
{
    __weak DJIRootViewController *weakSelf = self;

    [UIView animateWithDuration:0.25 animations:^{
        weakSelf.waypointConfigVC.view.alpha = 1.0;
    }];
    
    NSArray* wayPoints = self.mapController.wayPoints;
    if (wayPoints == nil || wayPoints.count == 0) {
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"No waypoint for mission" message:@"" delegate:self cancelButtonTitle:@"OK" otherButtonTitles:nil];
        [alert show];
        return;
    }

    self.gsTask = [DJIGroundStationTask newTask];
    for (int i = 0; i < wayPoints.count; i++) {
        CLLocation* location = [wayPoints objectAtIndex:i];
        if (CLLocationCoordinate2DIsValid(location.coordinate)) {
            DJIGroundStationWaypoint* waypoint = [[DJIGroundStationWaypoint alloc] initWithCoordinate:location.coordinate];
            [self.gsTask addWaypoint:waypoint];
        }
    }
    
}
~~~

위에서 추가한 코드내에서 **wayPoints**라는 이름의 지역 변수인 **NSArray**를 생성하고 **mapController**의 **wayPoints** 배열을 값으로 할당한다. 다음으로 배열이 있는지 혹은 비었는지를 검사한다. 만약 존재하지 않거나 비어있다면, UIAlertView을 통해서 waypoint가 없다는 것을 사용자에게 알려준다.

**중요**: 안전을 위해서, 이 튜토리얼 Part 1에서 논의했던 것과 같이 미션을 시작하기 전에 GPS 위성을 검사하는 로직을 추가하는 것이 중요하다. 만약 위성 카운트가 6보다 작다면 사용자에게 ground station 미션을 시작하지 못하게 해야하고 경고를 보여줘야 한다. DJI PC Simulator를 사용하기 때문에, 항상 위성 카운트가 10인 완벽한 상황에서 app을 테스트하고 있기 때문이다.

다음으로 **DJIGroundStationTask**의 클래스 메서도인 **newTask**를호출해서 **gsTask** 인스턴스 변수를 초기화한다. **wayPoints** 배열에서 각 waypoint에 대한 **CLLocation**를 얻기 위해 루프를 사용한다. 그리고 아래 메소드를 이용해서 유효한 **coordinate** 여부를 검사한다:

~~~objc
BOOL CLLocationCoordinate2DIsValid(CLLocationCoordinate2D coord);
~~~

마지막으로 좌표가 유효하면, **DJIGroundStationWaypoint**의 waypoint를 생성하고 메소드를 호출해서 **gsTask**에 추가한다:

~~~objc
-(void) addWaypoint:(DJIGroundStationWaypoint*)waypoint;
~~~

완료하면, DJIWaypointConfigViewController의 delegate 메소드인 **finishBtnActionInDJIWaypointConfigViewController**로 가보자. 해당 메소드의 밑에 **gsTask** 와 관련해서 아래 코드를 추가한다:

~~~objc
- (void)finishBtnActionInDJIWaypointConfigViewController:(DJIWaypointConfigViewController *)waypointConfigVC
{
    __weak DJIRootViewController *weakSelf = self;
    
    [UIView animateWithDuration:0.25 animations:^{
        weakSelf.waypointConfigVC.view.alpha = 0;
    }];
    
    for (int i = 0; i < self.gsTask.waypointCount; i++) {
        DJIGroundStationWaypoint* waypoint = [self.gsTask waypointAtIndex:i];
        waypoint.altitude = [self.waypointConfigVC.altitudeTextField.text floatValue];
        waypoint.horizontalVelocity = self.waypointConfigVC.speedSegmentedControl.selectedSegmentIndex * 3 + 3;
    }
    
    self.gsTask.isLoop = self.waypointConfigVC.repeatSwitcher.isOn;
    self.gsTask.headingMode = (DJIGSHeadingMode)self.waypointConfigVC.headingSegmentedControl.selectedSegmentIndex;
    self.gsTask.finishedAction = (DJIGSTaskFinishedAction)self.waypointConfigVC.actionSegmentedControl.selectedSegmentIndex;
    
    [self.inspireMainController uploadGroundStationTask:self.gsTask];
}
~~~

위에서 각 **DJIGroundStationWaypoint**의 **altitude**와 **hrozontalVelocity* 속성을 설정하기 위해서 루프를 이용한다.**DJIWaypointConfigViewController** 내에 설정된 값을 기반으로 **gsTask** waypoint 배열에 있다. 이것을 완료하고나서 **gsTask**의 **isLoop**, **headingMode**, **finishedAction** 속성을 업데이트한다. 마지막으로 **DJIInspireMainController**(이 예제에서 Inspire을 사용한다고 가정했다)의 **uploadGroundStationTask** 메소드를 호출해서 groundstation 태스크를 업데이트한다.

 **hideProgressView**라는 새로운 메소드를 생성하고 **uploadProgressView**를 숨기고 Mission Status를 업데이트 하기 위해서 **GroundStationDelegate** 메소드 내에서 구현하자. 아래와 같다 :

~~~objc
-(void) hideProgressView
{
    if (self.uploadProgressView) {
        [self.uploadProgressView dismissWithClickedButtonIndex:-1 animated:YES];
    }
}

#pragma mark - GroundStationDelegate
-(void) groundStation:(id<DJIGroundStation>)gs didExecuteWithResult:(GroundStationExecuteResult*)result
{
    if (result.currentAction == GSActionStart) {
        if (result.executeStatus == GSExecStatusFailed) {
            [self hideProgressView];
            NSLog(@"Mission Start Failed...");
        }
    }
    if (result.currentAction == GSActionUploadTask) {
        if (result.executeStatus == GSExecStatusFailed) {
            [self hideProgressView];
            NSLog(@"Upload Mission Failed");
        }
    }
}

-(void) groundStation:(id<DJIGroundStation>)gs didUploadWaypointMissionWithProgress:(uint8_t)progress
{
    if (self.uploadProgressView == nil) {
        self.uploadProgressView = [[UIAlertView alloc] initWithTitle:@"Mission Uploading" message:@"" delegate:nil cancelButtonTitle:nil otherButtonTitles:nil];
        [self.uploadProgressView show];
    }
    
    NSString* message = [NSString stringWithFormat:@"%d%%", progress];
    [self.uploadProgressView setMessage:message];
}
~~~

위와 같이, 처음 delegate 메소드는 ground station 결과를 얻기 위해서 이용한다. **hideProgressView** 메소드를 호출하는 것은 **GroundStationExecuteResult** 내부에서 **currentAction** and **executeStatus**를 검사한 후에 **uploadProgressView**를 숨기기 위해서다.

2번째 delegate 메소드는 waypoint 미션을 업로드하는 절차를 검사하는데 사용한다. 여기서는 **uploadProgressView** 인스턴스 변수를 초기화하고 **message** 속성을 delegate 메소드의 **progress** 변수로 설정한다. 좀더 상세한 내용은 **DJIGroundStation.h** 파일을 확인하자.

마지막으로 **DJINavigationDelegate** 메소드를 구현한다. 아래와 같다:

~~~objc
-(void) onNavigationMissionStatusChanged:(DJINavigationMissionStatus*)missionStatus
{
}

-(void) onNavigationPostMissionEvents:(DJINavigationEvent*)event
{
        if (event.eventType == NavigationEventMissionUploadFinished) {
        DJINavigationMissionUploadFinishedEvent* finishedEvent = (DJINavigationMissionUploadFinishedEvent*)event;
        [self.uploadProgressView setTitle:@"Mission Upload Finished"];
        if (!finishedEvent.isMissionValid) {
            [self.uploadProgressView setMessage:@"Mission Invalid!"];
        }
        else
        {
            [self.uploadProgressView setMessage:[NSString stringWithFormat:@"Estimate Time:%d s", (int)finishedEvent.eatimateTime]];
            [self.inspireMainController startGroundStationTask];
        }
        
        [self performSelector:@selector(hideProgressView) withObject:nil afterDelay:3.0];
    }
    else if (event.eventType == NavigationEventMissionExecuteFinished)
    {
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Mission Finished" message:@"" delegate:self cancelButtonTitle:@"OK" otherButtonTitles:nil];
        [alert show];

    }
}
~~~

첫번째 delegate는 미션 상태를 검사하는데 사용한다. **missionStatus** 변수의 **missionType** 속성에 접근해서 미션의 타입을 검사할 수 있다. **DJINavigation.h**내에 정의되어 있고 **DJINavigationMissionType** 타입의 enum에서 볼 수 있다:

~~~objc
typedef NS_ENUM(uint8_t, DJINavigationMissionType)
{
    /**
     *  No mission
     */
    NavigationMissionNone,
    /**
     *  Waypoint mission
     */
    NavigationMissionWaypoint,
    /**
     *  Hotpoint mission
     */
    NavigationMissionHotpoint,
    /**
     *  Followme mission, Not support now
     */
    NavigationMissionFollowme,
    /**
     *  Unknown mission
     */
    NavigationMissionUnknown
};
~~~

2번째 delegate 메소드는 현재 미션 이벤트를 검사한다. 현재 어떤 이벤트 미션인지를 얻어오기 위해서 **event** 변수의 **eventType** 속성을 검사한다. **DJINavigation.h** 헤더 파일내에 **DJINavigationEventType**가 정의되어 있다. 아래와 같다 :

~~~objc
/**
 *  Navigation event type
 */
typedef NS_ENUM(uint8_t, DJINavigationEventType){
    /**
     *  Mission upload finished event
     */
    NavigationEventMissionUploadFinished,
    /**
     *  Mission execute finished event
     */
    NavigationEventMissionExecuteFinished,
    /**
     *  Aircraft reach target waypoint event
     */
    NavigationEventWaypointReached,
};
~~~
2번째 delegate 메소드에서 eventType이 **NavigationEventMissionUploadFinished**와 동일한 경우에 **DJINavigationMissionUploadFinishedEvent**가 **DJINavigationEvent**의 서브클래스이고 메소드의 **event** 변수를 캐스팅해서 **DJINavigationMissionUploadFinishedEvent** 변수를 생성한다. **uploadProgressView**의 타이틀을 "Mission Upload Finished"로 업데이트한다. 결국  **finishedEvent**의 **isMissionValid** bool 값은 false가 된다면, **uploadProgressView**의 **message**를 "Mission Invalid!"로 설정한다. 그렇지 않으면 **uploadProgressView**의 **message**를 **finishedEvent** 객체의 **eatimateTime** 속성을 이용해서 포맷 **NSString** 객체로 설정한다. 

다음으로  waypoint ground station 태스크를 시작하기 위해 **inspireMainController**의 **startGroundStationTask** 메소드를 호출한다. 이렇게 하면 **performSelector** 메소드를 호출하고 3초간 지연 후에 progress view를 감추기 위해서 **hideProgressView**로 들어간다.

eventType이 **NavigationEventMissionExecuteFinished**와 동일할 때, 사용자에게 waypoint ground station 미션이 종료되었다는 것을 알리기 위해서 **UIAlertView**를 보여준다.

마지막으로 **stopBtnActionInGSButtonVC** 메소드를 구현하자. 이것은 ground station 태스크를 멈추기 위한 **DJIGSButtonViewController** delegate 메소드이다. 아래와 같다 :

~~~objc
- (void)stopBtnActionInGSButtonVC:(DJIGSButtonViewController *)GSBtnVC
{
    [self.inspireMainController stopGroundStationTask];
}
~~~

## App 테스트

이 튜토리얼에서 따라 많은 진전이 있었다. 이제 전체 app을 테스트할 때다.

**중요**: 비행체의 배터리 레벨이 10% 이상이 되지 않으면 ground station 미션은 동작하지 않는다.

모바일 장치로 app을 설치하기 위해서 프로젝트를 빌드 및 실행한다. Micro USB 케이블을 이용해서 비행체를 PC나 윈도우가 실행되는 가상머신에 연결한다. 그런 다음, 리모트 컨트롤러와 비행체에 순서대로 전원을 넣는다. 다음으로 DJI PC Simulator내에 **Display Simulator** 버튼을 누르고 현재 위치의 위도/경도 데이터를 simulator에 입력한다.

![simulatorPreview](../../images/iOS/GSDemo/simulator_preview.png)

Apple의 라이트링 케이블을 이용해서 모바일 장치를 리모트 컨트롤러에 연결하고 app을 실행한다. 다음과 같은 스크린샷을 보게 된다:

![enterNaviModeFailed](../../images/iOS/GSDemo/enterNaviModeFailed.jpg)

이런 이슈를 만나면 이 문제를 해결하기 위한 해결책을 튜토리얼의 Part 1을 참고하자. 다음으로 DJI PC Simulator로 돌아와서 **Start Simulation** 버튼을 누른다. 작고 붉은 비행체가 app의 지도상에 나타난다. 아래와 같다:

![aircraftOnMap1](../../images/iOS/GSDemo/aircraftOnMap1.jpg)

"**Edit**" 버튼을 누르고 map view는 여러분이 있는 지역을 줌인하고 비행체를 가운데 위치시킨다:

![locateAircraft](../../images/iOS/GSDemo/locateAircraft.gif)

map view 상에서 원하는 위치를 터치하면 waypoint 기능을 테스트할 수 있다. 탭을 할때마다, waypoint가 추가되고 보라색 핀이 waypoint 위치에 정확하게 나타난다. 아래와 같다:

![addWayPoints](../../images/iOS/GSDemo/addWaypoints_part2.gif)

**Start** 버튼을 누르면  **Waypoint Configuration** view가 나타난다. 변경된 것에 만족하면 **Finish** 버튼을 누른다. waypoint 미션은 업로드를 시작하며 마칠때가 되면 미션이 처리되기 시작한다. map view에서 설정한 waypoint 방향으로 비행체가 움직이기 시작하는 것을 볼 수 있다:

![flying](../../images/iOS/GSDemo/flying.gif)

동시에 Inspire 1이 이륙하고 자동으로 DJI PC Simulator에서 자동으로 날기 시작하는 것을 볼 수 있다.

![takeOff] (../../images/iOS/GSDemo/takeOff.gif)

waypoint 미션이 완료될 때, **Mission Finished**라는 제목의 알림 화면이 나타나며 Inspire 1이 돌아오기 시작한다.

![missionFinished](../../images/iOS/GSDemo/missionFinished.jpg) 

리모트 컨트롤러는 beep음을 내기 시작하고 리모트 컨트롤러에 있는 **Go Home** 버튼은 흰색 빛을 내기 시작할 것이다. 이제 DJI PC Simulator를 살펴보자:

![landing](../../images/iOS/GSDemo/landing.gif)
 
Inspire 1은 돌아와서 착륙하며 리모트 컨트롤러의 beep음은 멈춘다. app은 보통 상태로 돌아간다. 만약 **Clear** 버튼을 누르면, 이전에 설정한 모든 waypoint가 삭제되고 다른 waypoint 미션이 시작된다. 미션 동안 만약에 groundstation 태스크를 멈추고 싶다면, **Stop** 버튼을 누르면 된다.

## 요약
   이 튜토리얼에서 groundstation app을 테스트, 비행체 펌웨어 업그레이드, DJI Mobile SDK를 이용해서 간단한 map view 만들기, map view의 annotation 수정하기, DJI PC Simulator에서 GPS 데이터를 이용해서 map view에서 비행체 보여주기 등을 DJI PC Simulator를 사용해서 배웠다. 또 **DJIGroundStationWaypoint** 와 **DJIGroundStationTask** 설정하는 법, DJIGroundStationTask를 waypoint에 추가하기, ground station 태스크를 구성하기 위한 **upload**, **start**, **stop** 사용법 등을 DJIInspireMainController(이 예제에서 Inspire를 사용하고 있다)의 어떤 메소드를 사용해야하는지 배웠다. 마지막으로 ground station 태스크에서 정보를 얻기 위해서 **DJINavigationDelegate** 와 **GroundStationDelegate** 메소드 사용하는 법을 배웠다.
      
   축하한다! 이제 데모 프로젝트를 마쳤고 배운 것을 기초로 여러분의 ground station app을 만들 수 있다. waypoint가 추가되는 방법(지도상에서 선을 그려서 waypoint를 자동으로 생성)을 개선하고 waypoint의 속성(heading, 수평 속도 등)을 이용해서 다양한 기능을 추가할 수 있다. 멋진 ground station app을 만들기 위해서 아직 갈 길이 멀다. 행운을 빌며 이 튜토리얼을 즐겼기를 바란다.