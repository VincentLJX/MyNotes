#单节点CentOs7下FastDFS-5.0.5与nginx-1.8.0的整合
##0.软件清单

	FastDFS_v5.05.tar.gz
	fastdfs-nginx-module_v1.16.tar.gz
	libfastcommonV1.0.7.tar.gz
	nginx-1.8.0.tar.gz
	
下载上述文件，下载地址:<http:www.baidu.com>，将这些文件放到/user/local路径下

**备注：以下不涉及到配置文件部分的安装，只做了精简的说明，一般不会遇到问题**

##1.安装FastDFS的环境
1. 安装gcc

		yum install gcc-c++
2. 安装libevent

		yum -y install libevent
3. 安装libfastcommon
	
		cd /usr/local
		#1.解压
		tar -zxvf libfastcommonV1.0.7.tar.gz
		
		cd /usr/local/libfastcommon-1.0.7/
		#2.编译
		./make.sh
		#3.安装
		./make.sh install
		#4.拷贝libfastcommon的库文件到/usr/lib
		cp /usr/lib64/libfastcommon.so /usr/lib
		
##2.安装Tracker
###2.1.安装
	#解压
	tar -zxvf FastDFS_v5.05.tar.gz
	#解压后的文件夹名称就是FastDFS
	cd FastDFS
	#编译、安装
	./make.sh
	./make.sh install
	#安装成功将安装目录下的conf下的文件拷贝到/etc/fdfs/下
	cp -r /usr/local/FastDFS/conf/. /etc/fdfs
###2.2.配置
	cd /etc/fdfs
	#这一步提示是否覆盖，选在y
	cp tracker.conf.sample tracker.conf
	#修改tracker.conf，找到base_path后面变量改为/home/FastDFS
	vi tracker.conf
	base_path=/home/FastDFS
	
	#保存后出来要创建/home/FastDFS
	mkdir -p /home/FastDFS
	
###2.2.启动
	/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf restart
##3.安装Storage
###3.1.安装
由于，我们在一台机器上安装的，而tracker和storage使用相同的安装包所以这一步，什么也不用做
###3.2.配置
	cd /etc/fdfs
	#这一步提示是否覆盖，选在y
	cp storage.conf.sample storage.conf
	#修改storage.conf
	vi storage.conf
	group_name=group1
	base_path=/home/FastDFS
	store_path0=/home/FastDFS/fdfs_storage
	tracker_server=192.168.1.xxx:22122      #192.168.1.xxx你机器的ip
	
	#保存后出来要创建/home/FastDFS/fdfs_storage
	mkdir -p /home/FastDFS/fdfs_storage

###3.3.启动
	/usr/bin/fdfs_storaged  /etc/fdfs/storage.conf restart
##4.安装FastDFS-nginx-module
###4.1.安装
	#解压即可
	cd /usr/local
	tar -zxvfFastDFS-nginx-module_v1.16.tar.gz
###4.2.配置
	cd /usr/local/fastdfs-nginx-module/src
	#1.修改config文件将/usr/local/路径改为/usr/，也就是删除/local
	vi config
	#2.mod_fastdfs.conf拷贝至/etc/fdfs/下
	cp mod_fastdfs.conf /etc/fdfs/
	#3.修改mod_fastdfs.conf
	vi /etc/fdfs/mod_fastdfs.conf
	base_path=/home/FastDFS
	tracker_server=192.168.1.xxx:22122     #192.168.1.xxx你机器的ip
	url_have_group_name=true
	store_path0=/home/FastDFS/fdfs_storage
	#4.将libfdfsclient.so拷贝至/usr/lib下
	cp /usr/lib64/libfdfsclient.so /usr/lib/
	#5.创建nginx/client目录，为Nginx的安装做准备
	mkdir -p /var/temp/nginx/client
	
##5.安装Nginx
1. 安装必要的工具
	
		#1.
		yum install gcc-c++
		#2.
		yum install -y pcre pcre-devel
		#3.
		yum install -y zlib zlib-devel
		#4.
		yum install -y openssl openssl-devel
2. 解压
		
		cd /usr/local
		tar -zxvf nginx-1.8.0.tar.gz
3. 创建nginx文件夹
		
		mkdir -p /usr/local/nginx
4. 把5步骤中所有涉及到文件夹都创建一遍
5. 执行config
	
		./configure \
		--prefix=/usr/local/nginx \
		--pid-path=/var/run/nginx/nginx.pid \
		--lock-path=/var/lock/nginx.lock \
		--error-log-path=/var/log/nginx/error.log \
		--http-log-path=/var/log/nginx/access.log \
		--with-http_gzip_static_module \
		--http-client-body-temp-path=/var/temp/nginx/client \
		--http-proxy-temp-path=/var/temp/nginx/proxy \
		--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
		--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
		--http-scgi-temp-path=/var/temp/nginx/scgi \
		--add-module=/usr/local/fastdfs-nginx-module/src

6. 编译加安装

		make && make install
7. 配置文件
	
		cd /usr/local/nginx/conf
		vi nginx.conf
		#1.先加入这行
		user root;
		#在http{}中添加如下内容
		 server {
        listen       80;
        server_name  192.168.1.xxx;       #192.168.1.xxx你机器的ip

        location /group1/M00/{
                root /home/FastDFS/fdfs_storage/data;
                ngx_fastdfs_module;
       		 }
    	 }
 8. 启动
 		
 		cd /usr/local/nginx/sbin
 		./nginx -s reload
 		
##6.测试
###6.1.上传
	#~/icon.jpg你的文件路径加文件名
	/usr/bin/fdfs_test /etc/fdfs/client.conf upload ~/icon.jpg
	#最后一行显示：example file url: http://192.168.1.xxx/group1/M00/00/00/wKgBNFmgNzCAPSZjAAC8s7Be-88897_big.jpg  则表示成功
###6.2.外网访问
	#1.关闭防火墙
	systemctl stop firewalld.service #停止firewall
	systemctl disable firewalld.service #禁止firewall开机启动
	#2.在浏览器输入
	http://192.168.1.xxx/group1/M00/00/00/wKgBNFmgNzCAPSZjAAC8s7Be-88897_big.jpg