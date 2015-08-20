###JSPatch 动态通过指定URL 的js文件更改app内方法的调用实现后台动态更改app代码的效果.
####应用场合:用于app发布appStore后的补丁修改
##### 核心代码及配置
```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    [JPEngine startEngine];
    [NSURLConnection sendAsynchronousRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:@"https://github.com/YangYouYong/patch/raw/master/patch.js"]]
                                       queue:[NSOperationQueue mainQueue]
                           completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {
                               
                               NSString *script = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
                               if (script) {
                                   [JPEngine evaluateScript:script];
                                   self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
                                   self.window.rootViewController = [[ViewController alloc] init];
                                   [self.window makeKeyAndVisible];
                               }
                           }];
    
    return YES;
}

```

##### JS代码
//替换ViewController 里的 tableView: didSelectRowAtIndexPath:方法

```
defineClass("ViewController", {
  //instance method definitions
  tableView_didSelectRowAtIndexPath: function(tableView, indexPath) {
    var row = indexPath.row()
    var alertView = require('UIAlertView').alloc().initWithTitle_message_delegate_cancelButtonTitle_otherButtonTitles("Alert","alert_message", null, "OK", null);
     alertView.show()
  }
}, {})

```