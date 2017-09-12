#IOS基础控件的使用
##UISwitch
1. UISwitch的初始化

		UISwitch *switchView = [[UISwitch alloc] initWithFrame:CGRectMake(54.0f, 16.0f, 100.0f, 28.0f)];
2. 设置UISwitch的初始化状态

		switchView.on = YES;//设置初始为ON的一边
3. UISwitch事件的响应

		[switchView addTarget:self action:@selector(switchAction:) forControlEvents:UIControlEventValueChanged];
		
##设置圆角

	//1.设置圆角
    self.userIcon.layer.cornerRadius = self.userIcon.frame.size.width / 2;
    //2.将多余的部分切掉
    self.userIcon.layer.masksToBounds = YES;
##share