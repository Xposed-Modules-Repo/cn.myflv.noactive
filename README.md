NoActive正如其名，让Android后台CPU不再活跃

**模块功能介绍**：
通过Hook系统框架实现Android墓碑

**作用域说明**：

**系统框架**：
1、Hook应用切换事件，冻结切换至后台的应用，解冻切换至前台的应用
2、Hook广播分发事件，屏蔽被冻结的应用接收广播，从而避免触发广播ANR
3、Hook应用Activity休眠事件，阻止休眠长时间不活跃的Activity(休眠后存在打开重载、最近任务中消失问题)
4、Hook系统ANR事件，由于冻结之后，应用无法做出响应被系统认为是ANR，所以需要屏蔽ANR避免系统误杀被冻结的APP
5、Hook系统是否开启暂停执行已缓存变量获取，由于系统自带的暂停执行已缓存在收到广播后可能解冻再次活跃
6、Hook显示暂停执行已缓存开关，仅显示无作用(Happy freezer)

**电量和性能(MIUI)**：
1、Hook杀进程方法，阻止电量性能杀后台
2、禁用millet，该功能与NoActive重复

**冻结方式说明**：

目前Linux进程冻结方式有Kill -19、Kill -20、Cgroup Freezer V1、Cgroup Freezer V2
Kill -19和Kill -20兼容性最好，但是存在Bug，进程还在依然重载
Google官方使用Cgroup Freezer V2
NoActive仅仅作用于系统框架，不是Root权限，权限不足
Kill使用Android的Process.sendSignal，该方法为安卓封装间接调用Kill，所以可能存在部分系统19有效或者20有效，需要自测
Cgroup Freezer V1和V2采用NoActive参考millet自行实现并封装，或V2调用安卓Process.setProcessFrozen实现
所以NoActive支持5种冻结方式分别为Kill -19、Kill -20、Cgroup Freezer V1(NoActive)、Cgroup Freezer V2(NoActive)、Cgroup Freezer V2(系统API)
由于对System权限不足导致无法读取配置判断Cgroup Freezer版本，故Hook获取系统是否支持暂停执行已缓存来判断V2、其余皆为V1，如果测试没有效果，或者冻结error报错，请选择Kill方式，配置方式参考下面的配置文件说明。

**如果5种模式都无法生效，可以切换Kill -19模式，在每次开机后执行以下命令**

    su
    magiskpolicy --live "allow system_server * process {sigstop}"

**配置文件说明**：

目录 /data/system/NoActive

**即时生效配置**：

blackSystemApp.conf 系统黑名单(系统APP默认白名单)
killProcess.conf 杀死进程名单(后台3S杀死进程)
whiteApp.conf 白名单APP(用户APP默认黑名单)
whiteProcess.conf 白名单进程(添加白名单APP无需添加)

**重启生效配置(需要自己新建)**：

debug 开启调试日志
disable.oom 禁用修改oom_adj功能
kill.19 使用Kill -19冻结
kill.20 使用kill -20冻结
freezer.v1 使用Cgroup Freezer V1(NoActive)冻结
freezer.v2 使用Cgroup Freezer V2(NoActive)冻结
freezer.api 使用Cgroup Freezer API(系统API)冻结

**日志说明**：

日志级别分为debug(调试信息)、info(基本信息)、warn(警告信息)、error(错误信息)

**NoActive交流QQ群750812133**

最新下载地址：[https://coolstars.lanzoum.com/i0wkH0a1se6f][2]


> 如果你觉得模块不错，可以打赏开发者一瓶可乐(理性打赏)

![如果你觉得模块不错，可以打赏开发者一瓶可乐][1]



  [1]: https://m.360buyimg.com/babel/jfs/t1/112365/9/29244/36766/62e68cadE30683ff1/c4e6d9ef81b69e3c.jpg
  [2]: https://coolstars.lanzoum.com/i0wkH0a1se6f
