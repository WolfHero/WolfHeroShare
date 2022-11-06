### 视频标题
摆脱沉重笔电！X分钟将你的平板电脑打造成编程开发利器！

### 视频结构
1. 实际体验展示
2. 安装WSL（或者Linux云服务器）
3. 安装code-server
4. 安装花生壳
5. 配置开发环境、安装VSCode插件
6. 更多玩法
7. 已知问题、解决方案
8. 结尾

### 教程

>因为本文面向的群体可能是Linux新手甚至第一次接触Linux，某些方面可能讲的比较啰嗦，如果你是新手，我希望看完这篇比较长的教程后你可以学会通过自己尝试来解决问题。如果你已经较为了解Linux，也请指出你认为的不足以及理由，作者深表感谢。  
>——WolfHero

#### 安装WSL

> 如果你已经拥有完成网络配置的Linux服务器或者虚拟机，可以直接跳转至[[#安装code-server]]。
> 要安装使用WSL，你需要Windows 10及以上的操作系统。

首先我们需要安装Linux环境，这里我比较推荐WSL (Windows Subsystem for Linux，适用于Linux的 Windows子系统)，因为对于初学者来说，WSL的安装以及网络配置更加简单。以下为WSL的安装教程：

首先按下Windows键+Q键打开Windows搜索，输入`启用或关闭Windows功能` ，在打开的窗口中将 `适用于Linux的Windows子系统` 和 `虚拟机平台`勾选，然后点击确定，安装启用功能后需要重启一次计算机。

重启后使用管理员权限打开命令提示符或者Powershell，输入`wsl --set-default-version 2`，然后再输入`wsl --install -d Ubuntu` 以安装Ubuntu Linux。*（你也可以安装其他你喜爱的Linux发行版）*

WSL安装需要从互联网下载文件，下载安装完毕以后可能需要再次重启电脑。

**本篇中很多组件的下载都对网络环境有一定要求，如果网络连接不畅请尝试：连接手机热点 / 开启代理。**

安装完成后WSL会自动启动，然后要求输入普通用户的用户名（小写英文）和密码。

>**面向萌新的科普：**  
>其中密码的输入是用户不可见的，看上去没有变化，实际上已经输入了。

输入完成后WSL基本安装完成，我们还需要完成一些善后工作。

首先在WSL窗口中输入`sudo passwd root`修改root用户的密码。这里需要先验证当前普通用户密码，然后才能设置root用户的密码。

设置完root用户的密码后输入`su`切换到root用户，此时需要输入root用户的密码。

**注意：以下的操作都是在root用户下进行！**
如果中途WSL窗口被关闭，可以在Win+R运行中输入`wsl`重新启动WSL窗口，然后输入`su`重新切换到root用户。

>**面向萌新的科普：**  
>root账号相当于Linux的管理员账号，拥有Linux系统的最高权限。sudo命令是暂借管理员账号权限执行sudo后边的命令。

然后在WSL窗口中执行`apt update`来对包管理工具索引进行更新。如果因为网络问题更新索引失败的话，可以访问[中国科学技术大学开源软件镜像站](https://mirrors.ustc.edu.cn/help/ubuntu.html)或[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)更改下载源服务器。
#### 安装code-server
索引成功更新后，在WSL窗口中执行`apt install screen`安装screen。 screen是Linux上的一个终端窗口管理器，可以使程序在后台运行，点击[Linux screen命令详解](https://www.cnblogs.com/cpxlll/p/15800214.html)了解更多有关screen的信息。

在WSL窗口中输入`curl -fsSL https://code-server.dev/install.sh | sh`来下载并安装code-server。同样因为网络问题，你有可能下载失败，你可以选择更换网络（比如手机热点）或者从[code-server GitHub发行版下载地址](https://github.com/coder/code-server/releases) 下载。
>**面向萌新的科普：**  
>下载时需要注意文件的名字，比如[code-server_4.8.2_amd64.deb](https://github.com/coder/code-server/releases/download/v4.8.2/code-server_4.8.2_amd64.deb) 是适用于X86-64位 *（萌新可以简单理解为桌面端64位）* Debian系（包括Ubuntu）的code-server 4.8.2版本的安装包。

安装包下载完成后将其手动移动到C盘或者D盘根目录，这一步是为了下一步更好在WSL找到安装包文件。

回到WSL，如果你上一步将安装包文件移动到了C盘根目录，就在WSL中执行`cd /mnt/c` 。
>**面向WSL新手的科普：**  
>WSL内访问电脑的C盘、D盘均通过/mnt目录   
>**面向萌新的科普：**  
>Linux中切换目录的命令是cd（change directory），`cd /mnt/c`即为切换目录到/mnt/c下  
>pwd命令可以使Linux返回当前目录  
>ll命令（大多数时候）可以使Linux列出当前目录下的所有文件  

在WSL中输入`dpkg -i code-server`，然后按Tab键自动补全文件名，再按下回车执行以安装code-server。
>**面向萌新的科普：**  
>在Linux终端中，大部分时候你可以只输入文件、目录或命令的开头一部分，然后通过按下Tab键对其进行补全。  
>dpkg是Debian系Linux中用来安装deb格式安装包的工具。[了解更多](https://baike.baidu.com/item/dpkg/9944168)   

安装完成后，在WSL内输入`code-server`即可运行code-server服务端。

WSL如果显示类似如下信息，则说明code-server已经启动成功。
```
[2022-11-04T20:10:56.669Z] info  code-server 4.8.2 ef82b1a565709cc91a53dc7b609aeee435404c0e
[2022-11-04T20:10:56.669Z] info  Using user-data-dir ~/.local/share/code-server
[2022-11-04T20:10:56.681Z] info  Using config file ~/.config/code-server/config.yaml
[2022-11-04T20:10:56.681Z] info  HTTP server listening on http://127.0.0.1:8080/
[2022-11-04T20:10:56.681Z] info    - Authentication is enabled
[2022-11-04T20:10:56.681Z] info      - Using password from ~/.config/code-server/config.yaml
[2022-11-04T20:10:56.681Z] info    - Not serving HTTPS
```
如果显示类似如下信息，则说明端口被占用。
```
[2022-11-04T20:12:49.437Z] info  code-server 4.8.2 cc8ce3b3c642b162daf39ad1d94ab778282463ef
[2022-11-04T20:12:49.438Z] info  Using user-data-dir ~/.local/share/code-server
[2022-11-04T20:12:49.442Z] error listen EADDRINUSE: address already in use 127.0.0.1:8080
```
按下Ctrl+C退出code-server，在WSL中执行`vim ~/.config/code-server/config.yaml`以编辑code-server配置文件。
```yaml
bind-addr: 127.0.0.1:8080
auth: password
password: 12345678
cert: false
```
第三行password字段默认会生成一串随机的字符串，可以将其修改为自己想要的密码。  
**⚠警告：请不要在这里使用弱密码！！！**  
**⚠警告：请不要在这里使用弱密码！！！**  
**⚠警告：请不要在这里使用弱密码！！！**  
**攻击者可通过code-server的内置终端访问到你的电脑中的任何文件！**  

>如果你对code-server的安全性有更高要求，可以访问[外部访问 - code-server官方文档](https://coder.com/docs/code-server/latest/guide#external-authentication)页面来设置更加可靠有效的登陆验证。

另外如果默认的8080端口被占用了，可以在上述文件的第一行将8080改为8081，但你需要记住端口的数值，之后设置外网访问时会用到。 
>**面向萌新的科普：**  
>1. vim是Linux终端的一个文本编辑器，使用`vim 文件名或文件路径`打开文件（如果文件不存在则创建文件）。  
>2. vim打开时的状态是NORMAL，此时无法编辑但可以移动光标。  
>3. 按下A键可以进入编辑状态，此时终端左下角显示INSERT字样，表示正在编辑状态，可以输入字符。在编辑状态下按下Esc键可以返回NORMAL状态。  
>4. 在NORMAL状态下依次按下`:wq`（冒号是英文冒号）可以保存文件并退出，如果不保存直接退出则输入`:q!`（英文感叹号表示强制执行），输入`:wq!`表示强制保存并退出。  
>5. 在vim内按下Ctrl+V可以粘贴从Windows窗口中复制的文本，请注意检查粘贴是否完整。  

修改完成后再次执行`code-server` ，然后在浏览器内打开`127.0.0.1:8080`地址（如果你修改了端口号，这里的8080应该改为修改后的端口号）。正常情况下，你将看到一个密码输入框，输入你在上一步设置的密码，登陆后就可以看到code-server主界面了，一个与VSCode非常相似的界面。

到目前为止，你的code-server已经可以访问了，但我们仍然需要做一些善后工作。
#### 设置开机启动WSL以及code-server

默认WSL不会被开机自动启动，code-server服务端也不能在关闭WSL窗口后保持后台运行，所以我们要进行如下操作。

在WSL窗口中输入`vim /etc/init.wsl`创建一个能让code-server服务端在后台运行的脚本，在vim编辑窗口中粘贴以下代码。
```shell
# /etc/init.wsl
screen_name="Code_Server" # 要建立的screen名字
screen -dmS $screen_name
cmd="/usr/bin/code-server" # 要执行的命令，要指明路径，不指明时默认是在 / 目录下
screen -x -S $screen_name -p 0 -X stuff '/usr/bin/code-server\n' # 进行执行
```
保存退出后执行`chmod +x /etc/init.wsl`，这一步是为了让init.wsl可以被执行。 [Linux chmod 命令 | 菜鸟教程](https://www.baidu.com/link?url=9EsEvJMJQEoWxN7BbgxsClJraG32V9Otepm_qoSd39rbx05ZsmzYSt7wUyuTXZ5XMEhFWmYm7b11x_JGXzCDlK&wd=&eqid=82e88e9200182b950000000563660406)在这里你可以了解到更多有关Linux文件权限的信息。

本教程中的code-server是通过root用户运行的，为了让Windows开机时候能够自动以root用户启动code-server，我们需要更改sudo的权限设置，使执行sudo时不需要输入普通用户的密码。
*（实际生产服务器中此操作慎用，容易使服务器失去安全性）*

在WSL窗口中输入`vim /etc/sudoers`编辑sudo命令的权限设置，找到文件末尾，换行后加入 `username ALL=(ALL) NOPASSWD:ALL`，username需要替换成你的WSL普通用户的用户名。

编辑完成后保存退出，因为/etc/sudoers是只读文件，只能使用`:wq!`进行强制保存并退出。

然后回到Windows，按下Win+R键运行`shell:startup`打开开机启动文件夹，右键创建一个文本文件，命名为Ubuntu.vbs的开机自启动脚本，输入下述代码后保存关闭。

```VBScript
Set ws = WScript.CreateObject("WScript.Shell")
ws.run "ubuntu run sudo /etc/init.wsl start", vbhide
```
上述代码原理为调用命令提示符运行`ubuntu run sudo /etc/init.wsl start`，即在WSL内以root用户运行/etc/init.wsl脚本启动code-server。

此时你的计算机会在开机时自动启动WSL和code-server，然后我们还需要创建一个脚本用来快速重启WSL以及运行在其之上的code-server。

返回桌面创建文本文件，命名为RestartWSL.bat，输入下述代码，即可双击运行重启WSL。
```Batch
@echo off
echo =======Closing WSL...======
wsl -t Ubuntu
echo ======Restarting WSL...======
ubuntu run sudo /etc/init.wsl start
```
上述代码中`wsl -t Ubuntu`部分用于终止WSL。

此时你可以双击RestartWSL.bat重启WSL，然后在浏览器中查看code-server是否正常运行，如果不能顺利看到code-server界面的话，请在命令提示符中运行`wsl`然后输入`su`切换到root用户重新开始这一阶段的操作。指路：[[#设置开机启动WSL以及code-server]]
#### 花生壳安装
> 如果你的code-server安装在公网环境内，则可跳过这一步，相应的是，你需要开放防火墙端口以及申请证书开启HTTPS，请参考官方文档[使用Let's Encrypt与Web服务端](https://coder.com/docs/code-server/latest/guide#using-lets-encrypt-with-caddy)。  
> *（外网设备在非HTTPS下访问code-server可能会使部分功能无法正常运转）*  

花生壳是一个可以将本地内网端口映射到公网的服务，通过此服务，公网设备可以简便地连接处于内网的code-server。点击[花生壳官网](https://hsk.oray.com/)下载花生壳客户端。

花生壳免费版可以映射一个端口，转发到HTTPS的443端口需要实名认证以及6元认证费用，不过申请[花生壳学生认证](https://hsk.oray.com/cooperation/) 可以直接免费获得HTTPS映射功能，可映射端口数也扩大到2个，以下是一个简单的对比：

| 版本   | 带宽  | 流量   | 映射数 | 额外 |
| ------ | ----- | ------ | ------ | ---- |
| 免费版 | 1Mbps | 1GB/月 | 1      |      |
| 学生版 | 1Mbps | 5GB/月 | 2      | 免费HTTPS映射     |

花生壳也有付费服务，但经过我的亲身体验，免费版的带宽已经可以满足日常使用了。

下载安装并注册好花生壳后，点击花生壳主界面的内网穿透→新增映射，在弹出的浏览器页面内的映射类型选择HTTPS（如果你是免费版账户，这里需要支付认证费用，学生版不需要），然后在右侧的外网域名中选择系统分配的壳域名，这个同时也是之后公网访问code-server的网站地址。

下方内网主机填入127.0.0.1，内网端口填入code-server默认端口8080，如果你在配置code-server时修改了端口号，请在内网端口内填入修改后的端口号。

>**面向萌新的科普：**  
>127.0.0.1是回环地址，当前设备可以通过其访问本机上的网络资源。

填好后点击确定保存映射，然后页面会回到花生壳在线管理网站，点击右下角的访问地址即可从公网访问code-server，并且此地址即是你的code-server的网站，你可以使用iPad或者其他任何可运行浏览器的设备打开它。

>**⚠警告：**  
>尽量不要公开你的code-server地址，这会使你的计算机处于被攻击的风险下！

#### 配置开发环境、安装插件

##### 安装插件
刚安装好的code-server是不包含任何插件的，既没有中文语言包，也没有运行/调试功能，这些都需要通过安装插件来进行（就如同Windows上的VSCode一样）。

code-server是开源项目，code-server会从[Open VSX Registry](https://open-vsx.org/) 上获取插件，所以你在插件商店搜索到的插件目录可能和VSCode不完全相同。

同样，因为网络问题，有可能你安装插件时会卡在Installing界面，此时切换网络环境，比如连接手机热点，或者跳转至[[#WSL内连接Clash代理]]。

另外还需要注意的是，如果你要通过安装Python插件来编写Python程序，你需要保证你的WSL内有安装Python，C或Java也同理。

##### 通过code-server打开终端

点击code-server左上角的菜单键，然后点击终端→新建终端打开WSL终端，如果你跟随本教程使用root用户启动code-server，此时终端的用户即为root。

>**面向萌新的科普：**  
>WSL（Linux）的终端与Windows的命令提示符类似，都是通过输入特定的字符指令使计算机能够实现一些功能。

输入`apt list --installed`可以列出当前WSL安装好的Linux程序，你可以通过翻阅列表来确定Python或JDK是否有安装以及安装的版本。

>**面向萌新的科普：**  
>1. WSL（Linux）内的JDK或C语言环境配置与Windows有较大差异，比如JDK在`apt list --installed`中通常显示为包含openjdk的字段，而C/C++语言则是gcc/g++。更加具体的信息可以通过百度OpenJDK和GCC进行了解。
>2. 举例：安装jdk17：`apt install openjdk-17-jdk` ，配置其他语言环境同样建议通过百度搜索“Linux 配置某某环境”学习。

互联网上有关VSCode配置环境的文章还是比较多的，请多多搜索学习，如果他们无法解决你的问题，也请在Issues中留言，我会进行解答。

#### 更多玩法

因为VSCode本身有丰富的插件，而WSL可以安装绝大部分的Linux程序，甚至连接计算机的物理显卡进行CUDA加速，二者结合之下，使用code-server能做到的事情便非常多。这里简单介绍几种：

##### Conda+VSCode Jupyter插件+CUDA实现GPU加速的深度学习

[Anaconda官方下载地址](https://www.anaconda.com/products/distribution#Downloads) 和[miniconda官方下载地址](https://docs.conda.io/en/latest/miniconda.html#linux-installers) ，国内如果下载缓慢也可以访问[anaconda | 清华大学开源镜像站](https://mirror.tuna.tsinghua.edu.cn/help/anaconda/) 下载。

VSCode Jupyter插件会在安装Python插件时自动安装，只要编辑*.ipynb文件就会自动开启Jupyter编辑器。

首先你需要将Windows内的Nvidia显卡驱动更新至最新，然后CUDA官方下载地址可以访问[这里](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=runfile_local) 。这里有来自Nvidia官方的详细[安装教程（英文）](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)。

安装好以后在终端内输入`nvcc -V`即可验证CUDA是否安装成功。

Pytorch和Tensorflow的下载安装均需选择Linux版本，这里是他们的官方网站：[Pytorch](https://pytorch.org/get-started/locally/) 和[Tensorflow](https://www.tensorflow.org/install/gpu) 。MXNet也是支持的，但我并不了解。

以Pytorch为例，安装完成之后在code-server内新建一个.ipynb文件，先在右上角选择安装有Pytorch的内核，然后创建代码单元格，输入：

```python
import torch  
torch.cuda.is_available()
```

如果返回True，则说明安装成功，享受GPU加速下的高速计算吧。

##### 访问SQL/NoSQL数据库

VSCode中的MySQL插件 *（开发者：cweijan）* 可以连接MySQL、SQLite、Redis在内的多种数据库，使用此插件可以完成大部分数据库客户端的工作。

经过作者体验，WSL安装MySQL与普通Linux不完全一致，主要原因是WSL内不能使用形如`systemctl` 、`service`这类快捷的服务工具。

我认为较为简单的安装方式是从[MySQL官网](https://dev.mysql.com/downloads/mysql/)或者镜像站下载tar.gz格式安装包，然后手动解压到/usr/local目录，然后在init.wsl中配置Windows开机启动WSL时顺带启动一个包含MySQL服务端的screen，我的init.wsl如下：

```shell
# code-server
screen_name="Code_Server" # 要建立的screen名字
screen -dmS $screen_name
screen -x -S $screen_name -p 0 -X stuff '/usr/bin/code-server\n' # 进行执行

#MySQL Deamon
screen_name1="mysqld" # 要建立的screen名字
screen -dmS $screen_name1
screen -x -S $screen_name1 -p 0 -X stuff '/usr/local/mysql/bin/mysqld --user=mysql\n' # 进行执行
```

当然在设置开机启动前你需要先创建mysql用户并手动启动mysqld完成首次运行配置。这些在互联网上有比较丰富的教程，再此不多赘述。

#### 已知问题与解决方案

code-server目前还是存在一些问题的，这里有我遇到的一些问题和我解决的方法，你也可以到[code-server GitHub Issues页面](https://github.com/coder/code-server/issues) 查看或提出你遇到的问题。

>**面向萌新的科普：**  
>GitHub的Issues页面可以看成是针对本开源项目创建的公共问答讨论平台，所有人可以在此提出疑问，而开发者或其他用户则可以帮助你解决问题。  

##### WSL内连接Clash代理

虽然WSL和Windows共用端口，你可以通过127.0.0.1或localhost访问到WSL上搭建的浏览器应用，但事实上WSL还是通过虚拟网卡从Windows共享网络的，所以你在Windows中启动的Clash等代理软件是无法实现WSL加速的，以下的方法可以解决这个问题（以Clash For Windows举例）：

编辑~/.bashrc，在底部输入以下代码：

```shell
# Clash For Windows
export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
export https_proxy="http://${hostip}:7890";
export http_proxy="http://${hostip}:7890";
```

其中的7890端口号是Clash For Windows的默认端口，如果你修改过的话，这里也应该统一修改。

保存退出后建议重新启动WSL，这样从WSL到code-server均可连接到代理。

##### code-server找不到某些插件

code-server找不到插件可能是因为该插件只在[微软VSCode插件商店](https://marketplace.visualstudio.com/)发布，你可以前往搜索插件，然后在插件介绍页右侧点击Download Extension下载vsix格式的插件离线安装包。

然后打开code-server，在插件页面右上角菜单处选择从VSIX安装，然后选择要安装的vsix文件。

大部分插件可以通过这种方式安装，但是类似语言包的插件则无法生效。

#### 结尾

如果你有疑问或者建议意见，可以在Bilibili视频下评论私信或者在GitHub项目中提出Issues，我会及时回答。

