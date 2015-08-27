# 사진과 비디오 재생 앱 생성하기

*주의: 이 튜토리얼은 Phantom 3 Professional와 Inspire 1에만 해당된다.*

<!-- toc -->

## 소개
이번 튜토리얼을 통해서 Inspire 1, Phantom 3 Professional 그리고 M100에 부착된 카메라와 익숙진다. Camera Mode를 제어하는 방법, 사진 및 비디오 앨범 앱을 적합한 UI를 만드는 방법 그리고 미디어 파일을 선택하고 다운로드 받는 방법을 익힌다. 이제 시작해 보자.

이번 튜토리얼의 데모 프로젝트를 다운로드 받을 수 있다 : <https://github.com/DJI-Mobile-SDK/Android-PlaybackDemo>

이 튜토리얼을 따라하는 동안 데모 프로젝트의 소스 코드를 참고할 것이다.

## Camera Mode

앨범 app을 개발하기 전에, 각 camera mode에서 SDK가 지원하는 범위를 간략히 알아보도록 하자. Inspire 1, Phantom 3 Professional 그리고 M100에는 4개 camera mode가 있다. (**Capture Mode, Record Mode, Playback Mode**, **Download Mode**). **Capture Mode**에는 사진을 찍고 카메라에 파라미터를 설정과 같이 사용자가 캡쳐 기능을 사용할 수 있다. **Record Mode**에서는 이런 캡쳐 기능이 막혀있고 사용자는 비디오 녹화 시작/정지만 가능하다. **Playback Mode**는 SD카드에 있는 미디어 파일 미리보기 기능을 사용가능하다. **Download Mode**는 미디어 파일을 다운로드 받을 수 있다. 사용하기 전에 올바르게 설정해야 한다.


** 하지만 Phantom 3 Advanced는 3개 camera mode만 제공한다 : Capture Mode, Record Mode, Download Mode. 추후 튜토리얼에서 다룬다.**

Mobile Android SDK에서 개발자는 `setCameraMode`를 호출해서 가능한 camera mode를 설정하고 `getCameraMode`를 통해 camera mode를 얻어올 수 있다.(Phantom 2 시리즈는 2개 모드만 제공한다 : `CameraMode.Camera_Camera_Mode` 와 `Camera.Camera_USB_Mode`이며 최근 다른 드론에서는 지원하지 않는다.)

~~~java
	//Set up the Camera Mode. 
    DJIDrone.getDjiCamera().setCameraMode(CameraMode, DJIExecuteResultCallback)
	
	//Get the Camera Mode
	DJIDrone.getDjiCamera().getCameraMode(DJICameraModeCallBack)
~~~

## 카메라 재생상태 구별하기

위에서 소개한 바와 같이 3개 혹은 4개 camera mode가 있다.(여러분이 사용하는 드론에 따라) 설정한 mode가 올바로 인지하는 방법은 포토 앨범 app을 개발하는 핵심이다. mobile android SDK에서 camera 재생 상태의 현재 상태를 반환하는 callback 함수가 있다. 개발자는 상태가 변경되었을 때 실행할 callback 함수를 설정할 수 있다. 이 callback은 초당 10번 호출된다.

~~~java
	DJIDrone.getDjiCamera().setDJICameraPlayBackStateCallBack(new DJICameraPlayBackStateCallBack() {
		@Override
		public void onResult(DJICameraPlaybackState state) {
			....
			
			Some statements..

			....
		}
	});
~~~

SDK가 재생 상태 정보를 받을 때마다, 정보를 패키지화해서 `DJICameraPlaybackState`객체를 생성해서 SDK에서 app으로 전송한다. 상태 통보(status notifications)에 대해서 좀더 이해하기 위해 아래 테이블에 `DJICameraPlaybackState` 속성을 간략히 설명한다.

