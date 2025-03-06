# 前言
本教程基于另外三篇教程精简
* [安卓手机使用docker教程](https://blog.050802.xyz/post/an-zhuo-shou-ji-shi-yong-docker-jiao-cheng.html)
* [云手机安装刷流工具vertex教程](https://blog.050802.xyz/post/yun-shou-ji-an-zhuang-shua-liu-gong-ju-vertex-jiao-cheng.html)
* [在云手机内给指定应用开代理](https://vip.123pan.cn/1681970/ymjew503t0l000d7w32x77t3xl96wk0uDIQ1DIr0Dcx2DIry.jpg)(必需先看并配置好)
精简了一些不相关的步骤，同时不再过多解释操作，如有兴趣自行去看完整的教程。
# 方法1:已经集成好了alpine的termux——Alpine Term app(已停止维护更新，版本较旧)
##
app下载点[这里](https://www.123684.com/s/CC39-jNj0h)
安装并打开app授予权限，等待系统启动，再根据提示输入用户名和密码(都是alpine)下面安装过程中需要输入的地方一律输入y即可，一行一个回车
![](https://vip.123pan.cn/1681970/ymjew503t0n000d7w32y53ddcr4b2a0eDIQ1DIr0Dcx2DIry.png)
```
sudo -s
```
输入密码
```
alpine
```
```
apk update && apk upgrade
```
```
apk add docker
```
```
service docker start
```
```
rc-update add docker
```
## 安装vt
```
docker pull lswl/vertex-base:latest
```
```
docker run -d \
  --name vertex \
  -v /var/docker/vertex:/vertex \
  -p 3005:3000 \
  -e TZ=Asia/Shanghai \
  --restart unless-stopped \
  lswl/vertex:stable
```
```
docker run -d \
  -v /var/docker:/srv \
  -v /var/docker/filebrowser:/config \
  -p 1080:80 \
  --name filebrowser \
  --restart unless-stopped \
  filebrowser/filebrowser:latest
```
## 映射端口
手指在软件的左上方向右滑，会出现菜单
如下所示：
![](https://vip.123pan.cn/1681970/ymjew503t0l000d7w32x77qo7h92b969DIQ1DIr0Dcx2DIry.png)
```
hostfwd_add tcp::3005-:3005
```
```
hostfwd_add tcp::1080-:1080
```
## 配置vt
* 获取vt登录密码
在浏览器打开http://localhost:1080
进入vertex/data文件夹，打开password文件，复制内容
* 登录vt并修改密码
在浏览器打开http://localhost:3005
默认用户名admin
密码为刚才复制的内容
自行去 系统设置 >安全设置 里面修改密码
* 导入删种规则
在浏览器打开http://localhost:1080
* 删种规则文件
进入/vertex/data/rule/delete将下载解压出来的文件上传上去即可
* rss相关请参考[云飞的教程](https://yunfeipt.flowus.cn/)
## **注意事项**
* 关于软件退出
只需要等进入系统后重新执行一遍端口映射即可
### **vt关联qb的ip填写(重要)**
在termux输入ifconfig
得到[如图](https://vip.123pan.cn/1681970/ymjew503t0m000d7w32xoua6xqow1rbwDIQ1DIr0Dcx2DIry.png)的回复，其中以10开头的ip地址就是在vt中填写的qb地址
![](https://vip.123pan.cn/1681970/ymjew503t0m000d7w32xoua6xqow1rbwDIQ1DIr0Dcx2DIry.png)
# 方法2敬请期待。。。。