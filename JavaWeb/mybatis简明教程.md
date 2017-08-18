#Eclipse下MyBatis简明教程
##工程搭建
- 创建Java工程
	
- 导入jar包

	把目录（mybatis-3.2.7/lib）下的文件导入到lib目录下
	
		asm-3.3.1.jar
        cglib-2.2.2.jar
        commons-logging-1.1.1.jar
        javassist-3.17.1-GA.jar
        log4j-1.2.17.jar
        log4j-api-2.0-rc1.jar
        log4j-core-2.0-rc1.jar
        mybatis-3.2.7.jar						-->mybatis的核心包
        mysql-connector-java-5.1.7-bin.jar		-->数据库驱动包
        slf4j-api-1.7.5.jar
        slf4j-log4j12-1.7.5.jar
	build path -> add build path
	
- 配置日志文件
	创建**config**文件夹用于存放配置文件
	
	在config文件夹中创建log4j.properties文件，输入如下内容
	
		log4j.rootLogger=DEBUG, stdout
		log4j.appender.stdout=org.apache.log4j.ConsoleAppender
		log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
		log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
	
- 创建po类
	创建user类
	
		public class User {
			private Integer id;
			private String username;
			private Date birthday;
			private String sex;
			private String address;
			/**
			 *	get/set也需要写，这里不展示了
			 **/
		}
- 配置数据
	在config文件夹中创建db.properties文件，输入如下内容
		 
		jdbc.driver=com.mysql.jdbc.Driver
        jdbc.url=jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8
        jdbc.username=root
        jdbc.password=123456
		
- 设置sql映射文件
	在config文件夹中创建user.xml文件，输入如下
	
		<?xml version="1.0" encoding="UTF-8" ?>
		<!DOCTYPE mapper
			PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
			"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
			<mapper namespace="test">

			<select id="findUserById" parameterType="java.lang.Integer" 				resultType="User类的包名.User">
				select * from user where id=#{id}
			</select>
		</mapper>
		
- 配置SqlMapConfig.xml
	在config文件夹中创建SqlMapConfig.xml文件，输入如下

		<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
        <configuration>
            <properties resource="db.properties"/>
            <environments default="development">
                <environment id="development">
                <!-- 使用jdbc事务管理-->
                <transactionManager type="JDBC" />
                <!-- 数据库连接池-->
                <dataSource type="POOLED">
                    <property name="driver" value="${jdbc.driver}" />
                    <property name="url" value="${jdbc.url}" />
                    <property name="username" value="${jdbc.username}" />
                    <property name="password" value="${jdbc.password}" />
                </dataSource>
                </environment>
            </environments>
            
            <mappers>
                <mapper resource="config/User.xml"/>
            </mappers>
        </configuration>
	
		
	
- 测试
	在测试方法中输入如下内容
	
		String resource = "SqlMapConfig.xml";
        //通过流将核心配置文件读取进来
        InputStream inputStream = Resources.getResourceAsStream(resource);
        //通过核心配置文件输入流来创建会话工厂
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(inputStream);
        //通过工厂创建会话
        SqlSession openSession = factory.openSession();
        //第一个参数:所调用的sql语句= namespace+.+sql的ID
        User user = openSession.selectOne("test.findUserById", 1);
        System.out.println(user);
        openSession.close();




##对数据库的增删改查
###1.根据id查找用户
在User.xml文件中的

	 <mapper namespace="test"> </mapper>
节点内添加：
	
	<select id="findUserById" parameterType="int" resultType="User所在的包.User">
    	select * from user where id=#{id}
    </select>

**注：以下皆是在此节点下添加**

###2.根据用户名查找用户
	<select id="findUserByUsername" parameterType="java.lang.String" resultType="User所在的包.User">
		select * from user where username like '%${value}%'
	</select>

