# zhifudashi_ios_demo
支付大师iOSdemo,支付宝微信免签，二维码收款，即时到账，安全高效。
# 支付大师 http://zfds.codecocoa.com
## 是真正的个人免签约第三方h5支付通道聚合支付接口，云端免挂机监控，即时到账，支持小程序、网站二维码扫码收款。只需个人微信支付宝账号即可自动化收款，超稳定不漏单

## 接口文档  http://paydocs.codecocoa.com


### 对接简单，异步回调，安全高效，不限行业


``` java
 NSString * orderid =[NSString stringWithFormat:@"%ld", (long)[[NSDate date] timeIntervalSince1970] * 1000];// 商户本地订单ID
    NSString * price=@"0.15";// 价格
    NSMutableDictionary * pardic =[NSMutableDictionary dictionaryWithCapacity:0];
    [pardic setObject:orderid forKey:@"order_id"];
    [pardic setObject:@"alipay" forKey:@"order_type"];// 支付类型 alipay / wechat
    [pardic setObject:price forKey:@"order_price"];// 支付价格
    [pardic setObject:@"测试商品2" forKey:@"order_name"];// 支付名称
    [pardic setObject:@"http://localhost:8080/ApiOrders/asyncnotify" forKey:@"redirect_url"];// 异步通知的地址
    [pardic setObject:@"app" forKey:@"extension"];// 拓展字段
    [pardic setObject:self.uid forKey:@"uid"];
    NSString * value =[NSString stringWithFormat:@"%@%@",orderid,price];
    NSString * md5_key =[NSString stringWithFormat:@"%@%@",[self md5:value],self.appkey];
//    签名->加密方法 md5(md5(order_id + order_price) + secretkey) //
    NSString * sign =[self md5:md5_key];
    [pardic setObject:sign forKey:@"sign"];
    NSString * json =[KWHelpClass dictionaryToJson:pardic];
    NSLog(@"json=====%@",json);
    // 订单的创建接口地址，最新的请到http://docs.zhifudashi.com 文档中心或者商户后台获取
    [MJHttpTool Post:[NSString stringWithFormat:@"%@/ApiOrders/createorder",@"https://www.zhifudashi.com"] parameters:pardic success:^(id responseObject) {
        
            NSLog(@"responseObject======%@",responseObject);
        if ([[responseObject objectForKey:@"code"]integerValue]==200) {
            NSDictionary * data=[responseObject objectForKey:@"data"];
            NSString * qr_url =[data objectForKey:@"qrUrl"];
            self->_pricelabel.text=[NSString stringWithFormat:@"%.2f",[[data objectForKey:@"orderPrice"]floatValue]];
            [self generatingTwoDimensionalCode:qr_url];
        }else{
            [SVProgressHUD showErrorWithStatus:[responseObject objectForKey:@"msg"]];
        }
        
        } failure:^(NSError *error) {
            
            NSLog(@"error======%@",error);
    }];

```


### 交流群：QQ：385468484

### 更多demo
php:   https://github.com/mailes/zhifudashi_php_demo  
ios: https://github.com/mailes/zhifudashi_ios_demo    
android: https://github.com/mailes/zhifudashi_android_demo   
java:https://github.com/mailes/zhifudashi_java_demo   
