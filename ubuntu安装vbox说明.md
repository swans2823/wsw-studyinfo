#### ubuntu安装vbox说明

##### 安装vbox

```
１．参考链接：https://www.virtualbox.org/wiki/Linux_Downloads
步骤：1.1更新源：Sudo vim /etc/apt/sources.list
　　　　　添加内容：deb https://download.virtualbox.org/virtualbox/debian xenial contrib
　　　1.2添加key:wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc
                -O- | sudo apt-key add -
                wget -q https://www.virtualbox.org/download/oracle_vbox.asc 
                -O- | sudo apt-key add -
      1.3安装：sudo apt-get update ｜　sudo apt-get install virtualbox-5.2
```

##### 安装VBox extension

```
１．参考链接：https://blog.csdn.net/yujiaao/article/details/72380277
步骤：2.1下载 Vbox Extension(官网地址：https://www.virtualbox.org/wiki/Downloads)
　　　　　　　　　　　　　　　寻找与VBox版本相同的extension｜下载 VirtualBox 5.2.18 Oracle VM 　　　　　　　　　　　　　　　VirtualBox Extension Pack　｜　ALL supported platforms
　　　2.2使用root用户安装扩展包:sudo su  
     VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-5.2.18.vbox-extpack --replace
```

##### 启动服务器上的virtualBox Web服务

```
3.1创建vbox用户 sudo useradd -g vboxusers vbox
3.2在/etc/default/virtualbox处，为Web服务创建一个配置文件。
   sudo vi /etc/default/virtualbox VBOXWEB_USER="vbox"
        VBOXWEB_USER=vbox
        VBOXWEB_TIMEOUT=0
        VBOXWEB_LOGFILE="/var/log/vboxwebservice.log"
        VBOXWEB_HOST="10.0.0.121"
3.3创建日志文件及该用户:
	sudo touch /var/log/vboxwebservice.log
	sudo chown vbox:vboxusers /var/log/vboxwebservice.log 
3.5开启VirtualBox Web服务：
	sudo service vboxweb-service start 
3.6核查VirtualBox Web服务的状态：
	 sudo service vboxweb-service status    
```

##### 客户端安装RemoteBox,这个软件是绿色版的，只支持Linux，下载解压运行就可以了

```
4.1下载tar包:wget http://knobgoblin.org.uk/downloads/RemoteBox-2.4.tar.gz
4.2解压tar包，进入解压后的路径
	tar zxvf RemoteBox-2.4.tar.gz
	cd RemoteBox-2.4/
4.3运行：./RemoteBox-2.4
4.4使用display需要安装freefrdp
	sudo apt install freerdp-x11
```

##### 