###3.添加用户1
	<insert id="insertUser1" parameterType="User所在的包.User">
		insert into user(username,birthday,sex,address) values(#{username},#{birthday},#{sex},#{address})
	</insert>

###4.添加用户2
	<insert id="insertUser2" parameterType="User所在的包.User">
		<selectKey keyProperty="id" order="AFTER" resultType="int">
			select LAST_INSERT_ID()
		</selectKey>
		insert into user(username,birthday,sex,address) values(#{username},#{birthday},#{sex},#{address})
	</insert>

###5.修改用户
	<update id="updateUser" parameterType="User所在的包.User">
		update user set username=#{username},birthday=#{birthday},sex=#{sex},address=#{address} where id=#{id}
	</update>

###6.删除用户
	<delete id="deleteUser" parameterType="int">
		delete from user where id=#{id}
	</delete>
###测试
		private SqlSessionFactory sqlSessionFactory;

	@Before
	public void createSqlSessionFactory() throws IOException {
		String resource = "SqlMapConfig.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
	}
	
	@Test
	public void test(){
		SqlSession sqlSession = null;
		sqlSession = sqlSessionFactory.openSession();
		//1.根据id查找用户
		//selectUserById(sqlSession);
		//2.根据用户名查找用户
		//selectUserByUsername(sqlSession);
		//3.添加用户1
		//insertUser1(sqlSession);
		//4.添加用户2
		//insertUser2(sqlSession);
		//5.修改用户
		//updateUser(sqlSession);
		//6.删除用户
		//deleteUser(sqlSession);
	}
	//1.根据id查找用户
	public void selectUserById(SqlSession sqlSession){
		User user = sqlSession.selectOne("test.findUserById",10);
		System.out.println(user);
		sqlSession.close();
	}
	//2.根据用户名查找用户
	public void selectUserByUsername(SqlSession sqlSession){
		List<User> users = sqlSession.selectList("test.findUserByUsername","小明");
		for (User user : users) {
			System.out.println(user);
		}
		sqlSession.close();
	}
	//3.添加用户1
	public void insertUser1(SqlSession sqlSession){
		User user = new User();
		user.setUsername("美娜");
		user.setSex("2");
		user.setAddress("beijing");
		sqlSession.insert("test.insertUser1",user);
		sqlSession.commit();
		sqlSession.close();
	}
	//4.添加用户2
	public void insertUser2(SqlSession sqlSession){
		User user = new User();
		user.setUsername("小楠");
		user.setSex("w");
		user.setAddress("beijing");
		sqlSession.insert("test.insertUser2",user);
		sqlSession.commit();
		sqlSession.close();
	}
	//5.修改用户
	public void updateUser(SqlSession sqlSession){
		User user = new User();
		user.setUsername("美娜");
		user.setSex("w");
		user.setAddress("beijing");
		user.setId(25);
		sqlSession.update("test.updateUser", user);
		sqlSession.commit();
		sqlSession.close();
	}
	//6.修改用户
	public void deleteUser(SqlSession sqlSession){
		sqlSession.update("test.deleteUser", 25);
		sqlSession.commit();
		sqlSession.close();
	}

##Mapper动态代理

###接口文件
1. Mapper接口方法名和Mapper.xml中定义的statement的id相同
2. Mapper接口方法的输入参数类型和mapper.xml中定义的statement的parameterType的类型相同
3. Mapper接口方法的输出参数类型和mapper.xml中定义的statement的resultType的类型相同

		public interface UserMapper {
			public User findUserById(Integer id);
			public List<User> findUserByUsername(String userName);
		}

###映射文件
1. 接口的名称和映射文件名称除扩展名外要完全相同
2. 接口和映射文件要放在同一个目录下
3. namespace的值为UserMapper接口路径

		<?xml version="1.0" encoding="UTF-8" ?>
		<!DOCTYPE mapper
		PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
		"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
		<mapper namespace="UserMapper所在的包.UserMapper">

			<!-- 1.通过id查找用户 -->
			<select id="findUserById" parameterType="int" resultType="User所在的包.User">
				select * from user where id=#{id}
			</select>
			
			<!-- 2.根据用户名查找用户 -->
			<select id="findUserByUsername" parameterType="java.lang.String" resultType="User所在的包.User">
				select * from user where username like '%${value}%'
			</select>
		</mapper>



###加载映射文件
1. 单个加载

		<mapper class="UserMapper所在的包.UserMapper"/>
2. 批量加载
	
	
		1. 接口的名称和映射文件名称除扩展名外要完全相同
		2. 接口和映射文件要放在同一个目录下
		
		<package name="接口所在的包名"/>

###测试
	
	private SqlSessionFactory sqlSessionFactory;

	@Before
	public void createSqlSessionFactory() throws IOException {
		String resource = "SqlMapConfig.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
	}

	@Test
	public void test() throws Exception {
		SqlSession sqlSession = sqlSessionFactory.openSession();
		UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
		// 1.根据id查找用户
		// selectUserById(userMapper);
		// 2.根据用户名查找用户
		// selectUserByUsername(userMapper);
	}

	// 1.根据id查找用户
	public void selectUserById(UserMapper userMapper) {
		User user = userMapper.findUserById(1);
		System.out.println(user);
	}

	// 2.根据用户名查找用户
	public void selectUserByUsername(UserMapper userMapper) {
		List<User> users = userMapper.findUserByUsername("小明");
		for (User user : users) {
			System.out.println(user);
		}
	}


