# 前言
本教程基于安卓手机使用docker教程请先看完，并做完[前置步骤。](https://blog.050802.xyz/post/an-zhuo-shou-ji-shi-yong-docker-jiao-cheng.html)注意要先进入root用户，安装完后记得进行端口映射和关代理。
[云手机代理教程](https://vip.123pan.cn/1681970/ymjew503t0l000d7w32x77t3xl96wk0uDIQ1DIr0Dcx2DIry.jpg)
# 教程
* vt刷流版启动脚本(加上了portainer和vt的端口映射加大了分配的内存)
```
qemu-system-x86_64 -machine q35 -m 2048 -smp cpus=2 -cpu qemu64 \
  -drive if=pflash,format=raw,read-only=on,file=$PREFIX/share/qemu/edk2-x86_64-code.fd \
  -netdev user,id=n1,hostfwd=tcp::2222-:22,hostfwd=tcp::1080-:1080,hostfwd=tcp::3005-:3005,hostfwd=tcp::9000-:9000 -device virtio-net,netdev=n1 \
  -drive file=alpine.img,if=virtio \
  -boot order=c \
  -nographic
EOF
```
* vt安装
使用 Docker 命令安装
先拉取一次底包 (正常情况下底包不会更新, 只是拉取镜像, 不用创建容器)
用户名密码登录地址localhost:3005
默认用户名为 admin, 初始密码在 vertex/data/password 文件内, 用下面这个文件管理工具打开直接复制即可 登入之后务必在全局设置页修改账号及密码
```
docker pull lswl/vertex-base:latest
```
命令内的 /var/docker/vertex 为 Vertex 的数据保存目录, 可以更换至任一有读写权限的目录，不懂docker的不要改，改了下面的filebrowser也得改。
```
docker run -d \
  --name vertex \
  -v /var/docker/vertex:/vertex \
  -p 3005:3000 \
  -e TZ=Asia/Shanghai \
  --restart unless-stopped \
  lswl/vertex:stable
```
* 安装docker文件管理工具(默认用户名和密码都是admin访问地址localhost:1080)
```
docker run -d \
  -v /var/docker:/srv \
  -v /var/docker/filebrowser:/config \
  -p 1080:80 \
  --name filebrowser \
  filebrowser/filebrowser:latest
```
## vt关联qb的ip填写
* **记得把端口映射出来，如果有访问外部服务的也需要映射**
如这里需要访问外部的qb就映射了8181端口
在termux输入ifconfig
得到如图的回复，其中以10开头的ip地址(除127.0.0.1和0.0.0.0以外都可以试一试，如可能出现的172开头的IP)就是在vt中填写的qb地址
![](https://vip.123pan.cn/1681970/ymjew503t0m000d7w32xoua6xqow1rbwDIQ1DIr0Dcx2DIry.png)
# 最后分享我的删种规则，已经稳定使用一周
将压缩包文件下载并解压出来，把解压出来的文件上传到/vertex/data/rule/delete即可
https://www.123684.com/s/CC39-lNj0h