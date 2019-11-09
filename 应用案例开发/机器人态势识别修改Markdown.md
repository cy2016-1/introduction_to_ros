# 态势识别（机器人教育版）
## 本方案主要实现目的
### 本方案主要适用于Aelos edu（1代机器人）在线控制，Aelos Pro/Lite 、Aelos 1s 、 XYL_Arm 待测
## 1.步骤说明
### 1）安装说明
+   下载安装codelab-adapter插件，并运行
+   下载extension_aelosedu_online.py
+   python文件放入extension文件夹
+ 根据操作系统（windows/Mac/Linux)修改文件。这里最主要是对串口进行修改，具体修改如下

		def auto_detect_port():
    	device_desc = 'STMicroelectronics Virtual Port'
   		vid_pid = '0483:5740'
    	ports = serial.tools.list_ports.comports()
    	for port, desc, hwid in sorted(ports):
        print("port: {} desc: {} hwid: {}".format(port, desc, hwid))
        if device_desc in desc and vid_pid in hwid:
            return port
    	#assert False, 'Aelos usb device not found!'



### 2）操作说明
+ 打开codelab-adapter插件，并运行
+ 点击extension_aelosedu_online,，用USB数据线连接机器人，进行加载
+ 打开网址（https://scratch3beta.lejurobot.com:8601/），注意用谷歌加载
+ 编写程序

### 3）实现流程说明（主要以python为主）
    def auto_detect_port():
    device_desc = 'STMicroelectronics Virtual Port'
    vid_pid = '0483:5740'
    ports = serial.tools.list_ports.comports()
    for port, desc, hwid in sorted(ports):
        print("port: {} desc: {} hwid: {}".format(port, desc, hwid))
        if device_desc in desc and vid_pid in hwid:
            return port
    #assert False, 'Aelos usb device not found

&#8194;&#8194;&#8194;&#8194;实现功能：这一部分主要是侦听端口，即以实现机品人与计算机之间的通信，注意，
device_desc = 'STMicroelectronics Virtual Port'中的端口一定要与计算机中USB的端口一致。

 
    class LejuAelosEduOnlineExtension(Extension):
    def __init__(self):
        super().__init__()
        self.EXTENSION_ID = "eim/leju/aelosonline"
        self.q = queue.Queue(maxsize=1)
        self.wired = WiredUsb()
        self.wired.online_mode()

    def extension_message_handle(self, topic, payload):
        self.q.put(payload)

    def run(self):
        while self._running:
            time.sleep(0.01)
            if not self.q.empty():
                payload = self.q.get()
                python_code = payload["content"]
                try:
                    output = eval(python_code, {"__builtins__": None}, {
                        "wired": self.wired
                    })
                except Exception as e:
                    output = e
                    print(python_code)
&#8194;&#8194;&#8194;&#8194;实现功能：这一部分主要是 LejuAelosEduOnlineExtension与codelab-adapter插件之间的内嵌。

	def set_arms(self, l_shoulder, l_elbow, r_shoulder, r_elbow):
        pose = [80, 30, 100, 100, 93, 55, 124, 100,
                120, 170, 100, 100, 107, 145, 76, 100]
        pose[0] = l_elbow
        pose[1] = l_shoulder
        pose[8] = r_elbow
        pose[9] = r_shoulder
        self.set_angles(pose)
&#8194;&#8194;&#8194;&#8194;实现功能：这一部分主要是设置角度，如set_arms(90,90,90,90)
	


	def arm_pose(line):#line主要映射在Scratch里创建的列表（用数组表示）
    max_limit = 175
    min_limit = 15
    line = line.replace('aelos_armpose:', '') #用空格替代aelos_armpose
    coors = line.split(' ')#进行水平分割，不然数组列表里的字会连在一起
    coors = list(map(lambda x: float(x), coors))#进行映射
    x_ls, y_ls, x_le, y_le, x_lw, y_lw = coors[0], coors[1], coors[2], coors[3], coors[4], coors[5]#映射Scracth里数组列表里的x,y坐标值
    x_rs, y_rs, x_re, y_re, x_rw, y_rw = coors[6], coors[7], coors[8], coors[9], coors[10], coors[11]
    lse = math.sqrt((x_ls - x_le)**2 + (y_ls - y_le)**2)
    lew = math.sqrt((x_le - x_lw)**2 + (y_le - y_lw)**2)
    lsw = math.sqrt((x_ls - x_lw)**2 + (y_ls - y_lw)**2)
    rse = math.sqrt((x_rs - x_re)**2 + (y_rs - y_re)**2)
    rew = math.sqrt((x_re - x_rw)**2 + (y_re - y_rw)**2)
    rsw = math.sqrt((x_rs - x_rw)**2 + (y_rs - y_rw)**2)
    # left hand
    l_shoulder = math.asin(abs(x_le - x_ls) / lse) / math.pi * 180
    l_shoulder = l_shoulder if y_le < y_ls else 180 - l_shoulder
    l_elbow = math.acos((lse**2 + lew**2 - lsw**2) /
                        (2*lse*lew)) / math.pi * 180
    l_elbow = l_elbow - 90
    if y_lw > y_le:
        l_elbow = 180 - l_elbow
        l_elbow = l_elbow if l_elbow < max_limit else max_limit
    else:
        l_elbow = l_elbow if l_elbow > min_limit else min_limit
    # right hand
    r_shoulder = math.asin(abs(x_re - x_rs) / rse) / math.pi * 180
    r_shoulder = r_shoulder if y_re < y_rs else 180 - r_shoulder
    r_elbow = math.acos((rse**2 + rew**2 - rsw**2) /
                        (2*rse*rew)) / math.pi * 180
    r_elbow = r_elbow - 90
    if y_rw > y_re:
        r_elbow = 180 - r_elbow
        r_elbow = r_elbow if r_elbow < max_limit else max_limit
    else:
        r_elbow = r_elbow if r_elbow > min_limit else min_limit

    r_shoulder = 190 - r_shoulder
    r_elbow = 190 - r_elbow
    return int(l_shoulder), int(l_elbow), int(r_shoulder), int(r_elbow)

&#8194;&#8194;&#8194;&#8194;实现功能：这一部分主要是实现机器人态势模仿，注意，line一定要与Scratch里的数组列表相映射，数组列表在Scratch里创建，并且用水平分割符进行分割，以子目数组里面的字府相互拼接。


### 4）Scracth实现
+ 角色创建

    * 在造型栏用笔画出相应的六个角色，为left_shoulder,left_elbow,left_wrist,right_shouler,right_elbow,right_wrist,
    * 创建列表，捕捉创建的六个角色，十二个点
    * 拖入Posnet模块，Eim模块，Aelos模块及Maekly mark模块，捕捉点
    * 开启摄像头
    * 进行模块编程
    * 具体程序如下








![image](https://note.youdao.com/yws/api/personal/file/WEB7e4ba50d0ea4b7334f143920a51d3f3a?method=download&shareKey=05b58db8f89fb93d08a10c97dd45ba79)



# 2.总结
&#8194;&#8194;&#8194;&#8194;本方案主要实现机器人态势识别，其中有很多需要注意的细节，如配置插件接口时，要修改串口，与计算机里的串口一致，作态势识别时，要选择光源较好的点，尽量选择背景深的地方，提高
识别准确度，在编程时，要选择适当的间隔时间，让机器人动作有一个缓冲，要关注列表里点的大小，注意抖动程度，在做动作时，要选择合适的距离。此方案还有待改进，主要是点定位不准，抖动度较大，希望爱好者共同指正改进。