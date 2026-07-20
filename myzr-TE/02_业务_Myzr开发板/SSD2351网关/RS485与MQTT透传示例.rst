RS485与MQTT透传示例
====================

一、目标
----------

|  将 RS485 电表（dds1079）的数据，通过网关转换为 MQTT 消息，上报到 MQTT 服务器，用户在电脑上的 MQTT 客户端就能看到电表数据。
|  涉及设备

  - 网关开发板（内置 RS485 与以太网接口，运行网关程序）
  - RS485 电表 dds1079（或等效设备）
  - 客户电脑（用于网页配置 + MQTT 客户端）
  - MQTT 服务器（可以是公网的 broker.emqx.io，也可以是客户自建）


二、设备连接
--------------

|  RS485连接

1. 准备一根 RS485 通讯线 （A/B 两芯，或差分线）。
2. 将 RS485 接线端子与网关的 RS485 接口对应连接：

  - 电表 A ↔ 网关 A2
  - 电表 B ↔ 网关 B2
  - 电表GND↔ 网关GND

3. 确认电表和网关都上电正常：

  - 电表显示屏/指示灯正常；
  - 网关电源指示灯、RUN 指示灯正常。

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例1.png
   :alt: RS485与MQTT透传示例1.png


三、网口连接
-------------

|  ETH1 -- WAN: DHCP
|  BR0 -- LAN: 192.168.9.1
|  BR0 (ETH2, WLAN0, WLAN1)

1. 先通过网线将主机和开发板连接，通过静态ip，192.168.137.81进入网址首页找到MAC地址并记住。

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例2.png
   :alt: RS485与MQTT透传示例2.png

 
|  这里的MAC地址是C6:72:27:3C:73:E1

2. 进入主机cmd命令端，输入命令arp -a | find /i "c6-72-27-3c-73-e1"来找到

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例3.png
   :alt: RS485与MQTT透传示例3.png


3. 在网站中输入命令端进入网页

三、网页配置
-------------

|  进入左侧配置编辑，当前界面可以对配置文件进行修改，Device 是设备，UART 是串口，MQTT 是服务器 ，三者通过 object/interface 串起来”。

设备列表配置
~~~~~~~~~~~~~

参数说明
""""""""""

- **interface (接口名称)**

  - 含义 ：该采集任务的唯一识别名。
  - 说明 ：建议设置为设备型号或安装位置（如 dds1079 ），方便在日志和数据平台中区分不同的设备。

- **status (启用状态)**
  
  - 含义 ：该任务的开关。
  - 说明 ：设置为 enabled 表示立即开始采集；设置为 disabled 表示暂时停止该任务。

- **command (采集指令)**
  
  - 含义 ：发送给物理设备的原始指令报文。
  - 说明 ：通常为十六进制格式（Hex）。例如，图中是一串读取电表数据的 Modbus 或 DL/T 645 协议指令。

- **period (采集周期)**
  
  - 含义 ：两次采集之间的时间间隔。
  - 说明 ：单位为 毫秒 。例如设置为 1000 表示每隔 1 秒钟向设备发送一次指令。

- **action (处理动作)**
  
  - 含义 ：获取到设备回传数据后的处理方式。
  - 说明 ：常用 forward （转发），表示将采集到的原始数据直接转发给服务器或云平台。

- **object (物理端口)**
  
  - 含义 ：指令通过哪个硬件接口发出去。
  - 说明 ：例如 uart2 代表开发板上的第 2 个串口（通常对应板子上的 RS485 接口）。

- **format (数据格式)**
  
  - 含义 ：通信时使用的数据编码形式。
  - 说明 ： hex 代表十六进制（工业设备最常用）；也可以根据需求配置为 string （字符串）或 json 。
  
**示例：**

|  本次示例按照图中配置

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例4.png
   :alt: RS485与MQTT透传示例4.png


串口列表配置
~~~~~~~~~~~~~

参数说明
""""""""""

