#TableView的基础使用
##1.自定义cell
1. cell的.h文件

		//声明
		+(SettingUserInfoTVC *)cellWhithTableView:(UITableView *)tableView userIconUrl:(NSString *)userIconUrl userNameLbStr:(NSString *)userNameLbStr;
2. cell的.m文件

		+(SettingUserInfoTVC *)cellWhithTableView:(UITableView *)tableView userIconUrl:(NSString *)userIconUrl userNameLbStr:(NSString *)userNameLbStr{

		    SettingUserInfoTVC *cell = [tableView dequeueReusableCellWithIdentifier:@"SettingUserInfoTVC"];
		    if (cell==nil) {
		        cell = [[[NSBundle mainBundle]loadNibNamed:@"SettingUserInfoTVC" owner:nil options:nil]lastObject];
		    }
		    cell.userNameLb.text = userNameLbStr;
		    cell.userIconIv.backgroundColor = [UIColor redColor];
		    return cell;
		}

##2.分割线
1. 不显示多余的分割线

		self.tableView.tableFooterView = [[UIView alloc] initWithFrame:CGRectZero];
2. 分割线两端对齐

		- (void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath{
		    if ([cell respondsToSelector:@selector(setSeparatorInset:)]){
		        [cell setSeparatorInset:UIEdgeInsetsZero];
		    }
		    if ([cell respondsToSelector:@selector(setLayoutMargins:)]){
		        [cell setLayoutMargins:UIEdgeInsetsZero];
		    }
		}
3.不显示分割线

		tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
##取消选中状态

		-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
		{
		    [tableView deselectRowAtIndexPath:indexPath animated:YES];
		}