<table>
<tbody>
<tr>
<td><b>Attribute</b></td>
<td><b>Description</b></td>
</tr>
<tr>
<td>currentSelectedFileIndex</td>
<td>The index of the currently selected file. Can be used to identify which media file is currently being previewed by the user.</td>
</tr>
<tr>
<td>fileDeleteStatus</td>
<td>Reflects the status of a media file being deleted. There are three possible statuses:
<ul>
<li><b>Media_File_Delete_Failed</b></li>
<li><b>Media_File_Delete_Successed</b></li>
<li><b>Media_File_Deleting</b></li>
</ul>
Developers can use this enum to notify users of deletion progress.</td>
</tr>
<tr>
<td>isAllFilesInPageSelected</td>
<td>Returns whether all the media files in the current page have been selected.</td>
</tr>
<tr>
<td>isFileDownloaded</td>
<td>Notifies users, while the camera is in single preview mode, if the media file they are previewing has already been downloaded. Prevents files from being unintentionally being repeatedly downloaded.</td>
</tr>
<tr>
<td>isSelectedFileValid</td>
<td>Downloading 4K video or DNG images is not allowed. This attribute returns whether a selected file is valid to download.</td>
</tr>
<tr>
<td>mediaFileType</td>
<td>Reflects the file type of the currently previewed media file. There are three types of media files: 
<ul>
<li><b>Media_File_DNG</b></li>
<li><b>Media_File_JPEG</b></li>
<li><b>Media_File_VIDEO</b></li>
</ul>
</td>
</tr>
<tr>
<td>numbersOfMediaFiles</td>
<td>The number of the media files on the SDcard. Useful for displaying the album's basic information.</td>
</tr>
<tr>
<td>numbersOfPhotos</td>
<td>The number of the image files on the SDcard (DNG and JPEG files).</td>
</tr>
<tr>
<td>numbersOfSelected</td>
<td>The number of currently selected files. Can be used to show and confirm the number of files the user wants to delete or download.</td>
</tr>
<tr>
<td>numbersOfThumbnail</td>
<td>The number of the thumbnails that can be downloaded from the SDcard.</td>
</tr>
<tr>
<td>numbersOfVideos</td>
<td>Similar to numberOfPhotos, reflects the number of the video files.</td>
</tr>
<tr>
<td>photoCenterCoordinateX</td>
<td>The x-coordinate of the center of the photo.</td>
</tr>
<tr>
<td>photoCenterCoordinateY</td>
<td>The y-coordinate of the center of the photo.</td>
</tr>
<tr>
<td>photoHeight</td>
<td>The height of the photo.</td>
</tr>
<tr>
<td>photoWidth</td>
<td>The width of the photo.</td>
</tr>
<tr>
<td>playbackMode</td>
<td>An enum for identifying the current playback mode. There are 7 possible values: <b><ul><li>Media_Files_Download</li> <li>Multiple_Media_Files_Delete</li> <li>Multiple_Media_Files_Display</li> <li>Single_Photo_Playback</li> <li>Single_Photo_Zoom</li> <li>Single_Video_Playback_Start</li> <li>Single_Video_Playback_Stop</li></ul></b> UI elements and logical settings should be adjusted according to the current playback mode.</td>
</tr>
<tr>
<td>videoDuration</td>
<td>This attribute is related only to video previews. Shows the length of the video.</td>
</tr>
<tr>
<td>videoPlayPosition</td>
<td>The current position of the video.</td>
</tr>
<tr>
<td>videoPlayProgress</td>
<td>This attribute can be used to update the video progress bar. </td>
</tr>
</tbody>
</table>

 본 것처럼 이 클래스에는 많은 속성이 있다. 이들 중 일부는 미디어 파일에 대한 기본 정보를 포함하며, 다른 정보는 카메라가 무엇을 사용하고 app이 이에 따라서 적절하게 동작하는지 개발자에게 중요한 정보이다. 예를 들면, 개발자는 표시할 UI 앨리먼트를 선택하기 위해서 DJICameraPlaybackState 정보를 이용할 수 있다.

이제 SDK가 제공하는 재생 정보에 알아봤으며 앨범 app을 만들어보자.

## 포토 앨범 app을 위한 UI 컴포넌트

### 1. 다중 프리뷰 재생 모드를 위한 GridView

공식 app인 DJI Pilot에서 카메라가 다중 재생 모드인 경우 app view에서 8개 썸네일이 있다. 아직 DJI Pilot가 [설치하기 위해 여기를 클릭하자](https://play.google.com/store/apps/details?id=dji.pilot&hl=en). 편안한 UX를 달성하기 위해서 개발자가 app에 포함할 수 있는 비슷한 UI 컴포넌트를 제공한다. 첫번째로 구현할  Gridview가 그것이다.(추가 정보는 Android 튜토리얼의 GridView로 [여기를 클릭하자](http://developer.android.com/reference/android/widget/GridView.html))

아래는 다중 미리보기 UI를 위한 대략적인 아웃라인이다. 이 8개 사진 미리보기의 각각은 button이다. 이 button을 GridView로 구현해보자.

![Multiple playback view](../../images/Android/PlaybackAlbumDemo/gridview.jpg)

grid view의 2가지 특징이 있는데 여기서부터 시작해 보자 :

- gird view는 스크롤이 가능하다.
- 각 블록에 있는 객체는 사용자가 디자인할 수 있다.

기본 GridView를 override해서 요구사항을 만족시키자(스크롤이 되지 않는 8개 button으로 된 gridview). 다음 코드에서는 어떻게 GridView를 override해야할지 보여준다. 아주 기본적인 것이라 여러분의 향후 app에는 더 많은 기능을 추가할 것이기 때문에 가벼운 마음으로 보도록 하자.

~~~java
	public class PlaybackGridView extends GridView {
		public PlaybackGridView(Context context) {
			super(context);
		}

		public PlaybackGridView(Context context, AttributeSet attrs) {
			supper(context, attrs);
		}

		public PlaybackGridView(Context context, AttributeSet attrs, int defStyle) {
			super(context, attrs, defStyle);
		}

		@Override
		public boolean dispatchTouchEvent(MotionEvent ev) {
			if (ev.getAction() == MotionEvent.ACTION_MOVE) {
				return true;
			}
			return super.dispatchTouchEvent(ev);
		}
	}
~~~

`dispatchTouchEvent(MotionEvent ev)` 메소드는 gridview의 스크롤할 수 없는 속성을 설정한다. GridView를 override 위해서 이것을 사용하려면 개발자는 item tag로 전체 패스를 사용해야만 한다. 데모 프로젝트에 있는 `activity_playback_protocol.xml` 파일을 에제로 보면 :

~~~xml
	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		android:orientation="vertical">
			<com.dji.download.demo.PlaybackGridView
				android:id="@+id/GridView"
				android:layout_width="fill_parent"
				android:layout_height="fill_parent"
				android:layout_marginTop="57dp"
				android:layout_marginLeft="6dp"
				android:layout_marginRight="6dp"
				android:marginBottom="57dp"
				android:numberColumns="4"
				android:android:verticalSpacing="8dp"
				android:horizontalSpacing="8dp"
				android:stretchMode="columnWidth"
				android:gravity="center"/>
	</RelativeLayout>
~~~

이제 앨범 app를 위해 기본 gridview를 갖게 되었다. 다음으로 'button_gridview_item.xml'는 gridview의 각 엘리먼트를 정의하고 block layout을 제공한다.

~~~xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical" 
	    >
    
	    <Button 
	        android:id="@+id/TransparencyButton"
	        android:layout_width="fill_parent"
	        android:layout_height="110dp"
	        android:background="@android:color/yellow"
	        android:focusable="false"
	        android:focusableInTouchMode="false"
	        />
	</LinearLayout>
~~~

축하한다! 이제 다중 재생 미리보기 앨범을 만드는데 있어서 모든 컴포넌트의 요구사항을 만족한다. 앞으로의 코드는 UI가 좋아 보이도록 이것들을 조합하는 것을 볼 것이다.

~~~java
	public class PlaybackProtocolActivity extends Activity implements OnClickListener {
		private PlaybackGridView mGridView;

		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_playback_protocol);

			onInitActivity();
		}

		private void onInitActivity() {
			mGridView = (PlaybackGridView)findViewById(R.id.GridView);

			ButtonAdapter adapter = new ButtonAdapter(this);
			mGridView.setAdapter(adapter);
		}

		@Override
		protected void onPause() {
			//Pause the AOA service
			ServiceManager.getInstance().pauseService(true);
		}

		@Override
		protected void onResume() {
			//Resume the AOA service
			ServiceManager.getInstance().pauseService(false);
		}

		@Override
		public void onClick(View v)	{
			....
		
			Some statements
		
			....
		}

		class ButtonAdapter extends BaseAdapter {
	        private LayoutInflater inflater;
	        
	        public ButtonAdapter (Context mContext) {
	            super();
	            inflater = LayoutInflater.from(mContext);
	        }
	        
	        @Override
	        public int getCount()
	        {
	            return 8;
	        }
	
	        @Override
	        public Object getItem(int position)
	        {
	            return position;
	        }
		
	        @Override
	        public long getItemId(int position)
	        {
	            return position;
	        }
		
	        @Override
	        public View getView(int position, View convertView, ViewGroup parent)
	        {
	            Button mBtn;
	            final int p = position;
	            if (null == convertView) {
	                convertView = inflater.inflate(R.layout.button_gridview_item, null);
	                mBtn = (Button)convertView.findViewById(R.id.TransparencyButton);
	                convertView.setTag(mBtn);
	            } else {
	                mBtn = (Button)convertView.getTag();
	            }
	            return convertView;
	        }
	        
	    }
	}
