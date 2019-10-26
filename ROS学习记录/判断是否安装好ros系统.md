判断ros系统是否安装成功（以小海龟为例）
第一步：启动Ros Master 
    $roscore
第二步：启动小海龟仿真器
    $rosrun turtlesim turtlesim_node
第三步：启动海龟节点
    $ rosrun turtlesim turtle_teleop_key

如何创建工作空间和功能包？
创建工作空间以及编译：
把工作空间放在主文件夹中，mkdir catkin_ws（名字随便定义）
，再cd catkin_ws/，再mkdir src，再cd src/,再catkin_init_workspace(会产生
属性的变化，确定之后会产生一个CMakeList.txt文件，说明是一个工作空间了)，
回到根目录（catkin_ws）进行编译，catkin_make(编译之后会产生devel,build,没有install文件)，
产生install文件：catkin_make install

创建功能包（创建代码前得创建功能包，功能包是放置源码的最小单元）：
 功能包创建一定要在src里面，先cd src/,再catkin_create_pkg test_pkg roscpp rospy std_msgs(test_pkg（是功能包）中的src是用来放置你的代码文件的，include是
用来放置你的头文件的，功能包中必须要存在CMakeList.txt文件和package.xml文件)，编译功能包：回到catkin_ws/：先cd ..,再catkin_make
若要编译某一个程序，要去设置一下环境变量，source devel/setup.bash(让系统找到这个功能包)

发布者publisher的编程实现
创建新的工作包在src中，catkin_create_pkg learning_topic roscpp rospy std_msgs geometry_msgs turtlesim,接下来实现publisher这个代码，
怎么创建publisher：在learning_topic中的src把代码文件复制进去（主要是c++）
编译：首先得修改CMakeList.txt文件（里面是用来设置编译规则的）：
把add_executable(velocity_publisher src/velocity_publisher.cpp)
target_link_libraries(velocity_publisher ${catkin_LIBRARIES})复制到install上面位置。
然后到cd catkin_ws/中，再catkin_make，记得要设置环境变量source devel/setup.bash

运行：首先运行roscore,再运行海龟仿真器：rosrun turtlesim turtlesim_node，
最后运行自己功能包中的程序（按一定的速度进行运动）：rosrun learning_topic velocity_publisher
执行python文件时（注意确定是否为可执行文件）

订阅者Subscriber的编程实现（订阅海龟的位置信息）
在learning_topic中的src把代码文件复制进去
编译：首先得修改CMakeList.txt文件（里面是用来设置编译规则的）：
添加：add_executable(pose_subscriber src/pose_subscriber.cpp)
target_link_libraries(pose_subscriber ${catkin_LIBRARIES})
然后到cd catkin_ws/中，再catkin_make

运行：首先运行roscore,再运行海龟仿真器：rosrun turtlesim turtlesim_node，
最后运行自己功能包中的程序（按一定的速度进行运动）：rosrun learning_topic pose_subscriber
执行python文件时（注意确定是否为可执行文件）

话题消息的定义与使用
如何自定义话题消息（person.msg）:
string name
uint8 sex
uint8 age

uint8 unknown=0
uint8 male=1
uint8 female=2
步骤：
1.创建person.msg文件（在功能包learning_topic创建文件夹msg，在终端中创建person.msg文件 touch person.msg）
2.在package.xml中添加功能包依赖（放进package.xml中）
<build_depend>message_generation</build_depend>：编译依赖
<exec_depend>message_runtime</exec_depend>：运行依赖
3.在CMakeList.txt添加编译选项
.find_package(.......message_generation)
.add_message_files(FILES person.msg)
generate_messages(DEPENDENCIES std_msgs)
.catkin_package(......message_runtime)
3.编译生成语言相关文件

在之前创的功能包learning_topic创建文件夹msg，在里面创建person.msg文件 touch person.msg（把消息内容复制进去）
规则：1.点开package.xml文件，把
<build_depend>message_generation</build_depend>：
<exec_depend>message_runtime</exec_depend>：复制到下面
2.在CMakeList.txt添加编译选项（3个地方）
.find_package(.......message_generation)
.add_message_files(FILES person.msg)
generate_messages(DEPENDENCIES std_msgs)
.catkin_package(......message_runtime)
编译：回到工作空间的根目录cd catkin_ws/中，再catkin_make

把两个代码文件（publisher和subscriber）复制到learning_topic中的src中，
编译：首先得修改CMakeList.txt文件
add_executable(person_publisher src/person_publisher.cpp)
target_link_libraries(person_publisher ${catkin_LIBRARIES})
add_dependencies(person_publisher ${PROJECT_NAME}_generate_messages_cpp)

