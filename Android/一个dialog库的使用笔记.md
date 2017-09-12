#一个dialog库的使用笔记
##0.说明
我在github上发现了一个特别屌的库链接为<https://github.com/jjdxmashl/jjdxm_dialogui>，这个库基本上可以满足所有关于弹窗的功能。

##1.AS集成方法
Gradle中添加
	
	compile 'com.dou361.dialogui:jjdxm-dialogui:1.0.3'	#1.0.3是当前的版本号
##2.使用
###公用的变量
	
	Activity mActivity = this;
    Context mContext = getApplication();
   
**备注：若使用toast的话则，要先执行如下代码**
	
	DialogUIUtils.init(mContext);
	
