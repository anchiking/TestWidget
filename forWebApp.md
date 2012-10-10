----------

# 1. SKT Runtime 기본구조 

----------


## 1.1 공통 Runtime 기본구조 

> 여기에 메인 구조 그림이 들어갈 것이다. 
![Runtime 기본구조](./images/runtime1.png)
-	SKT Runtime은 크게 platform 별 Native Layer와 Java Script Layer의 2가지의 layer로 나뉘어 진다. 

-	Native layer는 각 플랫폼 별 Webview를 포함하여 사용한다. 

-	Native layer는 각 플랫폼 별 사용되는 언어로 개발되어 있으며 JavaScript layer에서 요청된 DeviceAPI 를 플랫폼 SDK를 이용하여 처리된다.  

<br> 

### 1.1.1 Runtime JavaScript layer 

-	Java Script layer는 웹앱에서 사용하기 위한 JavaScript API들을 선언 및 정의 하며 이를 Runtime의 native layer와 연결하는 역할을 한다.

-	**SRT-x.x.js** 형태로 배포 되며 Runtime이 업데이트 될때마다 버전명이 올라간다. 
		> **2012.10.08** 기준 **SRT-1.0.js** 배포 

-	**SRT-x.x.js** 는 크게 두가지 역할로 구분된다. 

	-	DeviceAPIs : 웹앱에서 사용하는 JavaScript API ProtoType 정의 
	-	exec module: JavaScript APIs 를 Native Runtime Library와 연결하여 통신하는 공통 인터페이스 모듈 
		-	js prompt()함수를 native 단에서 재정의 하여 JavaScript와 native의 연결 통로가 된다. 

<br>

----------

## 1.2 Android Runtime 기본 구조 
<br>
### 1.2.1 Android Native Layer 

-	Android Webview를 이용하여 Runtime JavaScript Library에서 호출된 API를 그에 해당하는 Native Class를 실행하고 성공/실패 콜백 전달하는 모듈

-	WebApp에서 Device의 특정 기능을 수행 및 webview를 사용하기 위하여 **Runtime.jar** 가 배포된다. 

	-	Runtime.jar는 Java class로 구성되어 있는 Android용 native library 이다. 
	- 	Runtime.jar는 web app의 index.html을 load하는 파일을 포함 하고 있다. 
	- 	Runtime.jar는 SKT Runtime에서 제공하는 DeviceAPI가 포팅되어 있다. 
	- 	Runtime.jar는 Native code로 모듈화 되어 Android SDK 로 부터 작업을 수행하고 결과를 JavaScript layer에 반환 한다. 


<br>

### 1.2.2 Android Project Files 

-	Android용 webapp을 개발하기 위해서는 아래 와 같은 project file 들을 세팅하여야 한다. 

	1) AndroidManifest.xml
	-	WebApp의 고유한 **package** 명 설정 

	2) res/value/string.xml 
	-	 실제 단말에서 보여지는 **WebApp의 이름** 설정 

	3)  res/drawable/icon.png
	-	 실제 단말에서 보여지는 **아이콘** 설정 


<br>

----------

## 1.3 IOS Runtime 기본 구조 

### 1.3.1 IOS Native Layer

-	 블라블라블라

### 1.3.2 IOS Project Files
-	블라블라블라 

<br>



----------


# 2. SKT Runtime 웹앱 개발

----------

## 2.1 Android Runtime 웹앱 개발 환경 

 - Authoring Tool을 이용하여 Android 용 웹앱 개발환경을 다운로드 받을 수 있다. 
 - [여기에 링크가 들어간다.](http://) 


### 2.1.1 Android Runtime 웹앱 개발 환경 구조 

> 여기에 그림이 들어간다. 

1) src - plugin 개발시에 작성하는 Java Native Code

2) asset - 실제 웹앱의 웹 리소스(HTML/CSS/JS/IMG) 와 Runtime JavaScript Library 가 저장되는 위치 
	
-	**assets/www** : 웹앱의 저장 위치 
-	**assets/www/index.html** : 웹앱의 첫 실행 파일 

3) libs - Webview 를 이용하여 Device 의 단말 접근 기능을 제공하는 DeviceAPI가 포팅된 Android library 위치 

