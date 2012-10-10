----------
# 1. SKT Runtime 기본구조 

----------


## 1.1 공통 Runtime 기본구조 

> 여기에 메인 구조 그림이 들어갈 것이다. 
![SKT Runtime 구조](./images/runtime1.png)

-	SKT Runtime은 크게 platform 별 Native Layer와 Java Script Layer의 2가지의 layer로 나눠 진다. 

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
#	2. SKT Runtime 확장 구조 개발 

----------

## 2.1. 개요 

-	WebApp은 Device Platform (Android, iOS..)에서 제공하는 Native SDK API를 사용할 수 없다. 따라서 WebApp에서 단말 기능을 사용하고자 한다면 Runtime에서 제공하는 JavaScript Extension API를 사용하여 한다. 이를 가능하게 하는것이 SKT Runtime 이며 , 이 SKT Runtime 내부에서 HTML5 Spec을 준수하는 API (contact , calendar, file .. etc) 와 추가적인 Additional API (audio, childBrowser..etc) 를 제공한다. 

	[여기는 DeviceAPI Spec 링크가 걸린다. ](http://)

-	통신사 별 과금 처리와 같은 추가적인 API 를 WebApp에서도 사용하고자 한다면 선 개발된 SKT Runtime에 Plugin 형태로 추가 개발 할 수 있다.  마찬가지로 SKT Runtime에서 제공하지 않는 단말 접근 기능을 개발하고자 한다면 이역시 plugin 형태로 개발 할 수 있다. 

##	2.2 Android Runtime 확장 구조 개발 

### 2.2.1 Android Runtime 확장 구조 개발 환경

> 여기에 그림이 들어간다. 

1) src - plugin 개발시에 작성하는 **Java Native Code**

2) asset - 실제 웹앱의 웹 리소스(HTML/CSS/JS/IMG) 와 Runtime JavaScript Library 가 저장되는 위치 
	
-	**assets/www** : 웹앱의 저장 위치 
-	**assets/www/index.html** : 웹앱의 첫 실행 파일 

3) libs - Webview 를 이용하여 Device 의 단말 접근 기능을 제공하는 DeviceAPI가 포팅된 Android library 위치 

-	**Runtime.jar** : webview 및 DeviceAPI가 포팅된 JAVA library

4) drawable - device에 보여지는 icon 및 splash image를 저장하는 위치 

-	**icon.png** : device에 보여지는 icon
-	**splash.png** : 웹앱 실행 초기에 보여지는 splash image (optional) 

5) values , xml - 웹앱의 name 및 runtime setting을 할 수 있는 폴더 

-	**plugin.xml** : plugin을 Runtime에 등록하는 파일 
-	**value/string.xml** : 웹앱의 name을 세팅하는 파일 
-	**xml/config.xml** : device의 orientation(portrait , landscape , audo) 및 splash image를 세팅 하는 파일 

6) AndroidManifest.xml : 하나의 Native Application으로써의 고유한 Package 명을 지정하는 파일 

<br>

### 2.2.2 Android Runtime 확장 구조 개발 절차 

**step 1.**  Authoring tool을 이용해 Android Web App Template를 받으면 Lib 폴더에 **Runtime.jar가 기본으로 포함**되어 있다.

> image area

**step 2.**  확장 구조 개발자는 아래의 그림과 같이 src 폴더의 .java 파일에 추가할 native 모듈에 대한 정의를 한다. 

> image area

- src 폴더를 생성한다. 

- package 명을 지정한다. 이는 plugin.xml에 입력될 값이다. 

	> co.kr.skr.plugin	

- native 모듈을 개발을 위한 .java 파일을 생성한다. 

	-	Plugin class 를 **extends** 한다. 
	-	**execute** 함수를 재정의 한다. 

	> example code 

		public class pluginEx extends Plugin{
		
			@Override
			public PluginResult execute(String arg0, JSONArray arg1, String callbackId) {
				
				// TODO Auto-generated method stub
				
				return null;
			}
		}