add_executable(person_subscriber src/person_subscriber.cpp)
target_link_libraries(person_subscriber ${catkin_LIBRARIES})
add_dependencies(person_subscriber ${PROJECT_NAME}_generate_messages_cpp)

然后到cd catkin_ws/中，再catkin_make

运行这两个程序：在CMakeList.txt添加编译选项
add_
首先运行roscore（ctrl+c:关闭）
再运行订阅者功能包中的程序：rosrun learning_topic person_subscriber
最后运行发布功能包中的程序：rosrun learning_topic person_publisherr


客户端Client的编程实现
首先创建功能包
在src中，catkin_create_pkg learning_service roscpp rospy std_msgs geometry_msgs turtlesim
在learning_service中的src把代码文件复制进去

编译：
首先满足编译规则：
add_executable(turtle_spawn src/turtle_spawn.cpp)
target_link_libraries(turtle_spawn ${catkin_LIBRARIES})
再进行编译catkin_make
运行：首先运行roscore,再运行海龟仿真器：rosrun turtlesim turtlesim_node，
最后运行自己功能包中的程序：rosrun learning_service turtle_spawn

服务端Server的编程实现
在learning_service中的src把代码文件复制进去
编译：
首先满足编译规则：
add_executable(turtle_command_server src/turtle_command_server.cpp)
target_link_libraries(turtle_command_server ${catkin_LIBRARIES})
再进行编译catkin_make（编译有问题）

运行：首先运行roscore,再运行海龟仿真器：rosrun turtlesim turtlesim_node，
最后运行自己功能包中的程序：rosrun learning_service turtle_command_server（开始等待）
再一个终端rosservice call/turtle_command(双击tab键自动补全)

服务数据的定义与使用
如何自定义服务数据？(Person.srv)
tring name
uint8 sex
uint8 age

uint8 unknown=0
uint8 male=1
uint8 female=2

---

string result(三横线上request,下response)

首先在learning_service创建中文件夹srv,再在该文件夹中创建Person.srv文件，把上面的内容复制进去

编译：
规则：1.点开package.xml文件，添加功能包依赖，把
<build_depend>message_generation</build_depend>
<exec_depend>message_runtime</exec_depend>复制到下面
2.在CMakeList.txt添加编译选项（3个地方）
.find_package(.......message_generation)
.add_message_files(FILES Person.srv)
generate_messages(DEPENDENCIES std_msgs)
.catkin_package(......message_runtime)
开始编译：回到工作空间的根目录cd catkin_ws/中，再catkin_make

把两个代码文件（person_client和person_server）复制到learning_service中的srv中，
编译：首先得修改CMakeList.txt文件
add_executable(person_server src/person_server.cpp)
target_link_libraries(person_server ${catkin_LIBRARIES})
add_dependencies(person_server ${PROJECT_NAME}_generate_messages_cpp)

add_executable(person_client src/person_client.cpp)
target_link_libraries(person_client ${catkin_LIBRARIES})
add_dependencies(person_client ${PROJECT_NAME}_generate_messages_cpp)

然后到工作空间cd catkin_ws/中，再catkin_make

运行这两个程序
add_
首先运行roscore
再运行订阅者功能包中的程序：rosrun learning_service person_server
最后运行发布功能包中的程序：rosrun learning_service person_client


参数的使用与编程方法：
Parameter：参数服务器，一个全局字典，用来保存各个节点之间的配置参数
使用方法：首先创建功能包
在catkin_ws中的 src中创建功能包catkin_create_pkg learning_parameter roscpp rospy std_srvs 

rosparam:看参数
rosparam list:
得到某个参数的值：rosparam get /background_b(按tab键去补全它)
rosparam get /rosversion :得到ros的版本
rosparam set /background_b 100：把background_b的值改成100  （会改变背景的颜色，得先刷新，rosservice call /clear "{}"）

保存一个文件
rosparam dump file_name
rosparam dump param.yaml(已经保存在主文件夹中)

加载一个文件
rosparam load file_name
rosparam load param.yaml

删除参数
rosparam delete param_key
rosparam delete /background_b

在代码中怎么使用这些参数
把代码文件复制到learning_parameter中src中
开始编译：
修改规则
add_executable(parameter_config src/parameter_config.cpp)
target_link_libraries(parameter_config ${catkin_LIBRARIES})
编译：catkin_make（蓝色变成白色）

运行：
运行roscore
运行海龟仿真器：rosrun turtlesim turtlesim_node 
运行rosrun learning_parameter parameter_config
