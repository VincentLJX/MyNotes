#Servlet
##Servlet有生命周期的方法
只产生一个对象，在第一次访问的时候，创建对象
- Init

		第一次访问, 创建serlvet的时候会执行, 用来 初始化 serlvet- Service
		每次请求的时候, 都会执行- Destroy		当servlet销毁, 不再服务客户端请求的时候销毁
	**小结: **一个servlet第一次被访问的时候,会创建出来,调用init 初始化,然后这个servlet类的一个实例对象就驻留在内存中,后续再去访问这个servlet的时候, 就会拿 内存中servlet实例来响应请求.直至服务器停止...

##servletContext对象
ServletContext对象就代表着一个web应用, 那么只要拿到一个SerlvetContext对象, 就代表着一个web应用...每个web应用,都有与其相对应的唯一的servletContext对象.

对于每个web应用,都有一个唯一的sevletContext对应与其对应,那么 如果你在servlet1拿到了对serlvetContext对象引用,向serlvetContext存入了一些值, 然后现在去servlet2, 那么 也可以获得对serlvetContext引用,这个时候,那么拿到的同一个对象,都是代表着 当前的web应用,由于拿到的是同一个对象, 那么就可以实现数据的共享. 域对象：
1. 域对象是一个容器,2. 域对象容器就会有大小(范围)3. 域对象都有实现数据共享的方法		
		setAttribute(name,value)  --- String , Object
		getAttribute(name) , 返回值是 Object ---- String
		removeAttribute(name)  ------- StringSerlvetContext 对象是一个域对象:
