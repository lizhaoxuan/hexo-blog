
---
title: Calabash探索4-Calabash踩坑总结
date: 2017-3-16 16:20:58
author : 暴打小女孩

tags: 测试
---

转载请注明出处：https://lizhaoxuan.github.io


## 前言

为了保证前面几章阅读上的流程度，我们将Calabash从环境搭建到具体使用中所遇到的所有问题做了一个总结，这里大部分是我实际所经历的坑，同时我也会尽可能的收录一下我在学习过程中从别的博客看到的问题。


<!-- more -->

## 坑1：Calabash安装时Ruby报错

```
$ sudo gem install calabash-android
Password:
Building natie extensions. This could take a while...
RROR: Failed to build gem native extension.

    /System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin/ruby extconf.rb
	mkmf.rb can't find header files for ruby at /System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/lib/ruby/include/ruby.h
```

好像大部分人都不会遇到这个问题，虽然看着像是Ruby缺了一个头文件，但实际问题不在于Ruby。

执行下面命令：

	$ xcode-select --install
    
安装xcode就OK了！看样子是ruby依赖了xcode？心塞~


## 坑2：Could not find an Android SDK please make it is install

```
No test server found for this combination of app and calabash version.Recreating test server
ERROR:Could not find an Android SDK please make it is install
...

```
其他JDK,ANT问题同理。

首先确保你确实安装了Android SDK,并且正确配置了环境变量。
如果已经确认了什么问题都没有，还是报这个错。
执行命令：

	$.bash_profile
 
还是报这个错？ 哈哈哈，下面就要出绝招了：**重启大法好啊~**
恩，重启下电脑应该就可以了


## 坑3：App did not start 或 WARN:Did not find 'android.jar'...

```
WARN: Did not find 'android.jar' in any standard directory of '.../sdk/platforms'.Calabash will therefore take longer to load
Feature: /**
	Scenario: /**
    App did not start(RuntimeError)
    ./feature/support/app_life_cycle_hooks.rb:5:in'Before'
    ...
```

首先确认手机或虚拟机以链接到电脑，并且调试模式什么的都开了。
确认App配置中是否赋予了网络权限,以Android为例

manifest.xml文件：

	<uses-permission android:name="android.permission.INTERNET" />
    


## 坑4：\**.apk is not signed with any of the available keystores

```
No test server found for this combination of app and calabash version.Recreating test server
**.apk is not signed with any of the available keystores

```

这个其实不算是坑，Calabash测试apk需要重新对其进行签名才可，按照错误提示或官方文档就可以解决这个问题，这个教程太多啦，就不熬述了。[付官方解决方案](https://github.com/calabash/calabash-android/wiki/Running-Calabash-Android)。


## 坑5：Calabash自定义的Steps,执行过程中提示未定义

```
You can implement step definitions for undefined steps with these snippets:

Then /^I through welcomePages $/ do
  pending # Write code here that turns the phrase above into concrete actions
end
```

我写的没毛病啊，怎么就一直提示未定义呢？
如果你也遇到了这个问题，请先严格检查自己的代码有没有问题！如果检查一切都没有问题，就看看是不也犯了我这个错误！我觉得一般人都范不了我这个错误……

最开始我的定义是这样写的：

```
Then /^I through welcomePages $/ do
	Then I drag from 90:50 to 20:50 moving with 20 steps 
    Then I drag from 90:50 to 20:50 moving with 20 steps 
    Then I drag from 90:50 to 20:50 moving with 20 steps 
end
```

好像看着也没毛病是吧？问题在这里``Then /^I through welcomePages $/ do`` 末尾``$``符前不能有空格！！！

改为下面这样就可以了！

```
Then /^I through welcomePages$/ do
	Then I drag from 90:50 to 20:50 moving with 20 steps 
    Then I drag from 90:50 to 20:50 moving with 20 steps 
    Then I drag from 90:50 to 20:50 moving with 20 steps 
end
```

</br>

 ------

[《Calabash探索1-Run Calabash》](https://lizhaoxuan.github.io/2017/03/20/Calabash探索1-Run%20Calabash/)

[《Calabash探索2-Calabash用法详解》](https://lizhaoxuan.github.io/2017/03/18/Calabash探索2-Calabash用法详解/)

[《Calabash探索3-Calabash进阶》](https://lizhaoxuan.github.io/2017/03/17/Calabash探索3-Calabash进阶/)

《Calabash探索4-Calabash踩坑总结》


















