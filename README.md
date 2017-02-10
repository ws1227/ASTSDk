
#ASTSDK使用说明
 


---

### 初始化 SDK
 
在需要使用 SDK 的地方导入头文件'StarLiveLib/ASTSDK.h'
 
添加 SDK 初始化方法。
 
 

```
-(BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
   
**  appKey和appsocks联系客服提供，cerName推送证书名称
   [[ASTSDK sharedSDK]registerWithAppID:@"appKey" appsocks:@"appsocks" cerName:@"cerName"];

 
  }
```

---



###### *以下所有的block回调方法中ceode=1表示是请求成功，其他数值表示请求失败并且返回的message说明了失败原因*

---

 
####登录SDK 一般是放在用户登录成功后调用

```
[[ASTSDK sharedSDK]UserLogin:@"用户ID" nickName:@"昵称" avatar:@"头像地址链接"completion:^(NSString * _Nonnull message, NSInteger code) {
            
       NSLog(@"登录返回状态%ld",(long)code);

      
 }];
 （此处需要注意只有第一次登录设置的名字和头像有效 后期调用名字和头像不会变需要调用UserModiNickName 和 UserModiUserFace分别修改用户昵称和头像）
```

####  `***特别说明以下api方法调用前必须是放在登录之后调用***`


---

###  当前登录状态
```
-(BOOL)isLogined;
```



### 修改头像

 
```
[[ASTSDK sharedSDK]UserModiUserFace:@"http://pic.58pic.com/58pic/13/91/46/64c58PICRTg_1024.png" completion:^(NSString * _Nonnull message, NSInteger code) {
    
        NSLog(@"更换头像返回状态%ld",(long)code);
    }];
```

---

###修改昵称
   
```
[[ASTSDK sharedSDK]UserModiNickName:@"nickname" completion:^(NSString * _Nonnull message, NSInteger code) {

                NSLog(@"更换昵称返回状态%ld",(long)code);
 
    }];
```

###返回充值列表的数据
###### 返回的数据说明
- iosStrength表示充值可以获取的体力
- priceStr表示支付价格(元)
- rechargeId充值ID
- 请求失败的话数组为空
 
```
[[ASTSDK sharedSDK]UserRechargeList:^(NSArray * _Nonnull dataArray) {
    
        
        for (NSDictionary *dic in dataArray) {
            
            NSLog(@"获取的充值列表%@",dic);
            
        }
    }];
```

---

###账户充值
-  rechargeId是从充值列表的数据中获取
-  transId是交易流水号 必须大于13位

```
[[ASTSDK sharedSDK]UserRecharge:@"1" transId:@"10000900000889" completion:^(NSString * _Nonnull message, NSInteger code) {
            
            NSLog(@"查询充值结果%ld---%@",(long)code,message);
            
        }];
```

---
###直播间确认充值按钮的点击事件的代理方法
- 使用方法初始化SDK时候需要遵守代理ASTSDKDelegate
 
 
```
[ASTSDK sharedSDK].delegate=self;
```


- charge是一个字典类型返回当前选中的充值额度的数据
 

```
-(void)userRecharge:(_Nonnull id)charge;
```

---
### 退出登录代理回调

```
-(void)exitSDK;
```

    

    