- **interface (接口名称)**

  - 含义 ：该串口配置的内部识别名。
  - 说明 ：通常命名为 uart1 、 uart2 等，用于在其他配置中引用此串口。

- **status (启用状态)**

  - 含义 ：是否激活该串口。
  - 说明 ： enabled 为开启， disabled 为关闭。若不使用该串口，建议关闭以节省资源。

- **device (设备路径)**

  - 含义 ：硬件串口在 Linux 系统中的物理地址。
  - 说明 ：例如 /dev/ttyS1 。这直接对应开发板上的某个物理接线端子，通常无需用户修改。

- **baud_rate (波特率)**

  - 含义 ：串口通信的传输速率。
  - 说明 ：必须与连接的外部设备一致。常见值有 9600 、 115200 等。

- **data_bits / stop_bits / parity (数据位/停止位/校验位)**

  - 含义 ：串口通信的基础底层协议参数。
  - 说明 ：工业标准通常为 8 位数据位、 1 位停止位、 none （无）校验。需根据外部设备手册进行匹配。

- **flow_control (流控)**

  - 含义 ：数据传输的流量控制方式。
  - 说明 ：通常设置为 none 。除非设备明确要求硬件或软件流控，否则不建议开启。

- **udelay (微秒延迟)**

  - 含义 ：串口读写操作后的强制等待时间。
  - 说明 ：单位为 微秒 。用于适配一些响应较慢的老旧工业设备，通常设置为 0 。

- **action / object (处理动作/转发对象)**

  - 含义 ：串口收到数据后的去向。
  - 说明 ：例如 action 为 forward ， object 为 server1 ，表示串口收到的所有原始数据都会立即转发给配置好的服务器。

- **format (数据格式)**

  - 含义 ：数据的表现形式。
  - 说明 ： string 代表文本字符串形式， hex 代表十六进制原始字节。

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例5.png
   :alt: RS485与MQTT透传示例5.png


**示例**

|  本次使用uart2接口，设备路径为图ttyS6，具体配置如图所示

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例6.png
   :alt: RS485与MQTT透传示例6.png


MQTT列表配置
~~~~~~~~~~~~~

