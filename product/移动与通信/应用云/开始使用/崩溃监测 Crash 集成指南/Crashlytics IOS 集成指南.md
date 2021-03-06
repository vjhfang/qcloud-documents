按照本指南在您的 iOS 应用中设置应用云 Crashlytics。

## 准备工作

在开始使用应用云 Crashlytics 之前，您需要：

* 一个启用了应用云的应用。
* 您集成了 TACCore。

## 将应用云 Crashlytics 代码库添加到您的 Xcode 项目中


### 1.在您的项目中集成应用云 SDK：
 
并在您的 Podfile 文件中添加应用云的私有源：

~~~
source "https://git.cloud.tencent.com/qcloud_u/cocopoads-repo"
source "https://github.com/CocoaPods/Specs"
~~~

> **注意：**
   一定要添加 https://github.com/CocoaPods/Specs 的原始源，否则会造成部分仓库找不到的问题。
 
### 2.添加 TACCrash 到您的 Podfile，您可以按照以下方法在 Podfile 中纳入一个 Pod：
 
~~~
pod 'TACCrash'
~~~

### 3.安装 Pod 并打开 .xcworkspace 文件以便在 Xcode 中查看该项目：
 
~~~
$ pod install
$ open your-project.xcworkspace
~~~

### 4.在 UIApplicationDelegate 子类中导入 TACCrash 模块：
 
Objective-C 代码示例：
~~~
#import <TACCrash/TACCrash.h>
~~~
Swift 代码示例：
~~~
import TACCrash
~~~


### 5.配置 TACApplication 共享实例，通常是在 `application:didFinishLaunchingWithOptions:` 方法中配置：
 
一般情况下您使用默认配置就可以了，用一下代码使用默认配置启动Crash服务。如果您在引入其它模块的时候，调用了该方法，请不要重复调用。
Objective-C 代码示例：
~~~
    [TACApplication configurate];
~~~

~~~
	TACApplication.configurate();
~~~

如果您需要进行自定义的配置，则可以使用以下方法，我们使用了 Objective-C 的语法特性 Category 和一些 Runtime 的技巧保障了，只有在您引入了 TACCrash 模块的时候，才能从 TACApplicaitonOptiosn 里面看到其对应的配置属性，如果你没有引入 TACCrash 模块这些属性就不存在，请不要在没有引入 TACCrash 模块的时候使用这些配置，这将会导致您编译不通过：

Objective-C 代码示例：
~~~
    TACApplicationOptions* options = [TACApplicationOptions defaultApplicationOptions];
	// 自定义配置
	//     options.crashOptions.[Key] = [Value];
    //
    [TACApplication configurateWithOptions:options];
~~~

Swift 代码示例：
~~~
	let options = TACApplicationOptions.default()
	// 自定义配置
	// options?.crashOptions.[Key] = [Value];
	TACApplication.configurate(with: options);
~~~


## 配置 Crashlytics 上报符号表脚本

1.在导航栏中打开您的工程。
2.打开Tab `Build Phases`。
3.点击 `Add a new build phase` , 并选择 `New Run Script Phase`。
4.将下面的代码粘贴入  `Type a script...` 文本框。

	~~~~
	"${PODS_ROOT}/TACCrash/Scripts/run"
	~~~~
