#SmallChart
##环境搭建
1. 修改build.gradle - project

		allprojects {
		    repositories {
		        jcenter()
		        maven { url "https://jitpack.io" }    #添加的部分
		    }
		}
2. 修改build.gradle - module

		dependencies {
		    compile fileTree(dir: 'libs', include: ['*.jar'])
		    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
		        exclude group: 'com.android.support', module: 'support-annotations'
		    })
		    compile 'com.android.support:appcompat-v7:25.3.1'
		    compile 'com.github.Idtk:SmallChart:v0.1.1'       #添加的部分

		    testCompile 'junit:junit:4.12'
		}
##折线图的使用
