# linux
## 基础
    1. linux中所有内容以文件形式保存,包括硬件
        1. 硬件文件是/dev/sd[a-p]
        2. 光盘文件是/dev/sr0
    2. linux文件没有扩展名
    3. linux所有存储设备都必须挂载之后用户才能使用,包括硬盘,U盘,光盘
    4. 目录和作用
        1. 和命令相关的目录(binary命令)
            1. /bin/存放系统命令的目录,普通用户和超级用户都可以执行,不过放在/bin下的命令在单用户模式下也可以执行
            2. /sbin/保存和系统环境设置相关的命令,只有超级用户可以使用这些命令进行系统环境设置,但是有些命令可以允许普通用户查看
            3. /usr/bin存放系统命令的目录,普通用户和超级用户都可以执行,这些命令和系统启动无关,在单用户模式下不能执行(单用户: 类似于windows下的安全模式)
            4. /usr/sbin存放根文件系统不必要的系统管理命令,例如多数服务程序,只有超级用户可以使用(linux中所有sbin目录中保存的命令只有超级用户可以使用,bin目录中保存的命令所有用户都可以使用)
        2. /boot系统启动目录,保存系统启动相关的文件,如内核文件和启动引导程序(grub)文件等
        3. /dev硬件设备文件保存位置
        4. /etc配置文件保存位置,系统内所有采用默认安装方式rpm安装的服务的配置文件全部都保存在这个目录当中,如用户账户和密码,服务的启动脚本,常用服务的配置文件等
        5. /home普通用户的家目录,建立每个用户时,每个用户要有一个默认登录位置,这个位置就是这个用户的家目录,所有普通用户的家目录就是在/home下建立一个和用户名相同的目录,如用户user1的家目录就是/home/user1
        6. /lib系统调用的函数库保存位置
        7. /lost+found当系统意外崩溃或机器意外关机,而产生一些文件碎片放在这里.当系统启动的过程中fsck(file system check)工具会检查这里,并修复已经损坏的文件系统,这个目录只在每个分区中出现,例如/lost+found就是根分区的备份恢复目录,/boot/lost+found就是/boot分区的备份恢复目录
        8. 挂载目录
            1. /media挂载目录,系统建议使用来挂载媒体设备,例如软盘和光盘
            2. /mnt挂载目录,建议挂载额外设备,如U盘,移动设备和其他操作系统的分区
            3. /misc挂载目录,系统建议挂载NFS服务的共享目录
        9. /opt第三方安装的软件保存位置,更常用的方式是放到/usr/local目录中,
        10. 虚拟文件系统(保存在内存中,而不是硬盘中)
            1. /proc虚拟文件系统,主要保存系统的内核,进程,外部设备状态和网络状态灯,例如/proc/cpuinfo保存CPU信息,/proc/devices保存设备驱动的列表,/proc/filesystems用来保存文件系统列表,/proc/net保存网络协议
            2./sys虚拟文件系统,与/proc目录类似,都是保存在内存中的,主要是保存内核相关信息的
        11. /root超级用户的家目录,普通用户的家目录在/home下,超级用户的家目录在/下
        12. /srv服务数据目录,一些系统服务启动之后,可以在这个目录中保存所需要的数据
        13. /tmp临时目录,系统存放临时文件的目录,该目录下所有用户都可以访问和写入,建议不存放重要数据
        14. /usr系统软件资源目录(Unix Software Resource),不是存放用户数据,而是存放系统软件资源的目录,系统中安装的软件大多保存在这里
        15. /var动态数据保存的位置,主要保存缓存,日志以及软件运行时产生的文件
    5. 服务器注意事项
        1. 远程服务器不允许关机,只能重启
        2. 重启时应该关闭服务
        3. 不要在服务器访问高峰运行高负载(例如压缩,扫描,复制等)命令
        4. 远程配置防火墙时不要把自己踢出服务器
## linux常用命令
### 命令格式
    1. 命令 [-选项] [参数]
    2. 个别命令不遵循此格式
    3. 当有多个选项时可以写在一起
    4. 简化选项与完整选项 -a等于--all
