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
>因为本文面向的群体可能是Linux新手甚至第一次接触Linux，某些方面可能讲的比较啰嗦，如果你较为了解Linux，请多多海涵。
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
索引成功更新后，在WSL窗口中执行`apt install screen`安装screen。 点击[Linux screen命令详解](https://www.cnblogs.com/cpxlll/p/15800214.html)了解更多有关screen的信息。

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
第三行password字段默认会生成一串随机的字符串，可以将其修改为自己想要的密码。另外如果默认的8080端口被占用了，可以在第一行将8080改为8081，但你需要记住端口的数值，之后设置外网访问时会用到。 
>**面向萌新的科普：**
>1. vim是Linux终端的一个文本编辑器，使用`vim 文件名或文件路径`打开文件（如果文件不存在则创建文件）。
>2. vim打开时的状态是NORMAL，此时无法编辑但可以移动光标。
>3. 按下A键可以进入编辑状态，此时终端左下角显示INSERT字样，表示正在编辑状态，可以输入字符。在编辑状态下按下Esc键可以返回NORMAL状态。
>4. 在NORMAL状态下依次按下`:wq`（冒号是英文冒号）可以保存文件并退出，如果不保存直接退出则输入`:q!`（英文感叹号表示强制执行），输入`:wq!`表示强制保存并退出。
>5. 在vim内按下Ctrl+V可以粘贴从Windows窗口中复制的文本，请注意检查粘贴是否完整。

修改完成后再次执行`code-server` ，然后在浏览器内打开`127.0.0.1:8080`地址（如果你修改了端口号，这里的8080应该改为修改后的端口号）。正常情况下，你将看到一个密码输入框，输入你在上一步设置的密码，登陆后就可以看到code-server主界面了，一个与VSCode非常相似的界面。

创建init.wsl
```shell
# /etc/init.wsl
screen_name="Code_Server" # 要建立的screen名字
screen -dmS $screen_name
cmd="/usr/bin/code-server" # 要执行的命令，要指明路径，不指明时默认是在 / 目录下
screen -x -S $screen_name -p 0 -X stuff '/usr/bin/code-server\n' # 进行执行
```
chmod +x init.wsl
vi /etc/sudoers 末尾加入 用户名 ALL=(ALL) NOPASSWD:ALL
创建开机自启动脚本
```VBScript
REM 按下Win+R键运行shell:startup打开开机启动文件夹
REM 保存在启动文件夹内即可实现开机自启动
Set ws = WScript.CreateObject("WScript.Shell")
ws.run "ubuntu run sudo /etc/init.wsl start", vbhide
```
创建重启脚本，保存为RestartWSL.bat，双击运行即可重启WSL
```Batch
@echo off
echo =======Closing WSL...======
wsl --t Ubuntu
echo ======Restarting WSL...======
ubuntu run sudo /etc/init.wsl start
```
#### 花生壳安装
> 如果你的code-server安装在公网环境内，则可跳过这一步

#### 已知问题与解决方案
##### WSL内无法连接GitHub

##### code-server无法安装插件
