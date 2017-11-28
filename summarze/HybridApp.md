### 混合APP(Mui和H5+)

1. 使用H5+ geolocation模块获取当前所在位置的经度和纬度，获取的坐标类型有4种,app内使用的地使用gps.js进行经度以及纬度的转化来使定位更加精准

`

	"gps"：表示WGS-84坐标系；
    "gcj02"：表示国测局经纬度坐标系；
    "bd09"：表示百度墨卡托坐标系；
    "bd09ll"：表示百度经纬度坐标系。
`


2. 接收推送消息

`

	//消息点击事件
	plus.push.addEventListener('click',function (msg) {
		
	},false);
	//接收穿透消息
	plus.push.addEventListener('receive',function (msg) {
		var str=JSON.stringify(msg);
	},false);

`

3. 判断开启GPS

`

	function getGEO_status(){
		if(plus.os.name == 'Android'){
			var context = plus.android.importClass("android.content.Context");
			var locationManager=plus.android.importClass("android.location.LocationManager");
			var main=plus.android.runtimeMainActivity();
			var mainSvr=main.getSystemService(context.LOCATION_SERVICE);
			var nogps = mainSvr.isProviderEnabled(locationManager.GPS_PROVIDER);
			if(nogps=== false){
				$.toast('请开启GPS');
			}
		}else if(plus.os.name == 'iOS'){
			var CLLocationManager = plus.ios.import("CLLocationManager");
			var authorizationStatus = CLLocationManager.authorizationStatus();
			switch(authorizationStatus) {
					case 0:
					case 1:
					case 2:
					$.toast('请开启GPS');
					break;
			}
			
		} 
	}

`

4. 设置应用屏幕常亮

`

	plus.device.setWakelock(true);

`

5. 设置APP后台自运行

`

	var g_wakelock = null;
	//允许程序后台运行，以持续获取GPS位置
	function wakeLock() {
	    //Android
	    debugger
	    var main = plus.android.runtimeMainActivity();
	    var Context = plus.android.importClass("android.content.Context");
	    var PowerManager = plus.android.importClass("android.os.PowerManager");
	    var pm = main.getSystemService(Context.POWER_SERVICE);
	    g_wakelock = pm.newWakeLock(PowerManager.PARTIAL_WAKE_LOCK, "ANY_NAME");
	    g_wakelock.acquire();
	}

	//结束程序后台运行
	function releaseWakeLock () {
	    if(g_wakelock != null && g_wakelock.isHeld()) {
	        g_wakelock.release();
	        g_wakelock = null;
	    }
	}

	document.addEventListener("pause", onAppPause, false);
	function onAppPause(){
		if(plus.os.name == 'Android'){
			wakeLock();
			//time=setInterval(function(){getPos();},1000);
		}
		//getPos();
	}
	document.addEventListener("resume", onAppReume, false);
	function onAppReume(){
		if(plus.os.name == 'Android'){
			releaseWakeLock();
			//clearInterval(time);
		}
	}

`

6. 点击图片。图片放大显示。使用mui.previewimage.js