-	execute 함수 내부에 native api를 이용하여 특정 기능을 구현 한다. 

	-	return 형은 PluginResult 이다. PluginResult 객체에 status와 필요하다면 JavaScript에 전달 할 message를 입력한다. 
	
		1) 특정 기능이 성공적으로 완료된 경우 (JavaScript SuccessCallback 호출)  

		`return new PluginResult(Status.OK, returnObj);`

		2) 특정 기능이 실패 한 경우 (JavaScript ErrorCallback 호출)  

		`return new PluginResult(Status.ERROR, errorObj);`

		3) 별도의 JavaScript 알림이 필요 없는 경우 

		`return new PluginResult(Status.NO_RESULT);`

 
	> example code (MacAddress plugin)

			@Override
			public PluginResult execute(String action, JSONArray args, String callbackId) {
					
				this.callbackId = callbackId;
				
				//action은 JavaScript에서 정의한 extension api 명이다. 
	
				//device의 MacAddress를 얻는 기능 
				if(action.equalsIgnoreCase("getMacAddress")){		
					WifiInfo wifiInfo = wifiManager.getConnectionInfo();
					String macAddress = wifiInfo.getMacAddress();
					
					PluginResult result = new PluginResult(PluginResult.Status.OK, macAddress);
					return result;
				}
				else if(action.equalsIgnoreCase("getDeviceID")){		
					String android_id = Secure.getString(ctx.getContentResolver(), Secure.ANDROID_ID);
					
					PluginResult result = new PluginResult(PluginResult.Status.OK, android_id);
					return result;
				}
				//device의 개통된 번호를 얻는 기능 
				else if(action.equalsIgnoreCase("getPhoneNumber")){
					String phoneNumber = tm.getLine1Number();
					return new PluginResult(PluginResult.Status.OK, phoneNumber);
				}
				
				return new PluginResult(PluginResult.Status.NO_RESULT);
			}

-	**setContext** 함수를 재정의 하여 plugin 로딩시 초기화 작업 및 API가 호출되기 전의 특정 작업을 수행 할 수 있다. 

	> example code (MacAddress plugin)

			@Override
			public void setContext(RuntimeInterface ctx) {
				// TODO Auto-generated method stub
				super.setContext(ctx);
				
				wifiManager = (WifiManager)ctx.getSystemService(Context.WIFI_SERVICE);
				tm = (TelephonyManager) ctx.getSystemService(Context.TELEPHONY_SERVICE);
			}

-	**isSynch** 함수를 재정의 하여 특정 API에 대하여 동기적으로 동작하게 함수를 개발 할 수 있다. 특정 api에 대하여 재정의 하지 않는다면 API는 비동기로 동작한다. 

	> example code (MacAddress plugin)

			@Override
				public boolean isSynch(String action) {
					//getMacAddress api는 동기적으로 동작 
					if(action.equals("getMacAddress"))
						return true;
					else if(action.equals("getDeviceID"))
						return true;
					else if(action.equals("getPhoneNumber"))
						return true;
					
					return super.isSynch(action);
				}

- **execute** 함수의 3번째 매개변수(callbackId)를 이용하여 JavaScript callback에 Native에서 pluginResult 객체를 지속적으로 return 할 수 있다. 

	> example code ( Plugin.success() api를 이용하여 callback을 보내는 방법) 
		
		@Override
		public PluginResult execute(String action, JSONArray args, String callbackId) {
		JSONObject obj = args.optJSONObject(0);	

			if(action.equalsIgnoreCase("for")){
	
				int end = obj.optInt("end");		
				int i = obj.optInt("start");
	
				//현재 action에 대한 callback ID를 내부에 저장한다. 
				this.callbackId = callbackId;
				threadstop = false;
	
				do{
					i++;
					if(i%1000 == 0){
						PluginResult result = new PluginResult(PluginResult.Status.OK, i);

						// result를 setKeepCallback(true) 로 설정한다. 
						// 위와 같이 설정 하면 JS library에서 callback을 delete 하지 않는다. 
						// default는 false이다. 
						result.setKeepCallback(true);	
					
						// action에 대한 calbakcId에 result를 지속적으로 보낼 수 있다. 
						this.success(result, this.callbackId);
					}
					else if(i == end){
						threadstop = true;
						PluginResult result = new PluginResult(PluginResult.Status.OK, "stop");
					
						// result를 setKeepCallback(false) 로 설정한다
						result.setKeepCallback(false);			
						// action에 대한 calbakcId에 result를 마지막으로 보낸다.
						// result의 keepCallback이 false이기 때문에 JS lib에서 callback을 delete한다. 
						this.success(result, this.callbackId);
					}	
	
				}while(!threadstop);
			}
	
			return new PluginResult(PluginResult.Status.NO_RESULT);
		}

**step 3.**  확장 구조 개발자는 아래의 그림과 같이 asset 폴더의 .js 파일에 추가할 JavaScript 모듈에 대한 정의를 한다. 

> image area

-	asset 폴더 내에 JavaScript plugin 을 위한 .js 파일을 생성한다. 

-	JavaScript plugin 객체를 생성한다.
 
	> code example

	`var MacAddress = function() {};`

-	JavaScript Extension API 의 prototype을 정의 한다. 

	> code example 1 (SuccessCallback, ErrorCallback , 매개변수가 없는 api) 

		MacAddress.prototype.getMacAddress = function() {
		//do sync
		var macAddress = srt.exec(null, null, 'MacAddress', 'getMacAddress', []);
		return macAddress;
		};
	
	> code example 2 (SuccessCallback, ErrorCallback , 매개변수가 있는 api) 

		MacAddress.prototype.list = function(successCallback, failureCallback,params) {
		srt.exec(successCallback, failureCallback, 'MacAddress', 'list', [ params ]);
		};

