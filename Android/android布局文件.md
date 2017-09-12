#Android布局文件

##获取屏幕的宽度
方法1：

	WindowManager wm = (WindowManager) getContext()
                    .getSystemService(Context.WINDOW_SERVICE);
    int width = wm.getDefaultDisplay().getWidth();
    int height = wm.getDefaultDisplay().getHeight();
 
 方法2：
 
	WindowManager wm = this.getWindowManager();
    int width = wm.getDefaultDisplay().getWidth();
    int height = wm.getDefaultDisplay().getHeight();

##LinearLayout