~~~

이제 막 우리 app의 framework를 마쳤다. 이제는 `PlaybackProtocolActivity`를 main activity로 설정하기 위해서 `AndroidManifest.xml`에 설정하자. 우리가 한 작업의 결과를 얻기 위해 프로젝트를 실행해보자.

### 2. Camera mode UI 

이 단원을 시작하기 전에, 먼저 약간의 조정이 필요하다:

1. AOA(Android Open Accessory) 지원하도록 설정하기 위해 [Camera App 생성하기](../../Android/FPVDemo/FPVDemo_en.md)를 따라하자.
2. main activity인 `PlaybackProtocolActivity`를 `DJIAoaActivity`로 변경한다.
3. FPV(first personal view)를 보여주기 위해 DjiGLSurfaceView element를 추가한다.(단계 2와 3은 "Camera App 생성하기" 튜토리얼에서 다루고 있다.)
4. `activity_playback_protocol.xml`에 gridview에 `android:visibility="Gone"`을 추가하자.
5. `button_gridview_item.xml`내에 `android:background="@android:color/yellow"`를 `android:background="@android:color/transparent"`로 대체하여 버튼 색상을 조절한다.

이제 계속 진행해 보자. 섹션 1에서 알아본 바와 같이 4개 camera mode가 있고 이 각각에 따라 기능이 달라진다. 개발자는 현재 camera 상태를 기반으로 UI 컴포넌트를 조정해야만 사용자에게 명확하다. `activity_playback_protocol.xml`에 버튼을 추가해 보자.