参数说明
""""""""""

- **interface (接口名称)**

  - 含义 ：该 MQTT 连接的唯一识别名。
  - 说明 ：用于区分不同的云平台连接（如 mqtt1 、 mqtt2 ）。

- **status (启用状态)**

  - 含义 ：是否开启此 MQTT 连接。
  - 说明 ：设置为 enabled 后，开发板会尝试连接云端服务器。

- **serverURL (服务器地址)**

  - 含义 ：MQTT 服务器（Broker）的域名或 IP 地址。
  - 说明 ：例如 broker.emqx.io 。这是数据上报的目的地。

- **clientId (客户端 ID)**

  - 含义 ：开发板在云端的“身份证号”。
  - 说明 ：必须保证在同一个服务器上是唯一的，通常用于云平台识别具体的网关设备。

- **username / password (用户名 / 密码)**

  - 含义 ：连接云平台所需的身份验证凭据。
  - 说明 ：如果服务器开启了安全认证，需在此填写正确的账号密码。

- **topic_sub (订阅主题)**

  - 含义 ：开发板“听”哪个频道。
  - 说明 ：开发板会监听此主题，用于接收来自云端的远程控制指令。

- **topic_pub (发布主题)**

  - 含义 ：开发板“说”哪个频道。
  - 说明 ：开发板采集到的所有数据都会通过这个主题发布到云端。

- **payload (测试负载)**

  - 含义 ：默认的发送内容。
  - 说明 ：通常用于心跳包或测试连接是否正常。

- **qos (服务质量)**

  - 含义 ：消息传输的可靠性等级。
  - 说明 ：可选 0 （最多一次）、 1 （至少一次）、 2 （只有一次）。工业场景建议设为 1 。

- **timeout (超时时间)**

  - 含义 ：网络响应等待时间。
  - 说明 ：单位为 毫秒 。在网络环境较差时，可适当调大此值。

- **action / object / format (处理动作 / 转发对象 / 数据格式)**

  - 含义 ：云端指令下发后的去向。
  - 说明 ：例如 action 为 forward ， object 为 uart2 ， format 为 hex 。这意味着：云端下发的 MQTT 消息会自动转换成十六进制，并从 RS485 串口发给现场设备。

**示例**

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例7.png
   :alt: RS485与MQTT透传示例7.png


**完成这三个列表的修改后本次示例的网页配置完成，下一步对串口调试工具进行配置**

四、电脑
----------

|  进入串口调试工具

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例8.png
   :alt: RS485与MQTT透传示例8.png


|  默认界面如图所示：

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例9.png
   :alt: RS485与MQTT透传示例9.png


|  点击左上角图标->工具->自动应答控制，打开右侧工具栏后点击右侧自动应答

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例10.png
   :alt: RS485与MQTT透传示例10.png


|  对着右侧空白处点击右键->导入
|  将电表配置.cfg导入到自动应答中

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例11.png
   :alt: RS485与MQTT透传示例11.png


|  导入完成后如图所示：

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例12.png
   :alt: RS485与MQTT透传示例12.png


|  随后启动自动应答

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例13.png
   :alt: RS485与MQTT透传示例13.png

|  串口设置配置
   本次示例串口设置按照图中左侧所示：

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例14.png
   :alt: RS485与MQTT透传示例14.png


.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例15.png
   :alt: RS485与MQTT透传示例15.png

|  设置完成后点击启动将接收设置和发送设置都设置成hex

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例16.png
   :alt: RS485与MQTT透传示例16.png

|  完成配置开启串口
|  随后数据日志会有数据输出为完成启动：

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例17.png
   :alt: RS485与MQTT透传示例17.png


|  串口调试助手配置完成
  
**MQTT客户端配置**

|  在电脑上安装 MQTT 客户端工具（推荐 MQTTX 或 mosquitto_sub）。
|  点击左上角添加连接

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例18.png
   :alt: RS485与MQTT透传示例18.png

  
|  使用与网关中 serverURL 一致的 MQTT 服务器信息建立连接：

  - 服务器地址：例如 broker.emqx.io 或客户自建 IP
  - Port： 1883 （若无特殊说明）
  - username/password：可以默认为：admin和public
  - Client ID：一定不要和配置文件中的clientid一致，否则开发板的clientid会被踢掉

|  点击右上角connect进行连接
|  可以参考配置编辑中的mqtt列表进行配置。

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例19.png
   :alt: RS485与MQTT透传示例19.png


.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例20.png
   :alt: RS485与MQTT透传示例20.png


|  完成连接后点击添加订阅，可以参考配置文件的topic进行编辑。
|  本次示例：

|  如果你想看网关上传的数据（电表读数）
|  MQTTX 的 Topic 框里填： emqx/my_gw/shsadl_645_ack

|  如果你想看网关接收到的指令
|  MQTTX 的 Topic 框里填： emqx/my_gw/shsadl_645_req

|  按照之前已经完成的mqtt列表的配置来填

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例21.png
   :alt: RS485与MQTT透传示例21.png


|  最推荐的填法（全通配符）
|  如果你想同时看到上面两种数据，直接填： emqx/my_gw/#

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例22.png
   :alt: RS485与MQTT透传示例22.png


.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例23.png
   :alt: RS485与MQTT透传示例23.png

  
|  完成订阅后可以查看mqtt订阅端就配置完成了，就会弹出电表数据如图所示。

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例24.png
   :alt: RS485与MQTT透传示例24.png


|  注意：开发板启动后会自动执行miot程序。

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例25.png
   :alt: RS485与MQTT透传示例25.png


|  如果想终止
|  输入root进入登录后输入/etc/init.d/S99miot stop

.. figure:: /image/MYZR-其他/网关/GW510/RS485与MQTT透传示例26.png
   :alt: RS485与MQTT透传示例26.png