### 目录处理命令
    1. ls
        1. 原义list
        2. 命令所在路径/bin/ls
        3. 执行权限:所有用户
        4. 功能: 显示目录文件
        5. 语法
            1. ls 选项[-ald] [文件或目录]
            2. -a显示所有文件,包括隐藏文件(隐藏文件以点开头)(all)
            3. -l详细信息显示(long)
            4. -d查看当前所在这个目录的属性(directory)
            5. -h人性化显示(human)
            6. -i查看文件的内部序列号(inode索引节点)(硬链接的i节点与原文件相同,一个i节点对应多个文件)
        6. 例子(ls -al)
            1. dr-xr-xr-x.  5 root root  4096 Oct 15  2017 boot
                2. dr-xr-xr-x.权限
                    1. d文件类型: -二进制文件d目录l软链接文件
                    2. r-x r-x r-xu所有者 g所属组 o其他人 r读 w写 x执行
                3. 5引用计数
                4. root所有者(唯一)(user)
                5. root所属组(唯一)(group)
                6. 4096文件大小(b)
                7. Oct 15 2017修改时间
                8. boot文件名
    2. mkdir
        1. 原义make directory
        2. 选项
            1. -p递归创建
            2. 可以创建多个目录
        3. 例子
            1. mkdir -p /tmp/1/1 /tmp/2/2
    3. cd
        1. .当前目录,..上一级目录
        1. 例子
    4. pwd
        1. 原义print working directory
    5. rmdir
        1. 原义remove directory
        2. 功能: **删除空目录**
        3. 例子
            1. rmdir /tmp/1/1
    6. cp
        1. cp -rp [原文件或目录] [目标目录]
        2. 选项
            1. -r复制目录时需要加上
            2. -p保留文件属性(例如: 修改时间)
        3. 例子
            1. cp -r /tmp/1/1 /root
            2. cp /root/1.log /root/2.log /tmp
            3. cp -r /tmp/1 /root/2 复制+改名
    7. mv
        1. 原义move
        2. 功能: 剪切或改名
        3. 例子
            1. mv /tmp/1 /root把tmp下的1这个目录剪切到root下
            2. mv 1 /root把当前目录下1这个目录剪切到root下
            3. mv /tmp/1 /root/2把tmp下的1这个目录剪且到root下并改名为2
            4. mv 1 /tmp/2改名
    8. rm
        1. 功能: 删除目录或文件
        2. 选项
            1. -r删除目录时添加
            2. -f强制删除(不会提示是否删除)
        3. 例子
            1. rm -rf /tmp/1
    9. 
### 文件处理命令
    1. touch
        1. 功能: 创建空文件
        2. 命令所在路径/bin/touch
        3. 执行权限: 所有用户
        4. 例子
            1. touch test.list在当前目录下创建文件
            2. touch /root/test.list在root目录下创建文件
            3. touch "program files"不建议文件名加空格
    2. cat
        1. 功能: 显示文件内容
        2. 选项
            1. -n显示行号(number)
    3. tac
        1. 功能: 显示文件内容(倒序显示)
    4. more
        1. 功能: 分页显示文件内容
        2. 快捷键
            1. 空格或f下一页
            2. b上一页
            3. Enter下一行
            4. q或Q退出
    5. less
        1. 功能: 分页显示文件内容
        2. 快捷键
            1. 空格或f或PgDn下一页
            2. b或PgUp上一页
            3. 方向键上下上一行下一行
            4. q或Q退出
            5. /xxx高亮显示xxx内容
                1. n下一个
                2. p回到第一个
    6. head
        1. 显示文件前n行
        2. 选项
            1. -n行数(默认为10)
        3. 例子
            1. head -n 5 /etc/services显示前5行
    7. tail
        1. 显示文件后n行
        2. 选项
            1. -n行数(默认为10)
            2. -f动态显示
### 链接命令
    1. ln
        1. 功能: 生成链接文件
        2. 英文原义: link
        3. 命令所在路径: /bin/ln
        4. 选项
            1. -s创建软链接(不填写选项表示硬链接)
        5. 例子
            1. ln -s [原文件] [目标文件]
            2. ln -s /etc/issue /tmp/issue.soft创建issue的软链接issue.soft
            3. ln /etc/issue /tmp/issue.hard创建issue的硬链接issue.hard
        6. 软链接特征
            1. 显示详细信息的第一位是l, 软链接的权限是777
            2. 无法通过软链接找到删除后的原文件
            3. 类似于windows下的快捷方式
        7. 硬链接特征
            1. 除了路径和文件名不相同,其他所有信息(权限, 修改时间等)均相同(类似于cp -p命令)
            2. 原文件和硬链接文件同步更新
            3. 原文件删除后不影响硬链接
            4. 硬链接的ls -i节点值与原文件相同
            5. 不能跨分区生成硬链接文件
            6. 不允许将硬链接指向目录
            7. 类似于windows下的拷贝+同步更新

### 权限管理命令
||权限|对文件的含义|对目录的含义|
|-|:-|:-|:-|
|r|读权限|可以查看文件内容(cat等)|可以列出目录中的内容(ls)|
|w|写权限|可以修改文件内容|可以在目录中创建或删除文件(rm)|
|x|执行权限|可以执行文件|可以进入目录|
    1. chmod
        1. 语法: chmod [{ugoa}{+-=}{rwx}] [文件或目录]
        2. 选项:
            1. -R: 递归查询(修改目录权限时,目录下所有文件权限都被修改)
        3. root和所有者可以修改权限
        4. 例子:
            1. chmod u=x,g-r [文件或目录]
            2. chmod 640 [文件或目录]

### 其他权限管理命令
    1. chown
        1. 功能: 修改所有者
        2. 例子:
            1. chown hlg test 将当前目录下的test文件的所有者改为hlg
            2. 