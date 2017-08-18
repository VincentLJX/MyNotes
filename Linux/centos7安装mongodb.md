#CentOs7安装MongoDB
##官方网站
<https://www.mongodb.com/download-center#community>
##下载版本
<https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-3.4.7.tgz>
##解压
	tar -zxvf mongodb-linux-x86_64-rhel70-3.4.7.tgz -C /app
##重命名
	mv  mongodb-linux-x86_64-rhel70-3.4.7.tgz mongodb3.4.7
##添加到环境变量中
	export PATH=/app/mongodb3.4.7/bin:$PATH
##创建数据库目录
MongoDB的数据存储在data目录的db目录下，但是这个目录在安装过程不会自动创建，所以你需要手动创建data目录，并在data目录中创建db目录。

	mkdir -p /data/db

##启动
	./mongo
##测试
	> db.runoob.insert({x:10})
	WriteResult({ "nInserted" : 1 })
	> db.runoob.find()
	{ "_id" : ObjectId("5604ff74a274a611b0c990aa"), "x" : 10 }
	>
##MongoDb web 用户界面
MongoDB 提供了简单的 HTTP 用户界面。 如果你想启用该功能，需要在启动的时候指定参数 --rest

	mongod --dbpath=/data/db --rest