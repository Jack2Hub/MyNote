

# 安装
- [VMware Workstation上安装Ubuntu 16.04 Server操作系统](https://blog.csdn.net/gongxifacai_believe/article/details/52454814)

# 环境配置

## 网络配置

### NAT 模式

#### 设置静态IP
> **亲测**：需要先安装 **resolveconf**，否则修改interfaces会不起作用
- VMware 编辑 --> 虚拟机网络编辑器 --> 选择NAT模式
  - 取消dhcp服务，并记住网络信息： **子网ip/网关**
- ubuntu虚拟机内修改
  - /etc/network/interfaces // **此文件的配置信息优先级最高**
    ``` c++
    auto lo
    iface lo inet loopback  // 一般这两行都有，回环用

    auto ens33              // 不一定是eth0
    iface ens33 inet static // 设置静态ip模式
    address 192.168.66.100  // 设置ip
    netmask 255.255.255.0
    gateway 192.168.66.2    // 必须与上一v步中看到的网络编辑器里的网络信息一致

    dns-nameservers 8.8.8.8 // 多台dns服务器ip用空格隔开
    ```
  - /etc/resolve.conf   // 此文件会被覆盖，重启失效，可临时用
  - /etc/resolveconf/resolve.conf.d/base
    - 需要安装resolveconf  `sudo apt-get install resolveconf`

#### 虚拟机与宿主机互相ping通
- 虚拟机能ping通宿主机，宿主机不能ping通虚拟机
  > 在上一步设置完静态ip的步骤后，在windows下查看 **VMnet8** 网卡，可以发现其网段没有和虚拟机的网段一致，修改成一致即可

## 修改用户名
- [Ubuntu下更改用户名和主机名](https://www.cnblogs.com/zeusmyth/p/6231350.html)
> 步骤简单但是顺序重要，严重会导致**重启后无法登录**
  - 更改主机名
    - 修改 **hostname** 文件  `sudo vim /etc/hostname`
    - 修改 **hosts** 文件     `sudo vim /etc/hosts`
      - 之后建议重启，否则有些命令执行会比较慢
  - 更改用户名
    - 修改 **sudoer** 文件
      - `sudo vim /etc/sudoers`
        > 先将赋予要修改名称的最高权限，防止后续步骤出现权限不足
    - 修改 **shadow** 文件
      - `sudo vim /etc/shadow`
      ``` shell
      username: passwd: lastchg: min: max: warn: inactive: expire: flag
      登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志
      ```
    - 修改 **/home** 目录名称
      - `sudo mv /home/jack /home/jack.huang`
    - 修改 **passwd** 文件
      - `sudo vim /etc/passwd`
      ``` shell
      用户名: 密码 : uid  : gid :用户描述：主目录：登陆shell
      ```
    - 修改 **用户组**
      - `sudo vim /etc/group`
    - 修改 **sudoer** 文件
      - 到此已经修改好了用户名，可以将原先的用户名sudo权限删除
  - `sudo reboot -f`



## 交叉编译
## python
- /CODE/Python/[basic.md

# 软件配置
- 源配置
  - `sudo vi /etc/apt/source.list`
  - `sudo apt update`
- 网络配置
  - 一开始可能没用ifconfig，需要安装`sudo apt-get install net-tools`
  - 主机一开始可能ssh连不上虚拟机, 需要安装ssh-server
    - `sudo apt-get install openssh-server`, **openssh-client** 一般ubuntu默认安装
    - ps -ef | grep ssh: 有grep 到 **sshd** 才算开启server，或者`sudo /etc/init.d/ssh start`这么开启
- 编辑配置
  - VIM
    - 一开始只有VI编辑器, 故在编辑器中可能会出现 **退格键或者方向键** 失灵的现象, 需要安装VIM
    - `sudo apt-get remove vim-common` 和 `sudo apt-get install vim`
    ```
    github上一个vim配置，可以参考使用
    $ git clone https://github.com/chxuan/vimplus.git ~/.vimplus
    $ cd ~/.vimplus
    $ ./install.sh
    ```
- 其他配置
    `
    apt-get install dos2unix git git-core gnupg:i386 flex:i386 gperf:i386 doxygen build-essential zlib1g-dev:i386 lib32ncurses5-dev x11proto-core-dev libx11-dev:i386 lib32readline6-dev lib32z1-dev g++-4.8-multilib mingw32 tofrodos python-markdown libxml2-utils:i386 lzop:i386 rar liblzo2-dev:i386 fakeroot:i386 libuuid1:i386 squashfs-tools:i386 gawk:i386 libjpeg62-dev:i386 libcairo2-dev:i386 libgtk2.0-0:i386 phablet-tools realpath unzip samba unrar
    `