~~~xml
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
		android:layout_width="match_parent"
		android:layout_height="martch_parent"
		android:orientation="vertical">
			<com.dji.download.demo.PlaybackGridView
				android:id="@+id/GridView"
				.../>
			
			<RelativeLayout 
		        android:layout_width="fill_parent"
		        android:layout_height="40dp"
		        android:layout_alignParentBottom="true">
		        
		        <Button 
		            android:id="@+id/CaptureBtnPlayback"
		            android:layout_width="wrap_content"
		            android:layout_height="wrap_content"
		            android:layout_alignParentLeft="true"
		            android:text="@string/playback_protocol_capture_button_string"
		            android:textSize="14sp"
		            android:visibility="gone"
		            />
		        
		        <Button 
		            android:id="@+id/StartRecordBtnPlayback"
		            android:layout_width="wrap_content"
		            android:layout_height="wrap_content"
		            android:layout_toRightOf="@id/CaptureBtnPlayback"
		            android:text="@string/playback_protocol_strat_record_button_string"
		            android:textSize="14sp"
		            android:visibility="gone"
		            />
		        
		        <Button 
		            android:id="@+id/StopRecordBtnPlayback"
		            android:layout_width="wrap_content"
		            android:layout_height="wrap_content"
		            android:layout_toRightOf="@id/StartRecordBtnPlayback"
		            android:text="@string/playback_protocol_strat_record_button_string"
		            android:textSize="14sp"
		            android:visibility="gone"
		            />
		        
		        <Button
		            android:id="@+id/MultiplePreviewBtnPlayback"
		            android:layout_width="wrap_content"
		            android:layout_height="wrap_content"
		            android:layout_toRightOf="@id/StopRecordBtnPlayback"
		            android:text="@string/playback_protocol_multiple_preview_mode_button_string"
		            android:textSize="14sp"
		            android:visibility="gone"
		            />
		        
		        <Button
		            android:id="@+id/MultiSelectPreviewBtnPlayback"
		            android:layout_width="wrap_content"
		            android:layout_height="wrap_content"
		            android:layout_toRightOf="@id/MultiplePreviewBtnPlayback"
		            android:text="@string/playback_protocol_edition_mode_button_string"
		            android:visibility="gone"
		            android:textSize="14sp" 
		            />
		        
		        <Button
		            android:id="@+id/DownloadBtnPlayback"
		            android:layout_width="wrap_content"
		            android:layout_height="wrap_content"
		            android:layout_toRightOf="@id/MultiSelectPreviewBtnPlayback"
		            android:visibility="gone"
		            android:text="@string/playback_protocol_download_button_string"
		            android:textSize="14sp"
		            />
		        
		        <Button
		            android:id="@+id/DeleteBtnPlayback"
		            android:layout_width="wrap_content"
		            android:layout_height="wrap_content"
		            android:layout_toRightOf="@id/DownloadBtnPlayback"
		            android:visibility="gone"
		            android:text="@string/playback_protocol_delete_button_string"
		            android:textSize="14sp"
		            />
		        
		        <Button 
		            android:id="@+id/PlaybackModeBtnPlayback"
		            android:layout_width="wrap_content"
		            android:layout_height="wrap_content"
		            android:layout_alignParentRight="true"
		            android:text="@string/playback_protocol_playback_mode_button_string"
		            android:textSize="14sp"
		            />
		        
		        <Button 
		            android:id="@+id/RecordModeBtnPlayback"
		            android:layout_width="wrap_content"
		            android:layout_height="wrap_content"
		            android:layout_toLeftOf="@id/PlaybackModeBtnPlayback"
		            android:text="@string/playback_protocol_record_mode_button_string"
		            android:textSize="14sp"
		            />
		        
		        <Button 
		            android:id="@+id/CaptureModeBtnPlayback"
		            android:layout_width="wrap_content"
		            android:layout_height="wrap_content"
		            android:layout_toLeftOf="@id/RecordModeBtnPlayback"
		            android:text="@string/playback_protocol_capture_mode_button_string"
		            android:textSize="14sp"
		            />
		    </RelativeLayout>
	</RelativeLayout>
~~~

**Capture**, **Record** 와 **Playback** 버튼은 항상 나타나야하며 사용자가 이 3개 모드를 전환할 수 있게 한다. 하지만 **Playback** mode는 **Single Preview**, **Multiple Preview** 와 **Multiple Edition** 같은 3개 서브 모드가 있다. **Playback** mode에 들어가면 사용자가 **Single Preview** mode가 된다. **Multiple Preview** mode에서는 이미지를 누르면 **Single Preview** mode에 들어가게 된다. **Multiple Edition** playback mode는 **Multiple Preview**와 달라서 사용자가 파일을 선택, 다운받기, 삭제가 가능하다. 아래 스크린샷을 통해서 각 모드에 들어가는 버튼과 기능에 대해서 대략 살펴볼 수 있다. 상태 다이어그램을 통해서 각 모드 사이의 관계에 대해서 알 수 있다:

![Capture Mode](../../images/Android/PlaybackAlbumDemo/capturemode.jpg)

![Record Mode](../../images/Android/PlaybackAlbumDemo/recordmode.jpg)

![Single Playback Mode](../../images/Android/PlaybackAlbumDemo/singleplayback.jpg)

![Multiple Playback Mode](../../images/Android/PlaybackAlbumDemo/multipleplayback.jpg)

![State switch figure](../../images/Android/PlaybackAlbumDemo/statuschange.jpg)

이제 gridview `onItemClick` 로직은 현재 playback 상태를 기반으로 수정하자. 코드는 다음과 같다 :

~~~java
	private void onStatusChange(final int status) {
        PlaybackProtocolActivity.this.runOnUiThread(new Runnable() {
            public void run() {
                switch (status) {
                    case CAPTURE : {
                        mStartTakePhotoBtn.setVisibility(VISIBLE);
                        mStartRecordBtn.setVisibility(GONE);
                        mStopRecordBtn.setVisibility(GONE);
                        mMultiplePreviewModeBtn.setVisibility(GONE);
                        mMultiSelectPreviewModeBtn.setVisibility(GONE);
                        mDownloadBtn.setVisibility(GONE);
                        mDeleteBtn.setVisibility(GONE);
                        mGridView.setVisibility(GONE);
                        mPlayVideoBtn.setVisibility(GONE);
                        mPauseVideoBtn.setVisibility(GONE);
                        break;
                    }
                    
                    case RECORD : {
                        mStartTakePhotoBtn.setVisibility(GONE);
                        mStartRecordBtn.setVisibility(VISIBLE);
                        mStopRecordBtn.setVisibility(VISIBLE);
                        mMultiplePreviewModeBtn.setVisibility(GONE);
                        mMultiSelectPreviewModeBtn.setVisibility(GONE);
                        mDownloadBtn.setVisibility(GONE);
                        mDeleteBtn.setVisibility(GONE);
                        mGridView.setVisibility(GONE);
                        mPlayVideoBtn.setVisibility(GONE);
                        mPauseVideoBtn.setVisibility(GONE);
                        break;
                    }
                    
                    case PLAYBACK : {
                        mStartTakePhotoBtn.setVisibility(GONE);
                        mStartRecordBtn.setVisibility(GONE);
                        mStopRecordBtn.setVisibility(GONE);
                        mMultiplePreviewModeBtn.setVisibility(VISIBLE);
                        mMultiSelectPreviewModeBtn.setVisibility(GONE);
                        mDownloadBtn.setVisibility(VISIBLE);
                        mDeleteBtn.setVisibility(VISIBLE);
                        mGridView.setVisibility(GONE);
                        break;
                    }
                    
                    case MULTIPLEPLAYBACK : {
                        mStartTakePhotoBtn.setVisibility(GONE);
                        mStartRecordBtn.setVisibility(GONE);
                        mStopRecordBtn.setVisibility(GONE);
                        mMultiplePreviewModeBtn.setVisibility(GONE);
                        mMultiSelectPreviewModeBtn.setVisibility(VISIBLE);
                        mDownloadBtn.setVisibility(VISIBLE);
                        mDeleteBtn.setVisibility(VISIBLE);
                        mGridView.setVisibility(VISIBLE);
						break;
                    }
                }
            }
        });
    }
