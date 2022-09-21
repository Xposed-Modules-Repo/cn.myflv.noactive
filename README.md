**NoActive正如其名，让Android后台CPU不再活跃**

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

7、Hook获取PMS为冻结后的APP释放唤醒锁

8、Hook接收小米Millet网络临时解冻APP

9、Hook接收小米Binder通知临时解冻APP

10、Hook获取NMS为冻结后的APP断开网络连接

11、Hook控制AppStandby为冻结后的APP限制后台

**电量和性能(MIUI)**：
1、Hook杀进程方法，阻止电量性能杀后台
2、禁用millet，该功能与NoActive重复

**冻结方式说明**：

目前Linux进程冻结方式有Kill -19、Kill -20、Cgroup Freezer V1、Cgroup Freezer V2

推荐 API & V2 > Kill -19 & Kill -20

首推API和V2其次Kill，小米MIUI13机子可以使用V1，其他机子使用V1会有内存泄露

Freezer天然优势，Kill存在一段时间后进程还在打开仍然重载

部分机子存在系统框架无法读写Freezer目录，可以开启提权模式，与Thanox的Su插件一样

关于白名单推送进程，需要同时白名单主进程，才不会被StandBy限制后台

MIUI对APP开启保持连接，即可启用网络解冻，微信等即时通信APP必备

**日志说明**：

日志级别分为debug(调试信息)、info(基本信息)、warn(警告信息)、error(错误信息)

**NoActive交流QQ群750812133**


> 如果你觉得模块不错，可以打赏开发者一瓶可乐(理性打赏)

![如果你觉得模块不错，可以打赏开发者一瓶可乐][1]



  [1]: https://m.360buyimg.com/babel/jfs/t1/112365/9/29244/36766/62e68cadE30683ff1/c4e6d9ef81b69e3c.jpg
  [2]: https://coolstars.lanzoum.com/i0wkH0a1se6f
