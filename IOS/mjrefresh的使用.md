#MJRefresh的使用
##基本使用
1. 添加头文件

		#import "MJRefresh.h"
2. 设置

		//添加下拉刷新
	    // 添加头部的下拉刷新
	    MJRefreshNormalHeader *header = [[MJRefreshNormalHeader alloc] init];
	    [header setRefreshingTarget:self refreshingAction:@selector(headerClick)];
	    _tableView.mj_header = header;
	    
	    // 添加底部的上拉加载
	    MJRefreshBackNormalFooter *footer = [[MJRefreshBackNormalFooter alloc] init];
	    [footer setRefreshingTarget:self refreshingAction:@selector(footerClick)];
	    _tableView.mj_footer = footer;

3. 响应函数

		-(void)headerClick{
		    [self.dateList removeAllObjects];
		    [self.mTableView reloadData];
		    self.begin = 0;
		    self.isHasNext = true;
		    [self getDetailDataListItems];

		}
		-(void)footerClick{
		    if (self.isHasNext == true) {
		        self.begin = self.begin + self.skip;
		        [self getDetailDataListItems];
		    }else{
		        [self.mTableView.mj_footer endRefreshing];
		    }

		}
4. 关闭

		[_tableView.mj_header endRefreshing];
        [_tableView.mj_footer endRefreshing];
