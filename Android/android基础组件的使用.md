#Android基础组件的使用
1. EditText
2. Button
3. TextView


##1.EditText
1. 设置样式

文件1:search_et_bg.xml
	
	<?xml version="1.0" encoding="utf-8"?>
	<selector xmlns:android="http://schemas.android.com/apk/res/android">
	    <item android:state_window_focused="false" android:drawable="@drawable/search_et_bg_focused" />
	    <item android:state_focused="true" android:drawable="@drawable/search_et_bg_normal" />
	</selector>

文件2:search_et_bg_focused.xml

	<?xml version="1.0" encoding="utf-8"?>
	<shape xmlns:android="http://schemas.android.com/apk/res/android">
	    <solid android:color="#FFFFFF" />
	    <corners android:radius="3dip"/>
	    <stroke
	        android:width="1dip"
	        android:color="#728ea3" />
	</shape>
	
文件3:search_et_bg_normal.xml

	<?xml version="1.0" encoding="utf-8"?>
	<shape xmlns:android="http://schemas.android.com/apk/res/android">
	    <solid android:color="#FFFFFF" />
	    <corners android:radius="3dip"/>
	    <stroke
	        android:width="1dip"
	        android:color="#BDC7D8" />
	</shape>

2. 布局文件
	
		<EditText
		    android:id="@+id/action_search_bar_et"
		    android:layout_marginLeft="10dp"
		    android:paddingLeft="10dp"
		    android:layout_width="0dp"
		    android:layout_height="35dp"
		    android:layout_weight="100"
		    android:drawableLeft="@drawable/search"
		    android:background="@drawable/search_et_bg"
		    android:textCursorDrawable="@null"        //光标与文字同一颜色
		    android:hint="请输入搜索内容"
		    android:textColor="@color/gray"
		    />
		    
3. 设置左边图片的大小

		Drawable leftDrawable = actionSearchBarEt.getCompoundDrawables()[0];
            if(leftDrawable!=null){
                leftDrawable.setBounds(0, 0, 70, 70);
                actionSearchBarEt.setCompoundDrawables(leftDrawable, actionSearchBarEt.getCompoundDrawables()[1], actionSearchBarEt.getCompoundDrawables()[2], actionSearchBarEt.getCompoundDrawables()[3]);
            }
4. 点击空白的地方失去焦点

		scrollView:空白区域的控件
		actionSearchBarEt:EditText对象
		
		scrollView.setOnTouchListener(new View.OnTouchListener() {
            public boolean onTouch(View v, MotionEvent event) {
                // TODO Auto-generated method stub
                scrollView.setFocusable(true);
                scrollView.setFocusableInTouchMode(true);
                scrollView.requestFocus();
                InputMethodManager imm = (InputMethodManager) getSystemService(SearchMiddleActivity.this.INPUT_METHOD_SERVICE);
                imm.hideSoftInputFromWindow(actionSearchBarEt.getWindowToken(), 0);
                return false;
            }
        });
        
##2.Button
##3.TextView

TextView的一些设置

	if (hotSearchTextViewParams == null){
            hotSearchTextViewParams = new LinearLayout.LayoutParams(
                    LinearLayout.LayoutParams.WRAP_CONTENT, LinearLayout.LayoutParams.WRAP_CONTENT);
            hotSearchTextViewParams.setMargins(10, 10, 10, 10);
        }

        TextView textView = new TextView(this);
        textView.setText(text);
        //设置字体的颜色
        textView.setTextColor(getResources().getColor(R.color.gray));
        //设置Margin
        textView.setLayoutParams(hotSearchTextViewParams);
        //设置圆角与padding
      textView.setBackground(getResources().getDrawable(R.drawable.search_hot_textview_style));

计算宽与高

	TextView tv = getHotTextView(hotStr);
    int spec = View.MeasureSpec.makeMeasureSpec(0, View.MeasureSpec.UNSPECIFIED);
    tv.measure(spec,spec);
    int measuredWidth = tv.getMeasuredWidth();
    int measuredHeight = tv.getMeasuredHeight();

风格文件search_hot_textview_style.xml

	<?xml version="1.0" encoding="utf-8"?>
	<shape xmlns:android="http://schemas.android.com/apk/res/android">
	    <solid android:color="@color/lightgray" />
	    <corners android:radius="5dp" />
	    <padding
	        android:left="5dp"
	        android:top="5dp"
	        android:right="5dp"
	        android:bottom="5dp" />
	</shape>