~~~

이 메소드는 UI 상태를 4개 부분(capture, record, playback과 multipleplayback)으로 분류한다. camera mode가 변경될 때, 이 루틴은 개별 UI 컴포넌트의 visibility를 조정할 수 있다.

UI를 조정하는 것 외에, app에서 drone으로 camera mode를 바꾸라는 요청을 보낼 수 있다. 섹션 1에서 `setCameraMode(CameraMode mode)` 메소드가 있었다. 이것을 사용해 보자 :

~~~java
	private void onPressStatusBtn(final int status) {
        DJIDrone.getDjiCamera().setCameraMode(CameraMode.find(status), new DJIExecuteResultCallback() {
            @Override
            public void onResult(DJIError mErr) {
                if (DJIError.RESULT_OK == mErr.errorCode) {
                    onStatusChange(status);
                }
            }
        });
    }
~~~

The parameter `status` tells this method which mode to switch the camera to. We use the following constants as input parameters.

~~~java
	private final static int CAPTURE = 2;
    private final static int RECORD = 3;
    private final static int PLAYBACK = 4;
    private final static int MULTIPLEPLAYBACK = 5;
~~~

The constants start from '2' because the value of `Camera_Capture_Mode` in the enum `CameraMode` is 2. We edit our `onClick()` function so that `onPressStatusBtn(final int status)` is invoked when the appropriate buttons are pressed, and adjust which constant we pass in depending on which button is pressed:

~~~java
	public void onClick(View v) {
		switch(v.getId()) {
			....
			
			case R.id.CaptureModeBtnPlayback : {
                onPressStatusBtn(CAPTURE);
                break;
            }
			
			case R.id.RecordModeBtnPlayback : {
                onPressStatusBtn(RECORD);
                break;
            }
            
            case R.id.PlaybackModeBtnPlayback : {
                onPressStatusBtn(PLAYBACK);
                break;
            }
            
            case R.id.MultiplePreviewBtnPlayback : {
                DJIDrone.getDjiCamera().enterMultiplePreviewMode(mExecuteCallback);
                onStatusChange(MULTIPLEPLAYBACK);
                break;
            }

			....
		}
	}
~~~

To keep the UI logic consistent, we also detect the current status of the camera in the `onCreate()` function. In our demo, we do this after `checkPermission(Context, DJIGeneralListener)` gets the SDK level.