-	**Runtime.jar** : webview 및 DeviceAPI가 포팅된 JAVA library

4) drawable - device에 보여지는 icon 및 splash image를 저장하는 위치 

-	**icon.png** : device에 보여지는 icon
-	**splash.png** : 웹앱 실행 초기에 보여지는 splash image (optional) 

5) values , xml - 웹앱의 name 및 runtime setting을 할 수 있는 폴더 

-	**value/string.xml** : 웹앱의 name을 세팅하는 파일 
-	**xml/config.xml** : device의 orientation(portrait , landscape , audo) 및 splash image를 세팅 하는 파일 

6) AndroidManifest.xml : 하나의 Native Application으로써의 고유한 Package 명을 지정하는 파일 

<br>

### 2.1.2 Android 웹앱 개발 절차

**step 1.**  Authoring tool을 이용해 Android Web App Template를 받으면 Lib 폴더에 **Runtime.jar가 기본으로 포함**되어 있다.

> image area

**step 2.**  Web App 개발자는 assets 폴더 내부에 .html , .js , .css 와 같은 webapp을 작성하여야 하며 시작 파일은 반드시 index.html 이어야 한다. **SRT-1.0.js는 template 기본 파일**이다. 

> image area

-	특정 웹페이지 내부에서 DeviceAPI를 사용하고 할 때에는 아래와 같이 JavaScript Library를 선언한다.
 
	`<script type="text/javascript" charset="utf-8" src=*../path/SRT-1.0.js"></script>`

	> example 1. 현재 wifi가 연결되었는지를 판단하는 sample 

		function validSuccessCalback_DS(prop,value) {
		alert("The aspect::" + prop.aspect + "  property::" + prop.property + " is " + value);
		if(value == "undefined")
			alert("현재 wifi로 연결 되어 있지 않음");
		}

		function validErrorCallback_DS(response){
		alert("다음 error: " +  response.message + ", 발생");
		}

		navigator.devicestatus.getPropertyValue(validSuccessCalback_DS ,validErrorCallback_DS ,{aspect:"WiFiNetwork", property:"ssid"});

**step 3.**  Res 폴더 내부에 웹앱 개발자가 사용하고자 하는 아이콘 이미지를 **icon.png**의 이름으로 삽입한다. 

> image area

**step 4.** **AndroidManifest.xml** 파일 내부의 package 명을 개발자가 변경을 해줘야 한다. 이는 고유한 값으로 Android 시스템 내부의 중복된 어플이 설치되지 않도록 한다. 

> image area

	<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:windowSoftInputMode="adjustPan"
    package="co.kr.skt.testapp.test" android:versionName="1.1" android:versionCode="5">

**step 5.** Application의 실제 단말기에서 보여지는 이름을 **string.xml의 app_name**에 정의한다.

> image area

	<resources>
  	<string name="app_name">테스트웹앱</string> 
	</resources>

**step 6.** **res/xml/config.xml** 의 orientation , splashscreen preference를 이용하여 웹앱의 방향(가로/세로)과 splash image(html 이 로딩 되기 전에 보여지는 image)를 세팅 할 수 있다. 이는 선택 사항으로 웹앱 개발자가 작성하지 않으면 default 값을 따르게 된다. 

> image area

	1) 웹앱의 방향(default 는 auto)

		- 가로 방향
		<preference name="orientation" value="landscape"/>

		- 세로 방향
		<preference name="orientation" value="portrait"/>

		- 자동 	
		<preference name="orientation" value="auto"/>

	2) splash image(default 는 보여지지 않음) 

		- value값인 splashskt.png가 res/drawable 폴더 내부에 존재 하여야 한다.
		> image area
		<preference name="splashscreen" value="splashskt"/>
		

**step 7.** Eclipse의 빌드 아이콘을 이용하여 Android App을 빌드하여 Device에 정상적으로 설치됨을 확인한다. 

> image area

**step 8.** Device에 WebApplication이 정상적으로 출력됨을 확인한다. 

> image area 


----------


## 2.2 iOS Runtime 웹앱 개발 환경

### 2.2.1 iOS Runtime 웹앱 개발 환경 구조  

### 2.2.2 iOS 웹앱 개발 방법