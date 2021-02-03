**目的：方便SSR部署在Ubuntu等Linux系统上，加快虚拟机或者其他边缘端设备的上下行速度**



## 1. 下载好SSR（小火箭）

注：系统需有安装Python2.7才能运行，你可以先打开命令行输入：python -V 看看版本

网址： https://down.zhushou.site/electron-ssr-0.2.6.AppImage 
首先下载文件到Ubuntu系统中

下载客户端后双击打开，可以看到客户端界面，点 知道了 进入软件



## 2.  配置订阅链接

等下载好软件，在桌面上可以看到 “小飞机图标” 的客户端，右键它

右键后选择“ 服务器 - 订阅管理 ”进入订阅设置

进入订阅设置点击“添加”，出现输入框，粘贴复制好的订阅链接粘贴，点完成。返回后就可以开始更新订阅服务器，如果无法更新成功请右键客户端-系统代理模式选择为 关闭 然后再更新，选择一个节点启动

但此时还无法使用，还需要进行网络设置

如果无法使用请进入“系统代理”里检查是否有选择刚刚设置的 SOCKS5.LINUX界面客户端的PAC文件有问题，所以不能使用PAC模式，只能使用全局，在需要的时候启动，不需要的时候关闭，如果你有PAC文件和动手能力你可以自行解决。



## 3. 配置privoxy代理

安装privoxy
```bash
sudo apt-get install privoxy
```

编辑代理配置文件
```bash
sudo vim /etc/privoxy/config
```

命令行下找到内容：
```bash
/listen-address  localhost:8118
```

并注释掉这句话 listen-address  localhost:8118

在文件末尾添加：
forward-socks5t   /   127.0.0.1:1080 .
listen-address  127.0.0.1:8118

重启代理服务
```bash
sudo service privoxy restart
```

使用代理的时候，需要在终端里面先输入
```bash
export http_proxy='http://127.0.0.1:8118' 
export https_proxy=$http_proxy
```

注意：
每次打开一个终端，当需要使用代理的时候都需要先输入上面的两句话

要想下次打开终端自动载入，需要修改profile文件
```bash
vim /etc/profile
```

在里面加入上面的两个export命令，重新注销系统才能生效，最后可以通过echo命令测试一下：
```bash
echo $ http_proxy
```

测试，如果把返回200 ，并且把google的首页下载下来了，那就是成功了：
```bash
wget www.google.com
```