~~~java
	 new Thread(){
            public void run() {
                try {
                    DJIDrone.checkPermission(getApplicationContext(), new DJIGerneralListener() {
                        
                        @Override
                        public void onGetPermissionResult(int result) {
                            // TODO Auto-generated method stub
                            Log.e(TAG, "onGetPermissionResult = "+result);
                            Log.e(TAG, "onGetPermissionResultDescription = "+DJIError.getCheckPermissionErrorDescription(result));
                            if (result == 0) {
                                //This method require sdk level higher than level-1. Also permission should be got befor invoking this method.
                                DJIDrone.getDjiCamera().getCameraMode(new DJICameraModeCallBack() {
                                    public void onResult(CameraMode mode) {
                                        onStatusChange(mode.value());
                                        if (mode._equals(CameraMode.Camera_PlayBack_Mode.value())) {
                                            DJIDrone.getDjiCamera().setCameraMode(CameraMode.Camera_PlayBack_Mode, mExecuteCallback);
                                        }
                                    }
                                });
                            } else {
                                Log.e(TAG,"Result error.");                     
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

The Inspire 1, Phantom 3 Professional and M100's remote controllers have buttons for capturing, recording and entering the playback mode. Once these buttons have been pressed, the camera status will be changed directly. Would be great if our application could listen to the buttons at the remote controller and automatically adjust the UI?

To listen to the remote controller, we set up `DJIRemoteControllerUpdateAttitudeCallBack`. Add the code below to the onCreate() function, but make sure to declare the variable `mRemoteControllerUpdateAttitudeCallBack` first.

~~~java
	        mRemoteControllerUpdateAttitudeCallBack = new DJIRemoteControllerUpdateAttitudeCallBack() {

            @Override
            public void onResult(DJIRemoteControllerAttitude attitude)
            {
                if (attitude.playbackStatus || attitude.recordStatus || attitude.shutterStatus) {
                    DJIDrone.getDjiCamera().getCameraMode(new DJICameraModeCallBack() {

                        @Override
                        public void onResult(CameraMode mode)
                        {
                            onStatusChange(mode.value());
                        }
                        
                    });
                }
            }
            
        };
        
        DJIDrone.getDjiRemoteController().setRemoteControllerUpdateAttitudeCallBack(mRemoteControllerUpdateAttitudeCallBack);
~~~

`attitude.playbackStatus`, `attitude.recordStatus` and `attitude.shutterStatus` reflect the status of the corresponding buttons on the remote controller. Once the user clicks the corresponding button on the remote controller, these attributes will become `true`.

You have finished implementing the adaptive UI. Build and run your app, and enjoy!

### 3. Album gesture control

For a better user experience, we will be adding gestures to our application. In **Multiple Preview** mode, if the user swipes up or down, the view will display the previous or next page of images. In **Single Preview** mode, if the user swipes left or right, the view will display the previous or next media file. We use GestureDetector to implement this function.

*For a further understanding of the GestureDetector class, follow this link: <http://developer.android.com/reference/android/view/GestureDetector.html>*

~~~java

	private GestureDector mGestureDector;

	mGestureDetector = new GestureDetector(this, new GestureDetector.SimpleOnGestureListener(){ 
           @Override
           public boolean onFling(MotionEvent e1, MotionEvent e2, float velocityX, float velocityY) {
               if (e2.getRawX() - e1.getRawX() > GESTURETHRESHOLD) {
                   DJIDrone.getDjiCamera().singlePreviewNextPage(mExecuteCallback);
                   return true;
               }
               
               if (e1.getRawX() - e2.getRawX() > GESTURETHRESHOLD) {
                   DJIDrone.getDjiCamera().singlePreviewPreviousPage(mExecuteCallback);
                   return true;
               }
               
               if (e1.getRawY() - e2.getRawY() > GESTURETHRESHOLD) {
                   DJIDrone.getDjiCamera().multiplePreviewPreviousPage(mExecuteCallback);
                   return true;
               }
               
               if (e2.getRawY() - e1.getRawY() > GESTURETHRESHOLD) {
                   DJIDrone.getDjiCamera().multiplePreviewNextPage(mExecuteCallback);
                   return true;
               }
               return false;
           }
	});
~~~

If you run your application and try to test this method, you may find that nothing happens. The reason for this is that both the gridview and the gesture detectors will try to handle the user's actions. Both of them try to handle an action at the same time, causing failures. To avoid this situation, we need to override the function `dispatchTouchEvent`

~~~java
	@Override
    public boolean dispatchTouchEvent(MotionEvent event) {
        if (mGestureDetector.onTouchEvent(event))
            return false;
        super.dispatchTouchEvent(event);
        return true;
    }
~~~

Nice! Some predefined gestures have been added to your app and have made your app more user friendly.

![Single swipee gesture](../../images/Android/PlaybackAlbumDemo/singleslide.png)

![Multiple swipe gesture](../../images/Android/PlaybackAlbumDemo/multipleslide.jpg)

### 4. Playing Video

DJI Camera provides a video preview function for users to enjoy their recorded videos. In the current state of our app, users can enter the single preview playback mode and navigate to their video file by swiping left and right. We would also like for them to be able to click a play or pause button to control video playback when previewing a video file. To implement this, first add two buttons to your `activity.xml` file.

~~~xml
	 <RelativeLayout 
        android:layout_width="fill_parent"
        android:layout_height="50dp"
        android:layout_alignParentBottom="true"
        android:layout_marginBottom="40dp"
        >"
        
        <ImageButton
            android:id="@+id/PlayVideoBtnPlayback"
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:layout_alignParentLeft="true"
            android:layout_marginLeft="40dp"
            android:src="@drawable/play_video"
            android:visibility="gone"
            android:background="@android:color/transparent"
            />
        
        <ImageButton 
            android:id="@+id/PauseVideoBtnPlayback"
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:layout_alignParentLeft="true"
            android:layout_marginLeft="40dp"
            android:src="@drawable/pause_video"
            android:visibility="gone"
            android:background="@android:color/transparent"
        />
    </RelativeLayout>
~~~

The image resources for these buttons need to be copied into the `drawable-mdpi` folder, and two elements need to be added into the .xml file `drawable`.

~~~xml
	<item android:drawable="drawable/play_video" android:state_pressed="false"></item>
	<item android:drawable="drawable/pause_video" adnroid:state_pressed="false"></item>
~~~

However, there are two types of media files the user can preview while in single preview playback mode, image files and video files. If the play button or the pause button were to appear while the user were previewing a picture, it would be unnecessary and annoying. To combat this we set up a listener in `onCreate()` to detect which type of file the user is currently previewing.

~~~java
	  mCameraPlaybackStateCallBack = new DJICameraPlayBackStateCallBack(){

            @Override
            public void onResult(DJICameraPlaybackState state)
            {
                mCameraPlaybackState = state;
                    PlaybackProtocolActivity.this.runOnUiThread(new Runnable() {

                        @Override
                        public void run()
                        {
                            // TODO Auto-generated method stub
                            isVideoPreview();
                        }
                        
                    });
            }
            
        };
~~~

`isVideoPreview()` is used to adjust the UI features.

~~~java
	private void isVideoPreview() {
        if (mCameraPlaybackState.mediaFileType.value() == CameraMediaFileType.Media_File_VIDEO.value()) {
            if (mCameraPlaybackState.videoPlayProgress == 0) {
                mPlayVideoBtn.setVisibility(VISIBLE);
                mPauseVideoBtn.setVisibility(GONE);
            } else {
                mPlayVideoBtn.setVisibility(GONE);
                mPauseVideoBtn.setVisibility(VISIBLE);
            }
        } else {
            mPlayVideoBtn.setVisibility(GONE);
            mPauseVideoBtn.setVisibility(GONE);
        }
    }
~~~

Now the app will automatically detect whether the camera is in video single preview playback mode, and remove the play and pause buttons if not.

![Play the video](../../images/Android/PlaybackAlbumDemo/playbutton.jpg)

![Pause the video](../../images/Android/PlaybackAlbumDemo/pausebutton.jpg)

You have successfuly implemented all the UI features that this tutorial has to offer! Give your app another go.

## Downloading and Deleting Media files

### 1. Finishing the Download feature

In previous sections, we discussed and implemented three out of four camera modes. In this section we will cover the final camera mode, **Download Mode**.

Please note that **DNG images and 4k video cannot be downloaded through playback mode**. In addition, the two methods below are not supported by the Inspire 1, Phantom 3 Professional or the M100:

- `fetchMediaData(DJIMedia, DJIExecuteResultCallback)`
- `fetchMediaThumbnail(DJIMedia, DJIExecuteResultCallback)`

As we've already implemented all the necessary UI features, the only thing we have left to do is add some logic to our existing buttons. Add the following code into `onClick(View v)`:

~~~java
	@Override
	public void onClick(View v) {
	....
		
		case R.id.MultiSelectPreviewBtnPlayback : {
	        if (!isEdited) {
	            DJIDrone.getDjiCamera().enterMultipleEditMode(mExecuteCallback);
	            isEdited = true;
	        } else {
	            DJIDrone.getDjiCamera().exitMultipleEditMode(mExecuteCallback);
	            isEdited = false;
	        }
	        break;
	    }
	    
	    case R.id.DeleteBtnPlayback : {
	        if (mCameraPlaybackState.playbackMode._equals(CameraPlaybackMode.Multiple_Media_Files_Display.value())) {
	            DJIDrone.getDjiCamera().deleteAllSelectedFiles(mExecuteCallback);
	            isEdited = false;
	        } else {
	            DJIDrone.getDjiCamera().deleteCurrentPreviewFile(mExecuteCallback);
	        }
	        break;
	    }
	    
	    case R.id.DownloadBtnPlayback : {
	        File destDir = new File(Environment.getExternalStorageDirectory().getPath() + "/Dji_Sdk_Test/");
	        if (!destDir.exists()) {
	            destDir.mkdirs();
	        }
	        if (!mCameraPlaybackState.playbackMode._equals(CameraPlaybackMode.Multiple_Media_Files_Display.value()))
	            DJIDrone.getDjiCamera().downloadCurrentPreviewFile(destDir, mFileDownloadCallBack);
	        else
	            DJIDrone.getDjiCamera().downloadAllSelectedFiles(destDir, mFileDownloadCallBack);
	        break;
	    }
		
	....
	}
~~~

We create a `DJIFileDownloadCallBack` object to be added in the `onCreate()` function, as shown below. `DJIFileDownloadCallBack` includes a handler. You can find the handler declaration in our demo code.

~~~java
	mFileDownloadCallBack = new DJIFileDownloadCallBack() {
            
            @Override
            public void OnStart()
            {
                handler.sendMessage(handler.obtainMessage(SHOWDOWNLOADDIALOG, null));
                
                if(mProgressDialog != null){
                    mProgressDialog.setProgress(0);
                }
                
                handler.sendMessage(handler.obtainMessage(SHOWTOAST, "download OnStart"));
                DJILogHelper.getInstance().LOGD("", "download OnStart",true,false);
            }

            @Override
            public void OnError(Exception exception)
            {
                // TODO Auto-generated method stub
                handler.sendMessage(handler.obtainMessage(CLOSEDOWNLOADDIALOG, null));
                handler.sendMessage(handler.obtainMessage(SHOWTOAST, "download OnError :"+exception.toString()));
                DJILogHelper.getInstance().LOGD("", "download OnError :"+exception.toString(),true,false);
            }
            
            @Override
            public void OnEnd()
            {
                // TODO Auto-generated method stub
                
                handler.sendMessage(handler.obtainMessage(CLOSEDOWNLOADDIALOG, null));
                handler.sendMessage(handler.obtainMessage(SHOWTOAST, "download OnEnd" + mCameraPlaybackState.numbersOfSelected));
                
                DJILogHelper.getInstance().LOGD("", "download OnEnd",true,false);
                
                DJIDrone.getDjiCamera().finishDownloadAllSelectedFiles(new DJIExecuteResultCallback() {
                    
                    @Override
                    public void onResult(DJIError mErr)
                    {
                        // TODO Auto-generated method stub
                        DJILogHelper.getInstance().LOGD("", "download finishDownloadAllSelectedFiles:"+mErr.errorDescription,true,false);
                    }
                });
                
                isEdited = false;
            }

            @Override
            public void OnProgressUpdate(int progress)
            {
                // TODO Auto-generated method stub
                if(mProgressDialog != null){
                    mProgressDialog.setProgress(progress);
                }
                
                DJILogHelper.getInstance().LOGD("", "download OnProgressUpdate progress="+progress,true,false);
            }

        };
~~~

For multiple preview downloads, developers should first invoke `enterMultipleEditMode(DJIExecuteResultCallback)` to enter the multiple edition playback mode so that the users can select the media files they want to delete or download. To support selecting media files and entering single preview playback status, the following code should be added in the `getView(int position, View convertView, ViewGroup parent)` function of `ButtonAdapter`.

~~~java
	mBtn.setOnClickListener(new OnClickListener() {
	
	    @Override
	    public void onClick(View v)
	    {
	        if (!isEdited) {
	            DJIDrone.getDjiCamera().enterSinglePreviewModeWithIndex(p, new DJIExecuteResultCallback() {
	
	                @Override
	                public void onResult(DJIError mErr)
	                {
	                    // TODO Auto-generated method stub
	                    if (mErr.errorCode == DJIError.RESULT_OK) {
	                        onStatusChange(PLAYBACK);
	                    }
	                }
	            });
	            isMultiple = false;
	            
	        } else {
	            DJIDrone.getDjiCamera().selectFileAtIndex(p, mExecuteCallback);
	        }
	    }
	    
	});
~~~

Now try selecting some media files and downloading them!

![Multiple Select](../../images/Android/PlaybackAlbumDemo/multipleselect.jpg)

![Downloading](../../images/Android/PlaybackAlbumDemo/download.jpg)

### 2. Automatically downloading media files

Some developers might want to download the media files from their drone automatically. This subsection will introduce exactly how to do this. First of all, developers should understand that **all the methods involved with downloading are asynchronous, which means some routines should wait for the ack from the drone to continue its execution.**

Here's the code to implement automatic downloading, the developer could use `handler` to execute the `ENTERPLAYBACK` function, then the process will be executed automatically:

~~~java
	private Handler handler = new Handler(new Handler.Callback() {
        
        @Override
        public boolean handleMessage(Message msg)
        {
            switch (msg.what) {
				case STARTAUTODOWNLOAD : {
					DJIDrone.getDjiCamera().setCameraMode(CameraMode.Camera_PlayBack_Mode, new DJIExecuteResultCallback() {
            
			            @Override
			            public void onResult(DJIError mErr)
			            {
			                // TODO Auto-generated method stub
			                if(mErr.errorCode == DJIError.RESULT_OK) {
			                    handler.sendEmptyMessageDelayed(ENTERMULTIPLEPLAYBACK, 2000);
			                }
			            }
			        });
					break;
				}                
                case ENTERMULTIPLEPLAYBACK : {
                    DJIDrone.getDjiCamera().enterMultiplePreviewMode(new DJIExecuteResultCallback() {
                        
                        @Override
                        public void onResult(DJIError mErr)
                        {
                            // TODO Auto-generated method stub
                            if (mErr.errorCode == DJIError.RESULT_OK) {
                                handler.sendEmptyMessageDelayed(ENTERMULTIPLEEDIT, 2000);
                            }
                        }
                    });
                    break;
                }
                
                case ENTERMULTIPLEEDIT : {
                    DJIDrone.getDjiCamera().enterMultipleEditMode(new DJIExecuteResultCallback() {
                        
                        @Override
                        public void onResult(DJIError mErr)
                        {
                            // TODO Auto-generated method stub
                            if (mErr.errorCode == DJIError.RESULT_OK) {
                                handler.sendEmptyMessageDelayed(SELECTFIRSTFILE, 2000);
                            }
                        }
                    });
                    break;
                }
                
                case SELECTFIRSTFILE : {
					// There are a lot of selection methods in our SDK, developers could use them to handle which one they want to download.
                    DJIDrone.getDjiCamera().selectFileAtIndex(0, new DJIExecuteResultCallback() {
                        
                        @Override
                        public void onResult(DJIError mErr)
                        {
                            // TODO Auto-generated method stub
                            if (mErr.errorCode == DJIError.RESULT_OK) {
                                handler.sendEmptyMessageDelayed(DOWNLOADIT, 2000);
                            }
                        }
                    });
                    break;
                }
                
                case DOWNLOADIT : {
                    File destDir = new File(Environment.getExternalStorageDirectory().getPath() + "/Dji_Sdk_Test/");
                    if (!destDir.exists()) {
                        destDir.mkdirs();
                    }
					/** The implementation of mFileDownloadCallBack could be found in the previous tutorial. **/
                    DJIDrone.getDjiCamera().downloadAllSelectedFiles(destDir, mFileDownloadCallBack);
                    break;
                }
            }
            return false;
        }
    });
~~~

`handler.sendEmptyMessage(STARTAUTODOWNLOAD)`를 호출해서 자동으로 미디어 파일을 다운받을 수 있다.

요약하자면, 개발자는 다음과 같은 단계를 통해서 사용자가 여러 파일을 다운받을 수 있게 한다:

1. `setCameraMode`는 camera 상태를 playback mode로 전환시킨다.

2. `enterMultiplePreviewMode`는 여러 미디어 파일을 다운받을 준비를 한다.

3. `enterMultipleEditMode`는 다양한 에디션 playback 상태에 들어가서 사용자가 다운받기를 원하는 이미지를 살펴볼 수 있다.
 
4. 일단 `enterMultipleEditMode`이 성공하고 ack를 반환한다. 사용자 입력을 기반으로 파일을 선택하기 위해서 `selectFileAtIndex`, `selectAllFiles` 과 `selectAllFilesInPage` 함수를 사용한다.

5. 일단 선택 모드가 성공하고 ack를 반환하면 `downloadAllSelectedFiles`를 호출한다.

6. App에서 파일을 성공적으로 다운받았다. `finishDownloadAllSelectedFiles`는 camera가 playback mode로 돌아오도록 하는 ack가 오는 동안 호출되어야 한다.

## 정리

이 튜토리얼의 전체 과정에서 카메라 모드를 전환하고 설정하는 것, 미디어 파일을 다운로드 받고 삭제하는 것, gesture 제어 구현, 사진/비디오 앨범 app 만드는 방법을 배웠다.(Inspire 1, Phantom 3 Professional, M100에 해당)

다음 튜토리얼에서는 Phantom 3 Advanced를 위한 앨범 앱을 구현할 것이다.


