Android Kernel Vs Linux Kernel
안드로이드 커널은 전형적인 리눅스 커널에서 찾을 수 없는 몇 가지 추가 기능을 포함하고 있다. 그것은 Binder IPC와 low-memory keller이다. 그러나 이것을 제외하고는 안드로이드 커널은 여전히 리눅스 커널이라고 말할 수 있다.

안드로이드 커널이 위의 몇 가지 기능을 제외하고 리눅스 커널과 같다는 것은 커다란 이점이 있다. 그것은 리눅스 커널에 이미 특정 리눅스 드라이버가 존재 한다면, 안드로이드 드라이버가 있다는 것과 동일한 말이다.

카메라, 오디오, 비디오, 버튼 그리고 터치스크린 등등... 이미 존재하는 리눅스 드라이버들을 가져다 이용하는 것은 안드로이드 지원을 더욱 쉽게 만들어 준다.

그러나 전형적인 리눅스 배포판이 어플리케이션들에 많은 다른 디바이스 파일들(/dev filesystem 안에 직접 파일 열기로) direct access를 허용하는 것과는 다르게, 안드로이드는 하드웨어를 직접 접근하는 것을 제한한다.

예를 들어 서로 다른 안드로이드 앱들이 음악 재생 이나 녹음을 위해 오디오 기능을 사용한다면, 안드로이드 아래에 리눅스 커널은 Advanced Linux Sound Archtecture(ALSA) 오디오 드라이버를 통해 이 오디오 기능을 제공한다. 대부분의 경우 한번에 하나의 프로세스만 ALSA 드라이버 자원을 열고 컨트롤 할 수 있다. 만약 각각의 앱들이 ALSA 드라이버 리소스에 접근을 하고 사용을 한다면, 문제가 발생할 것이다. 그 중 하나의 앱이 쉽게 ALSA 드라이버 리소스를 갖을 수 있다면, 그리고 다른 앱들의 접근을 막는다면... 문제가 커질것이다. 이 문제를 해결하기 위해 안드로이드는 managers를 사용 한다.

 

Android managers
매니져들은 모든 app들을 대신 해서 하드웨어 디바이스들을 컨트롤 하는 시스템의 구성 요소이다. 매니져들은 각각의 오디오/비디오/통신 등등의 리소스들과의 인터페이스와 할당을 책임지고, 앱이 그 리소스를 사용할 권한이 있는지를 정의 한다.

리소스를 사용하기 위해서, 입은 반드시 android.content.Context class의 getSystemService() 매써드를 통해 알맞은 매니져를 참조 생성해야 한다.

// Create a reference to the system "location" manager

LocationManager locationManager = (LocationManager) 

  mContext.getSystemService(LOCATION_SERVICE); 
그리고 나서, 이 매니져 레퍼런스를 통해 정보를 만들고 요청들을 컨트롤한다.

// Query the location manager to determine if GPS is enabled

isGPSEnabled = locationManager.

isProviderEnabled(LocationManager.GPS_PROVIDER);
앱들은 Java Android API를 이용해서 매니져들과 상호작용을 한다.

그리고 매니져들은 이런 Java 매써드에 응답하는 동안에, 하드웨어와 직접적으로 상호작용을 하는 native code를 호출하기 위해 결국은 Java native interface(JNI)를 사용해야만 한다. 여기서, 안드로이드 API와 native code를 호출하는 것 사이의 다리는 hardware abstraction layer(HAL)로 알려져 있다.
