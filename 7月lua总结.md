# lua工作学习总结

### 基本实现的功能
- [x] QQ和微信的抢红包
- [x] 王者荣耀自动冒险模式刷金币
- [x] 王者荣耀预启动，自动输入账号密码
- [x] QQ自动登录，录入账号
- [x] QQ登录YZM模块
- [ ] 吃鸡的简单自瞄
- [ ] 王者荣耀的简单自动躲技能

- 2018年7月
1.Lua脚本检测不到ms级别的时间，所以检测单次click或者touchClick的时间，用100次的点击的秒数相除；

2.一次安卓设备上左下角在900ms内实现点击三次的操作，发现实现不了，因为100次touchClick 71s左右  100次 click  62s左右 ，单次点击间隔超过600Ms,后来用广播命令实现的进入另一个场景；

3.可以触动精灵文档那里可以学习使用，应用思路，但是不一定api适用；

4.lua脚本先全局读function，当读完有不对，即结束，之后再按照当前执行函数的运行顺序执行，检测是否发生错误，首先是检测function声明有无语法参数等错误；

5.lua的函数声明如果要封装进一个模块内，用类似require "moduleF" 导入，此时注意文件路径，另外如果用到local +函数声明的表达形式，那么注意函数声明的顺序，局部变量时，函数声明中出现的其他函数，必须在之前有local或者全局声明；

``` lua
--lua脚本，验证local变量对执行的影响
	function a()
	    print("code Runner")
	end
	function main()
	    print(c)  --nil,局部变量，类比js，相当于变量的声明在当前执行语句后边，Js有先是声明，然后才赋值的设计模式
	    a()
	    b()  --error，我的理解是local b是局部变量，没有像函数声明一样提升提升，声明和赋值的运行顺序
	end
	local function b()
	    print("b")
	end
	local c=5  
	main()
```
6.lua的循环，for或者while，跳出循环用break或者return，如果是通过把变化的条件赋值时要注意一些小细节，可能会踩坑，js也是；

  7.Lua脚本简化操作一些基本核心点有：

- 找到特殊点，而不触发其他情况； - 特殊坐标+特殊颜色，而不会出现其他情况的干扰；
- 当出现上述符合条件的点point时，{"0xFFFFFF",90,66,66}等时，进行true的判断或者点击； - 这样子的优点是可以适配一个操作中的所有场景，做一个循环，不会因为这段脚本运行是出现在场景中的某一片段而打断；
- 一个大的循环，里面一秒执行一次，然后里面很多个if语句，这样就不会过多影响性能和运行；



8.logcat -s DEBUG 查看lua脚本的运行日志和错误原因，虽然不知道怎么用；
 
9.logcat可以查看当前操作，当前进入的app包名，入口等；
10.不同屏幕分辨率下，按照比例计算出的点的坐标可能不一样，是因为不知道设计者在设计样式时不同分辨率下所作的处理；
11.lua脚本的运行环境有SctTE和VSCode等的插件，建议用VSCode的Code Runner，虽然好像引入其他lua文件有问题，但是其他时候没发现什么bug，更不会出现中文乱码的情况；
12.一些安卓相关的命令：
 - io.write("这是一个公有函数！\n")
 - os.execute("settings put secure   show_ime_with_hard_keyboard 1")
  输入法和外接键盘共存，开启输入法
 - os.execute("input tap "..x.." "..y)
 - enableAdbKeyboard()  开启ADB键盘，QQ，腾讯系游戏需要开启ADB键盘才能输入密码
 - 业务：工程模式将输入法的禁用设置取消掉了

 
