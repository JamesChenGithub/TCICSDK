# TCICSDK 使用说明

## 功能介绍

`TCICSDK` 为 腾云互动课堂开放的aPass方案SDK简称

## iOS 集成

### pod 集成方式


1. 在工程`Poddile` 文件中，如果想集成**静态库**，添加依赖 `pod 'TCICSDK'`, `TCICSDK` 依赖以下 pod 库，在执行pod install命令时会自动安装；

    ```
    pod 'Masonry'
    pod 'YYModel'
    pod 'MBProgressHUD'
    pod 'TXLiteAVSDK_TRTC'
    pod 'TIWLogger_iOS'
    pod 'TIWCache_iOS'
    ```
    
    <!--如果要添加**动态库**：在工程`Poddile` 文件中添加依赖 `pod 'TCICSDK_Dyn'`， `TCICSDK_Dyn` 依赖以下 pod 库，在执行pod install命令时会自动安装，其他 `TXLiteAVSDK_TRTC`, `TIWLogger_iOS`默认已打包至动态库。

    ```
    pod 'Masonry'
    pod 'YYModel'
    pod 'MBProgressHUD'
    ```
    -->
    如果要使用`TXLiteAVSDK_Professional`，在工程`Poddile` 文件中添加依赖 `pod 'TCICSDK_Pro'` `TCICSDK_Pro` 依赖以下 pod 库，在执行pod install命令时会自动安装;
    
    ```
    pod 'Masonry'
    pod 'YYModel'
    pod 'MBProgressHUD'
    pod 'TXLiteAVSDK_Professional'
    pod 'TIWLogger_iOS'
    pof 'TIWCache_iOS'
    ```

2.  在终端中跳到`Podfile`所在目录，`pod install` 即可自动安装所需要的依赖;


注意事项：
1. 已安装的情况下，建议先 `pod search TCICSDK`以确认最新的版本号；然后 `pod repo update` 更新下，之后删除本地`Pods文件夹`以及`Podfile.lock`后，再使用`pod install --repo-update`以便安装最新;


### 使用说明

1. 在使用处引用 `#import  <TCICSDK/TCICClassController.h>` 即可

2. <a name="tcicclassconfig">`TCICClassConfig` 必填参数说明 </a>

	| 字段 | 类型 | 描述 | 必传 | 
	| ---- | ---- | ---- | ---- |
	| userId |  string |  进教室用户的userId，可参考 [云 API - 学生老师注册](https://classroom-docs.qcloudtrtc.com/#/business/Class?id=3%e5%ad%a6%e7%94%9f%e8%80%81%e5%b8%88%e6%b3%a8%e5%86%8c) | 必传 |
	| token | string | 可参考 [云 API - 换取票据](https://classroom-docs.qcloudtrtc.com/#/business/Class?id=4-%e6%8d%a2%e5%8f%96%e7%a5%a8%e6%8d%ae)，返回的token信息 | 必传 |
	| schoolId | uint32 | 学校ID，可参考 [云 API - 创建机构](https://classroom-docs.qcloudtrtc.com/#/business/Class?id=1%e5%88%9b%e5%bb%ba%e6%9c%ba%e6%9e%84) | 必传 |
	| classId | uint32 | 课堂编号，可参考 [云 API- 创建课堂](https://classroom-docs.qcloudtrtc.com/#/business/Class?id=12-%e5%88%9b%e5%bb%ba%e8%af%be%e5%a0%82)| 必传 | 
	| customParams | dictionary<string, string> | 自定义透传字段，业务侧传相应键值对信息，由SDK拼接成query串，带入课堂页。SDK 这些字段（ `platform`, `device`, `schoolid`, `classid`, `userid`, `sessionid`, `globalrandom`, `scene`, `debugjs`, `debugcss`, `samelayer`, `webloadstamp` ）占用了，业侧侧不要重复传 | 非必传 | 
	
	*  以下字段虽然SDK已占用，但是业务侧可覆盖写

		| 参数名称  | 参数类型  |  参数描述	| 是否能覆盖 | 
		| ---  |  ---  | ---- | ---- | 
		| scene | string | 场景(可根据场景配置不同的定制) | 可以 | 
		| debugjs | string | 调试阶段使用，自定义Javascript脚本地址(调试用，会覆盖学校配置) | 可以 | 
		| debugcss | string | 调试阶段使用，自定义css地址(调试用，会覆盖学校配置) | 可以 | 


	**如何更新课堂地址**：

	1. 使用KVC修改 `htmlUrl` 值 ，如  ` [roomConfig setValue:@"http://xx/yy/index.html" forKey:@"htmlUrl"];`
	2. 使用KVC修改课堂页时，`value` 必须满足以下条件：
		*  `Value`必须是 	`NSString` 类型，传入其他类型或`nil`，不会生效;
		*  `Value`必须能正确构造出`NSURL`对象, 即 `[NSURL URLWithString:value]` 或 `[NSURL fileURLWithPath:value]`  ，否则不会生效;
		*  若传入与SDK逻辑未调试的地址，比如传入的地址为 `https://v.qq.com/`，虽然能满足上两项条件，使用SDK也会正常打开，但SDK内部仍然会出示提示框，要求退出;

<!--	* 除此之后，还可以通过`KVC方式`入一些字段信息，可传键值如下表:

		| 字段 | 类型 | 描述 | 必传 | 
		| ---- | ---- | ---- | ---- |
		| htmlUrl |  string |  课堂html地址 | 非必填 |
		| coreVersion | string | 课堂html版本 | 非必填 |
		| coreEnv | string | 课堂环境 : 空:正式环境, test: 测试环境, dev: 开发环境, pre: 预发布环境| 非必传 |
		| appGroup | string | 屏幕分享时，由外享传入的appGroup | 非必传 | -->


	

	

2. `TCICClassController `说明

	| API | 说明 | 
	| --- | ---- | 
	| TCIC_SDK_Version | SDK版本号 | 
	|  + (instancetype)classRoomWithConfig:(TCICClassConfig *)roomConfig | 主线程调用；创建上课页面viewcontroller方法，roomConfig必传，如果roomConfig参数不合法（主要是是空），会返回空 |
	

	示例代码如下:
	
	```
	TCICClassConfig *roomConfig = [[TCICClassConfig alloc] init];
	roomConfig.userId = "test";
	roomConfig.token = "test_token";
	roomConfig.classId = 123454;
	roomConfig.schoolId = xxxxx;
	
	// 如何更新测试地址：详见注意事项2
	// 	[roomConfig setValue:@"http://xx/yy/index.html" forKey:@"htmlUrl"];
	           
	TCICClassController *vc = [TCICClassController classRoomWithConfig:roomConfig];
	if (vc) {
		[(UINavigationController *)self.window.rootViewController pushViewController:vc animated:YES];
	} else {
		NSLog(@"参数有误");
	}    
	```
	
	
    


	**注意事项**：
	1. 如果的你App（iPad）配置类似下面：
		* 需要支持iPad ;
		* 需要支持所有方向；
		
		**那么请勾选上 `Requires full screen` 选项 (该选项对现有App不影响)**，否则 ` TCICClassController ` 无法正常旋转至横屏
	
		![](https://main.qcloudimg.com/raw/26926026e4a4ed5d565ede21258a47ab.png)
    
    2. SDK使用Bugly共享式收集crash，业务方自行集成Bugly即可将SDK相关的crash同步给腾讯侧进行修复；

    


    		
    		
	
	

