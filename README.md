# 使用xRay和OpenWrt SSR Plus+搭建UDP游戏加速器来改善NAT类型

1. 在服务器端安装最新版本的xRay: [官方文档](https://github.com/XTLS/Xray-install#readme) 或（[YouTube推荐配置视频](https://www.youtube.com/watch?v=7GHh91AYAmM)）
1. 配置xRay的代理方式为VLESS over XTLS：[官方配置示例](https://github.com/XTLS/Xray-examples/tree/main/VLESS-TCP-XTLS-WHATEVER) 或（[YouTube推荐配置视频](https://www.youtube.com/watch?v=7GHh91AYAmM)）
1. 开放服务器端防火墙`UDP 1024-65535端口`
1. 在OpenWrt端升级xRay-core文件到1.3版本以上（OpenWrt系统-软件包）
1. 在OpenWrt中停止SSR Plus+服务
1. 使用SSH以root用户登录OpenWrt
1. 使用命令`cp /usr/bin/v2ray /usr/bin/v2ray.bk`备份`/usr/bin/v2ray`二进制可执行文件
1. 使用xray文件覆盖v2ray文件：`cp /usr/bin/xray /usr/bin/v2ray`
1. 在SSR Plus+配置VLESS（步骤2中已配置），流控（FLow）选择`xtls+rprx+direct+udp433`，并且启动VLESS。
1. 检查SSR Plus+日志中是以**xRay**运行的VLESS
1. 在`SSR Plus+ -- 访问控制 -- LAN IP访问控制 -- 增强游戏模式客户端LAN IP` 将内网中的游戏机IP地址填入。
1. 使用Nintendo Switch测试NAT类型会得到A；使用Xbox测试NAT类型会得到Open

> 注意：
>- 这种方法只会改善NAT类型，但并不会改善网络延迟和质量（Xbox网络检测建议网络延迟在60ms以下）
>- 可能是由于SSR Plus+中的某种bug，即使指定了数据包编码类型为xudp(xray-core)，SSR Plus+也不会使用xray-core执行VLESS 【待确认】，只会使用v2ray。所以只能手动将v2ray二进制可执行文件替换为xray
>- 此种方法应该不局限于VLESS+XTLS（在xRay中支持的其他协议未测试是否可以进行UDP转发来改善NAT类型, 但是根据xRay-core 1.3版本之后的描述应该是可以的）
>- 关于`代理协议 UDP 全方位透彻解析`请参阅xRay大佬RPRX[力作](https://github.com/XTLS/Xray-core/discussions/237)

> 优点：
>- 使用此方法可直接使用OpenWrt作为主路由器为游戏机进行加速，无需在内网中再部署其他设备，也无需修改游戏机的LAN配置
>- 此方法通过了[NatTypeTester](https://github.com/HMBSbige/NatTypeTester)的严格测试
>- Nintendo switch和Xbox也均测试成功
>- UU加速器就这样死了？


