# 前言
本教程基于termux和alpine linux本质上就是一个基于qemu的Linux虚拟机，如果手里有闲置的安卓手机可以将其变为一台Linux服务器，无需root权限。国内镜像源不稳定，不推荐替换，请自行准备科学上网环境。
[所需要的相关资料](https://www.123684.com/s/CC39-jNj0h)
图片加载缓慢点[这里](https://vip.123pan.cn/1681970/yk6baz03t0n000d7w33gz97h524ibt7bDIQ1DIr0Dcx2DIry.jpg)获取图片版教程

# 方法1:已经集成好了alpine的termux——Alpine Term app(已停止维护更新，版本较旧)
安装并打开app给与权限，等待系统启动，再根据提示输入用户名和密码(都是alpine)下面安装过程中需要输入的地方一律输入y即可
![](https://vip.123pan.cn/1681970/ymjew503t0n000d7w32y53ddcr4b2a0eDIQ1DIr0Dcx2DIry.png)
更新软件包索引并升级已安装的软件包
```
apk update && apk upgrade
```
安装docker
```
apk add docker
```
启动docker
```
service docker start
```
切换到root账户
```
sudo -s
```
输入密码alpine
将docker添加到开机自启动
```
rc-update add docker
```
后台启动
```
setsid containerd  
setsid dockerd
```
安装docker管理面板portainer以后就可以直接在portainer上面安装docker项目
```
docker run -d \
--name=portainer-zh \
-p 9000:9000 \
-v /var/run/docker.sock:/var/run/docker.sock \
--restart=always \
6053537/portainer-ce
```
如需远程ssh只需要安装openssh即可
```
apk add openssh
```
开启ssh密码登录
```
sed -i ‘s/PasswordAuthentication no/PasswordAuthentication yes/g’ /etc/ssh/sshd_config  
```
如需开启开机root用户登录
```
sed -i ‘s/PermitRootLogin no/PermitRootLogin yes/g’ /etc/ssh/sshd_config
```
重启sshd服务
```
service sshd restart  
```
## alpine到termux的端口映射
因为是qemu套娃，所以需要映射端口，进入后，手指在软件的左上方向右滑，会出现菜单
如下所示：
![](https://vip.123pan.cn/1681970/ymjew503t0l000d7w32x77qo7h92b969DIQ1DIr0Dcx2DIry.png)
例如映射ssh的22端口到手机的8022
选择 [1]QEMU，输入
```
hostfwd_add tcp::8022-:22
```
在同一局域网下就可以在电脑上通过ssh连接到到手机的虚拟机，ip为手机的ip，端口是映射到手机的8022
后续的服务安装好之后也需要手动映射端口
格式为
```
hostfwd_add tcp::手机端口-:虚拟机端口
```

如将上面的portainer映射到手机上
```
hostfwd_add tcp::9000-:9000
```
就可以在手机上通过http://localhost:9000访问portainer
# 方法2在termux里面安装Alpine Linux(系统更新，版本更高)
更新软件包索引并升级已安装的软件包
```
pkg update && pkg upgrade -y
```
安装必要的软件包
```
pkg install qemu-utils qemu-common qemu-system-x86_64-headless -y
```
```
pkg install wget -y
```
```
pkg install openssl -y
```
# 创建并进入alpine目录
```
mkdir alpine && cd alpine
```
下载Alpine Linux ISO文件
```
wget https://dl-cdn.alpinelinux.org/alpine/v3.20/releases/x86_64/alpine-virt-3.20.0-x86_64.iso
```
创建10G的qcow2镜像文件(动态占用空间并不会直接占用10g空间)
```
qemu-img create -f qcow2 alpine.img 10G
```
创建run.sh脚本
```
cat << 'EOF' > run.sh
```
再输入
```
qemu-system-x86_64 -machine q35 -m 1024 -smp cpus=2 -cpu qemu64 \
  -drive if=pflash,format=raw,read-only=on,file=$PREFIX/share/qemu/edk2-x86_64-code.fd \
  -netdev user,id=n1,hostfwd=tcp::2222-:22 -device virtio-net,netdev=n1 \
  -cdrom alpine-virt-3.20.0-x86_64.iso \
  -drive file=alpine.img,if=virtio \
  -nographic
EOF
```
赋予run.sh可执行权限
```
chmod +x run.sh
```
运行run.sh脚本进入引导系统
```
./run.sh
```
耐心等待系统启动完成，然后输入root进入系统
~~下载快速安装系统的应答文件~~
```
wget https://github.com/xiumuzidiao0/Alpine-Linux/blob/main/answerfile -O answerfile
```
如果下载不了,直接创建文件
```
cat << 'EOF' > answerfile
```
再输入
```
KEYMAPOPTS="us us"
HOSTNAMEOPTS="-n alpine"
INTERFACESOPTS="auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
    hostname alpine
    dns-nameservers 8.8.8.8 8.8.4.4
"
TIMEZONEOPTS="-z Asia/Shanghai"
PROXYOPTS="none"
APKREPOSOPTS="http://dl-cdn.alpinelinux.org/alpine/v3.20/main http://dl-cdn.alpinelinux.org/alpine/v3.20/community"
SSHDOPTS="-c openssh"
NTPOPTS="-c busybox"
DISKOPTS="-v -m sys -s 0 /dev/vda"
EOF
```
打开开机时输出讯息到控制台
```
sed -i -E 's/(local kernel_opts)=.*/\1="console=ttyS0"/' /sbin/setup-disk
```
安装系统至硬盘
```
setup-alpine -f answerfile
```
期间会让你设置root用户密码，需要输入两次
并让你设置一个普通用户(可不设置，建议设置)然后会提醒是否擦除虚拟硬盘数据
输入一次y
再等待安装完成后出现alpine:~#
关机
```
poweroff
```
等待虚拟机关机完成，退出到termux界面出现alpine:~# ~/alpine $
创建启动脚本
```
cat << 'EOF' > start.sh
```
再输入
```
qemu-system-x86_64 -machine q35 -m 1024 -smp cpus=2 -cpu qemu64 \
  -drive if=pflash,format=raw,read-only=on,file=$PREFIX/share/qemu/edk2-x86_64-code.fd \
  -netdev user,id=n1,hostfwd=tcp::2222-:22,hostfwd=tcp::8081-:80 -device virtio-net,netdev=n1 \
  -drive file=alpine.img,if=virtio \
  -boot order=c \
  -nographic
EOF
```
赋予run.sh可执行权限
```
chmod +x start.sh
```
运行run.sh脚本
./start.sh
脚本内容可以自己看着修改
如
* -m 1024: 分配 1024MB (1GB) 的内存给虚拟机。
* -smp cpus=2: 设置虚拟机使用 2 个 CPU 核心。
其它的自行ai
## 添加端口映射
* hostfwd=tcp::2222-:22: 将主机上的 TCP 端口 2222 转发到虚拟机上的 TCP 端口 22 (SSH)。这意味着你可以通过 ssh -p 2222 user@localhost 连接到虚拟机。
 * hostfwd=tcp::8081-:80: 将虚拟机上的 TCP 端口 80 (HTTP)转发到主机上的 TCP 端口 8081
***启动前请先想好需要映射的端口***
其它端口映射添加也都是这个格式，注意**以英文逗号分隔和-device之间有空格**
往后启动只需要在termux中进入alpine即cd alpine,再执行./start.sh即可
docker及其它的参考上面的，可能需要手动安装一下部分软件。
开放root登陆
```
echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config
```
