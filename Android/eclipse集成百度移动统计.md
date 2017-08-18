#Eclipse集成百度移动统计
##申请APPKEY
申请地址:<http://mtj.baidu.com/web/welcome/login>
##添加jar包到项目
0. 讲jar包覆之到工程lib文件夹下
1. 点击工程，选择 properties
2. 在左边选择 java build path
3. 在主窗口选择 libraries
4. 点击 addjar 选择jar包

##配置AndroidManifest.xml
###添加权限

	<uses-permission android:name="android.permission.INTERNET" />  
	<uses-permission  android:name="android.permission.ACCESS_NETWORK_STATE" />
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
	<uses-permission android:name="android.permission.READ_PHONE_STATE" />
	<uses-permission android:name="android.permission.WRITE_SETTINGS" />
	<uses-permission  android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
	<uses-permission  android:name="android.permission.ACCESS_FINE_LOCATION" />
	<uses-permission android:name="android.permission.GET_TASKS" />
	<uses-permission android:name="android.permission.BLUETOOTH"/>      	<uses-permission  android:name="android.permission.READ_EXTERNAL_STORAGE" />	<uses-permission  android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"  />	<uses-permission  android:name="android.permission.ACCESS_FINE_LOCATION" /> 
	
###配置meta-data

	 <application
	 	……………………    #其它的配置
	 
        <meta-data
            android:name="BaiduMobAd_STAT_ID"
            android:value="这个地方填写你申请的key" />
        <!-- 渠道商编号 -->
        <meta-data
            android:name="BaiduMobAd_CHANNEL"
            android:value="Baidu Market" />

        <!-- 是否开启错误日志统计，默认为false -->
        <meta-data
            android:name="BaiduMobAd_EXCEPTION_LOG"
            android:value="true" />
        <!-- 日志发送策略，可选值：APP_START、ONCE_A_DAY、SET_TIME_INTERVAL，默认为APP_START -->
        <meta-data
            android:name="BaiduMobAd_SEND_STRATEGY"
            android:value="APP_START" />
        <!-- 日志发送策略 为SET_TIME_INTERVAL时，需设定时间间隔(取消下行注释)。取值为1-24的整数，默认为1 -->
        <!-- <meta-data android:name="BaiduMobAd_TIME_INTERVAL" android:value="2" /> -->
        <!-- 日志仅在wifi网络下发送，默认为false -->
        <meta-data
            android:name="BaiduMobAd_ONLY_WIFI"
            android:value="false" />
        <!-- 是否获取基站位置信息 ,默认为true -->
        <meta-data
            android:name="BaiduMobAd_CELL_LOCATION"
            android:value="true" />
        <!-- 是否获取GPS位置信息，默认为true -->
        <meta-data
            android:name="BaiduMobAd_GPS_LOCATION"
            android:value="true" />
        <!-- 是否获取WIFI位置信息，默认为true -->
        <meta-data
            android:name="BaiduMobAd_WIFI_LOCATION"
            android:value="true" />
    </application>
	
#在代码中加入统计
##统计开启的次数
在程序AndroidManifest.xml中定义了android.intent.action.MAIN的Activity中的onCreate()方法中，添加如下代码：

	StatService.start(this);
##统计Activity页面
在onResume方法中添加如下代码：
	StatService.onResume(this);
在onPause方法中添加如下代码：

	StatService.onPause(this);


**注：统计哪个Activity则在哪个Activity上面加如上的代码**##统计Fragment页面

在onResume方法中添加如下代码：
	StatService.onPageStart(getActivity().getApplicationContext(),"自己定义一下Fragment的名称");
在onPause方法中添加如下代码：

	StatService.onPageEnd(getActivity().getApplicationContext(),"自己定义一下Fragment的名称");
	
**注：统计哪个Fragment则在哪个Fragment上面加如上的代码**
**注：以上是精简的集成方法**


