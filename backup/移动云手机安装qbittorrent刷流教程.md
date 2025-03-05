# 前言
本教程只适用移动云手机，其它请自测
移动云手机每周可以免费使用一天，有普通版和极致版，也就是说一个号码一周可以白嫖两天，理论上四个号码就可以实现永久白嫖，而且因为他是可以恢复数据的，所以理论上只用操作一次，后面只需恢复数据即可
有很多地区，比如北京、四川有移动的优惠，可以白嫖一段时间甚至永久，其他部分地区也有10块钱一个月的套餐，自行谷歌，直接购买36元一月没有什么性价比
由于其底层的限制，部分操作需要root权限，直接使用其定制版的面具修补后重启即可
**下载安装完termux后，到设置里手动给他存储空间权限(必须)** 和通知权限(非必须)
所需资料https://www.123684.com/s/CC39-FMj0h
# 安装教程
## 面具修补教程
安装并启动magisk，要下载我的网盘里的定制版的magisk，进入软件
* 点击上面的那个安装按钮
* 点击下一步
* 点直接安装
* 待安装完成，手动重启云手机(不是右下角的那个按钮，点了也没用)
## 进入termux安装qbittorrent-nox
以下命令执行过程中，如有需要输入的地方全部输入y即可
1.执行以下命令更新termux环境
```
pkg update -y && pkg upgrade -y
```
2.安装proot-distro
```
pkg install proot-distro
```
3.安装tsu
```
pkg install tsu
```
4.输入tsu获取权限
```
tsu
```
5.安装debian
```
proot-distro install debian
```
6.登录debian
```
proot-distro login debian
```
7.更新debian环境
```
apt update -y && apt upgrade -y
```
8.安装qbittorrent-nox
```
apt install -y qbittorrent-nox
```
9.启动qbittorrent-nox
由于默认的8080端口被系统占用，因此需要自定义，可自行更改这里的8181，**注意如更改要把下面的相关端口的配置项的也一同更改** 
```
qbittorrent-nox --webui-port=8181
```
~~10.往后启动qbittorrent只需要执行~~
```
qbittorrent-nox
```
# 修改qbittorrent配置
到浏览器中输入http://0.0.0.0:8181 (这里的端口与上面的自定义端口相对应，若有更改自行修改)进入webui
默认用户名为:admin
密码为:adminadmin
关闭掉
**启用跨站请求伪造(CSRF)保护**
**启用host header属性验证**
**否则无法外网访问**
![](https://vip.123pan.cn/1681970/yk6baz03t0n000d6xujp03kzmtxxov5aDIQ1DIr0Dcx2DIry.png)]
并自行修改用户名、密码
# frpc配置
## 免费的
可使用免费的樱花frp 官网:https://www.natfrp.com/
注册和实名认证这里不演示，很简单就是实名需要1元钱，且官网上面有更详细的教程，想要了解更多请去官网这里只做最基础的教程
**详见视频教程**
## 自建frp穿透
```
serverAddr = "2.2.2.2" #frps服务端公网IP
serverPort = 5443 #frps服务端公网端口

auth.method = "token"
auth.token = "a123456789" #frps的token

[[proxies]]
name = "ydqb" 
type = "tcp"
localPort = 8181
remotePort = 18181
```
frps以及后续的刷流工具的安装请自行谷歌
配置好后启动，即可在公网(如http://2.2.2.2:18181)访问，搭配一些刷流的工具如vt和mp的刷流插件开始开心地刷起来
# 关于在其他云手机上使用、数据恢复和意外退出termux
## 其云手机上使用
由于移动云手机部分文件访问权限的问题，需要root权限，其他的可能不需要，也就不需要安装magisk和tsu，也就是可以省略3、4两步
其它一样
## 数据恢复和termux意外退出
数据恢复后会掉面具(magisk)需要重新再修补一遍，重启过后，只需要打开termux并执行第4、6、10步即可
# 可能出现的问题
termux的仓库可能拉不下来(特别是debian镜像)可以给termux单独开代理，切忌开全局代理，会直接导致云手机断连，如有不小心开了可尝试重启云手机恢复。
clash meta请自行到github下载（网盘里的被屏蔽了）链接https://github.com/MetaCubeX/ClashMetaForAndroid/releases/tag/v2.11.6
# 抛砖引玉——另一个更方便的方法(仅限magisk可用)有我解决不了的问题，请大佬们有兴趣的研究一下，并不吝赐教，下面是一些我的经验。（模块是酷安大佬搞的）
目前可以在安装模块并重启后直接在浏览器里面打开webui,但是添加种子有问题，不论是bt还是pt种子都显示Host not found(non-authoritative),try again later
大家可以打开压缩包里的bin目录并到https://github.com/userdocs/qbittorrent-nox-static/releases里面去下载对应手机架构(一般是aarch64)的不同版本的文件替换一下就可以实现版本切换，但是要注意新的几个qbittorrent版本都是没有默认密码的需要自己去修改一，如没有修改，那默认用户名密码就是admin和adminadmin
在qBittorrent/config目录里面的qBittorrent.conf文件在[Preferences]下添加一条WebUI\Password_PBKDF2="@ByteArray(ruz0RpHqKptKv44/ZacGKA==:v6n/OXeMd5qASDJL8uEJsJVKk3LSrFXroxusykX929FQ24/rYIRWeWNfXrsJcBCvXsFmyNGSLF0GMj/bEDb2Fg==)"即使默认密码为adminadmin再重启手机即可
## 以下是一些ai的方法(似乎没有什么用)
好的，安卓上通过 Magisk 模块安装 qBittorrent-nox 后出现 "Host not found (non-authoritative), try again later" 错误，通常表明 DNS 解析出现了问题。以下是一些可能的解决方法：

**1. 检查网络连接：**

*   **确保网络连接正常：** 检查你的安卓设备是否连接到可用的 Wi-Fi 或移动数据网络。
*   **尝试重启网络：** 尝试关闭并重新打开 Wi-Fi 或移动数据，或者重启你的路由器。

**2. 检查 qBittorrent-nox 配置：**

*   **监听地址：** 确保 qBittorrent-nox 监听的地址是正确的。默认情况下，它应该监听 `0.0.0.0`，允许所有 IP 地址访问。你可以在 qBittorrent-nox 的配置文件中修改监听地址。
*   **端口：** 确保你使用的端口没有被其他应用占用，并且防火墙允许通过这些端口。
*   **Web UI 设置：** 检查 Web UI 的设置是否正确，确保启用了 Web UI 并且配置了正确的端口。

**3. 修改 DNS 设置：**

*   **使用公共 DNS 服务器：** 尝试修改你的安卓设备的 DNS 设置，使用公共 DNS 服务器，例如 Google DNS (8.8.8.8 和 8.8.4.4) 或 Cloudflare DNS (1.1.1.1)。
    *   **Wi-Fi：** 在 Wi-Fi 设置中，修改当前连接的 Wi-Fi 网络的 DNS 设置。
    *   **移动数据：** 修改移动数据的 DNS 设置可能需要使用一些第三方应用，例如 DNS Changer。
*   **Termux DNS：** 如果你在 Termux 中运行 qBittorrent-nox，可以尝试修改 Termux 的 DNS 设置。

**4. 检查防火墙设置：**

*   **安卓防火墙：** 某些安卓设备可能自带防火墙，检查防火墙设置是否阻止了 qBittorrent-nox 的网络连接。
*   **Magisk 模块：** 某些 Magisk 模块可能会修改防火墙设置，导致 qBittorrent-nox 无法连接网络。尝试禁用这些模块，看看是否能够解决问题。

**5. 检查 Host 文件：**

*   **Host 文件冲突：** 检查你的安卓设备的 Host 文件 (`/system/etc/hosts`) 是否有冲突的 DNS 记录。你可以使用 Magisk 模块，例如 Hosts Editor，来编辑 Host 文件。
*   **删除不必要的记录：** 删除 Host 文件中与 qBittorrent-nox 相关的任何不必要的记录。

**6. 检查 Magisk 模块：**

*   **模块冲突：** 某些 Magisk 模块可能会与 qBittorrent-nox 冲突，导致 DNS 解析问题。尝试禁用其他 Magisk 模块，看看是否能够解决问题。
*   **更新模块：** 确保你使用的 qBittorrent-nox Magisk 模块是最新版本。

**7. 重新安装 qBittorrent-nox：**

*   **卸载模块：** 卸载 qBittorrent-nox Magisk 模块。
*   **清除数据：** 清除 qBittorrent-nox 的数据和缓存。
*   **重新安装：** 重新安装 qBittorrent-nox Magisk 模块。

**8. 其他建议：**

*   **重启设备：** 尝试重启你的安卓设备。
*   **检查日志：** 查看 qBittorrent-nox 的日志文件，可能会有更详细的错误信息。
*   **搜索：** 在网上搜索 "qBittorrent-nox Host not found" 可能会找到其他用户遇到的类似问题和解决方法。

**具体步骤示例（修改 DNS 设置）：**

1.  **进入 Wi-Fi 设置：** 打开安卓设备的设置，进入 Wi-Fi 设置。
2.  **修改 DNS：** 长按当前连接的 Wi-Fi 网络，选择“修改网络”。
3.  **高级选项：** 点击“高级选项”，找到 IP 设置。
4.  **静态 IP：** 将 IP 设置从 DHCP 改为“静态”。
5.  **DNS 1：** 将 DNS 1 设置为 `8.8.8.8` (Google DNS)。
6.  **DNS 2：** 将 DNS 2 设置为 `8.8.4.4` (Google DNS)。
7.  **保存：** 保存设置，重新连接 Wi-Fi。

**请注意：**

*   修改系统设置可能存在风险，请务必小心操作，并备份重要数据。
*   如果以上方法都无法解决问题，可能需要寻求更专业的帮助。

希望这些方法能够帮助你解决问题。 如果你有任何其他问题，请随时提出。
