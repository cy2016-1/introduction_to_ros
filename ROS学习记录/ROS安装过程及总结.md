# ROS安装过程中遇到的问题总结
## 1.Ubuntu 16.04
`sudo apt-get update` 使用更新语句遇到的一些问题与解决方法

+ 问题描述：`Problem executing scripts APT::Update::Post-Invoke-Success 'if /usr/bin/test -w /var/cache/app-info -a -e /usr/bin/appstreamcli; then appstreamcli refresh > /dev/null; fi'`  
这一步是在安装问题中`sudo apt-get update`中遇到的

+ 解决办法：
`sudo pkill -KILL appstreamcli`  
这一步主要是杀死进程

## 2.设置密钥时出现如下错误
+ 显示错误  
        ```
        sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
        ```
        gdp:冲突的指令
+ 解决办法：   
如果在连接密钥服务器时遇到了问题，可以尝试在上面的命令中用`hkp://pgp.mit.edu:80 或 hkp://keyserver.ubuntu.com:80`来替换。  
这一步主要是因为由于网造成的。

## 3.安装ros-kinetic时出现错误
+ 问题描述  
  `sudo apt-get install ros-kinetic-desktop-full`  
  执行上述命令时会出现如下错误
  ```
  E: 文件 list 第 1 行的记录格式有误 /etc/apt/sources.list.d/ros-latest.list (URI parse)
  E: 无法读取源列表。
  ```
+ 解决办法
执行下述命令即可解决   
`sudo rm /etc/apt/sources.list.d/ros-latest.list`

## 4.找不到bash目录   
+ 错误： `bash: /opt/ros/kinetic/setup.bash: `没有那个文件或目录   
+ 出现这一步主要是因为bashrc里面有重复多余的配置，可执行下述命令   
 `gedit ~/.bashrc `   
+ 在打开文件的最后找到`bash: /opt/ros/kinetic/setup.bash `
+ 删除重复的多余配置,如果没有多余项查看这句话拼写是否正确，或者与你的ros版本相称   
+ 如果这一步还是不行，那么你应该是ros-kinetic没安装好，你应当重新安装sudo apt-get install ros-kinetic-desktop-full 这条命令。   
## 5.Ubuntu16.04安装ROS 过程中中显示无法定位软件解决办法
解决办法
   
```deb http://packages.ros.org/ros/ubuntu xenial maindeb http://packages.ros.org/ros-shadow-fixed/ubuntu xenial main  ```   
## 6.实现Linux与Windows之间的复制   
### 只需要在ubuntu安装一个vmware-tools的工具。具体实现如下： 
+ 打开虚拟机的菜单中文为“虚拟机”，英文是“VM”，下拉框中会有一个Install vmware tools 工具的安装选项。 
+ 点击之后，在CD-ROM中生成在ubuntu的桌面下会出现 VMwareTools-9.2.3-1031360.tar.gz 的文件（你的版本号跟我不同，内容一样），当前为路径（/media/VMware Tools）。 
+  将此文件复制到随便文件下进行解压，本人如下操作：
                cp  VMwareTools-9.2.3-1031360.tar.gz /tmp
                cd  /tmp
                tar -xzvf VMwareTools-9.2.3-1031360.tar.gz  
+ 这是会出现解压后的目录( vmware-tools-distrib目录）。然后执行编译操作。然后打开命令窗到指定安装包路径，执行：
                   sudo  root
                   sudo ./vmware-install.pl
        最好根权限运行，以防无法编译。 
### 开始进行安装，遇到什么提醒默认设置即可，一路回车。如果执行过程中出现“…致命错误：`Linux/smp_lock.h`没有那个文件或目录，编译中断….”的错误，不用理会只管一路回车即可。 
+ 安装完成提醒： 
　　　  `Enjoy,`
    
　　　`–the VMware team`

+ 重新启动ubuntu系统，大功告成。 