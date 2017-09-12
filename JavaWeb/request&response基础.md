#Request&Response
##常见的状态码
		200   ---  ok
		302   ---  与location头一起，实现重定向
		304   ---  资源没变，拿缓存
		404   ---  资源不存在
		500   ---  服务器端错误
##Request

##Response
###设置响应头信息:调用 response.setHeader(name,value)
	addHeader(String,String) 在原有值添加
	setHeader(String,String) 替换原有值
	###使用302+location响应头实现请求重定向
请求重定向: 访问某个资源, 某个资源通知来访者去定位访问另外一个资源.常见的应用场景是, 登录成功的时候使用, 如果用户登录成功,那么让浏览器重新定向到网站的首页去 .
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		String username = request.getParameter("username");
		String password = request.getParameter("password");
		
		if("admin".equals(username)&&"admin".equals(password)){
			//重定向方法1
			response.setStatus(302);
			response.setHeader("location", "/day09_response/index.html");   
			//重定向方法2
			response.sendRedirect("/day09_response/index.html");
			
		}
		
	}
	
###设置refresh头,实现定时刷新
	response.setHeader("refresh", "5;url=http://localhost:8080");
###使用禁止缓存的三个头通知浏览器不要缓存.浏览器很多时候,会对访问的资源进行缓存,这个时候,如果访问的资源是一个实时都会变化的页面,那么浏览器缓存后就看不到实时的最新的值了.所以这个时候要通知浏览器禁止缓存一些页面的内容.通知浏览器禁止缓存可以使用如下的三个头:	
	Cache-control: no-cache	Pragma: no-cache	Expires: 时间值 --- 只需要是当前时间值之前的值就表示 不缓存 
	
	response.setHeader("cache-control", "no-cache");
	response.setHeader("pragma", "no-cache");
	response.setDateHeader("expires", -1);
	
###设置响应体的内容:	有getWriter()  ----------- 字符流	有getOutputStream() ------ 字节流
##Request对象

Request对象代表着一次客户端的http的请求,http请求分为请求行,请求头,请求体,一次请求中所有的数据都会被封装在这个request对象中,其中这个request对象是 服务器在调用Servlet的service方法的时候,传递进来的.也是就是说服务器创建好了这个对象,并且将所有的数据封装到了这个对象中,所以如果要获得来自客户端请求的数据,通通找request.
###Get和POST乱码解决
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		String value = request.getParameter("name");
		byte[] bytes = value.getBytes("ISO8859-1");
		value = new String(bytes,"UTF-8");
	}
	
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("UTF-8");	
		String value = request.getParameter("name");
	}##请求重定向和请求转发的区别
1. 重定向使用response，转发使用request
2. 重定向有2次请求，转发只有一次请求
3. 重定向浏览器地址栏变化，转发地址不变
4. 重定向用户可以看到，而转发是服务器内部行为
5. 重定向可以指向网站上任何资源，而转发只能访问当下应用
		
		response.senRedirect("重定向的地址");
		request.getRequestDispatcher("转发路径").forward(request,response);