- 	JavaScript plugin 객체를 SKT Runtime JavaScript Library의 plugin으로 등록 한다. 

	> code example 

		srt.addConstructor(function() {
			//window.plugin.MacAddress 로 javascript 에 노출 된다. 
			srt.addPlugin('MacAddress', new MacAddress());
		});


> Full Example (MacAddressPlugin.js) 
	
	var MacAddress = function() {};
	
	MacAddress.prototype.getMacAddress = function() {
		//do sync
		var macAddress = srt.exec(null, null, 'MacAddress', 'getMacAddress', []);
		return macAddress;
	};
	
	MacAddress.prototype.list = function(successCallback, failureCallback,params) {
		srt.exec(successCallback, failureCallback, 'MacAddress', 'list', [ params ]);
		};
	
	MacAddress.prototype.getDeviceID = function() {
		//do sync
		var deviceID = srt.exec(null, null, 'MacAddress', 'getDeviceID', []);
		return deviceID;
	};
	
	srt.addConstructor(function() {
		srt.addPlugin('MacAddress', new MacAddress());
	});

**step 4.**  플러그 인을 Runtime에 등록하고자 할 때에는 개발한 plugin 이름과 실제 구현된 java파일 위치를 **res/xml/plugin.xml** 파일에 작성한다.

> image area

-	name 은 JavaScript 등록을 위해 srt.addPlugin() 에 등록되었던 이름이다. 
-	value 는 실제 Java 파일이 작성된 package의 full path 이다. 

	>	plugin.xml 

   	 `<plugin name="MacAddress" value="co.kr.skr.plugin.MacAddressPlugin" />`

	 `<plugin name="CallLog" value="org.skt.runtime.plugin.CallLogPlugin" />`
	

**step 5.** 웹앱의 시작 파일은 반드시 index.html 이어야 한다. **SRT-1.0.js는 template 기본 파일**이다. 

> image area

-	특정 웹페이지 내부에서 DeviceAPI를 사용하고 할 때에는 아래와 같이 JavaScript Library를 선언한다.
 
	`<script type="text/javascript" charset="utf-8" src=*../path/SRT-0.1.js"></script>`

-	특정 웹페이지 내부에서 추가된 Plugin을 사용하고 할 때에는 아래와 같이 JavaScript Library를 선언한다.

>	Example 1. MacAddress Plugin 사용 예제 
 
		<script type="text/javascript" charset="utf-8" src="../path/macaddressplugin.js"></script>
		<script type="text/javascript">
		function getMacAddress(){
			var macAddress = window.plugins.MacAddress.getMacAddress();
			alert("MacAddress = " + macAddress);
		}
		function getPhoneNumber(){
			var phoneNumber = window.plugins.MacAddress.getPhoneNumber();
			alert("phoneNumber = " + phoneNumber);
		}
		function getDeviceID(){
			var id = window.plugins.MacAddress.getDeviceID();
			alert("Device id = " + id);
		}
		</script>

>	Example 2. CallLog Plugin 사용 예제 

		<script type="text/javascript" charset="utf-8" src="../path/calllogplugin.js"></script>
		<title>CallLog Test</title>
		<script type="text/javascript">
		
		function calllogSC(obj){
			if (obj.length > 0) {
					alert(obj[0].number);
					alert(obj[0].duration);
				} else {
					alert("empty call log");
				}
		}
		function errorcallback(e) {
			alert(e);
		}
		function getCallLogs() {
			window.plugins.CallLog.list(calllogSC, errorcallback, "day");
		}
		</script>

**step 6.**  Res 폴더 내부에 웹앱 개발자가 사용하고자 하는 아이콘 이미지를 **icon.png**의 이름으로 삽입한다. 

> image area

**step 7.** AndroidManifest.xml 파일 내부의 package 명을 개발자가 변경을 해줘야 한다. 이는 고유한 값으로 Android 시스템 내부의 중복된 어플이 설치되지 않도록 한다. 

> image area

	<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:windowSoftInputMode="adjustPan"
    package="co.kr.skt.testapp.test" android:versionName="1.1" android:versionCode="5">

**step 8.** Application의 실제 단말기에서 보여지는 이름을 **string.xml의 app_name**에 정의한다.

> image area

	<resources>
  	<string name="app_name">테스트웹앱</string> 
	</resources>

**step 9.** Eclipse의 빌드 아이콘을 이용하여 Android App을 빌드하여 Device에 정상적으로 설치됨을 확인한다. 

> image area

**step 10.** Device에 WebApplication이 정상적으로 출력됨을 확인한다. 

> image area 
