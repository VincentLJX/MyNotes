#IOS上传用户头像修改用户名控件BBInputKit
1. 在info.plist中添加权限

		Privacy - Photo Library Usage Description     1
		Privacy - Camera Usage Description		1
		
2. 将下面两个文件夹拖到工程目录下

		BBInputKit   #用户名		UIViewController+SelectPhotoIcon   #用户头像
3.调用修改昵称的方法