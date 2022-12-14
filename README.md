# iot-modbus

#### 介绍
物联网通讯协议，基于netty框架，支持COM（串口）和TCP协议，支持服务端和客户端两种模式，实现Java控制智能设备，同时支持设备组多台设备高并发通讯。采用工厂设计模式，代码采用继承和重写的方式实现高度封装，可作为SDK提供封装的接口，让具体的业务开发人员无需关心通讯协议的底层实现，直接调用接口即可使用。实现了心跳、背光灯、扫码、刷卡、指静脉、温湿度和门锁（支持多锁）、LCD显示屏等指令控制、三色报警灯控制。代码注释丰富，包括上传和下发指令调用例子，非常容易上手。

#### 版本说明
1.  V1.0.0版本仅支持TCP服务端通讯模式；
2.  V2.0.0版本支持TCP服务端和客户端两种模式，客户端模式还增加了心跳重连机制。
3.  V3.0.0版本支持COM（串口）和TCP协议，增加logback日志按文件大小和时间切割输出。
4.  V3.1.0版本代码优化，抽取公共模块子工程。
5.  V3.2.0版本TCP通讯增加支持LCD显示屏控制指令，支持批量控制LCD显示屏。
6.  V3.2.1版本串口通讯增加支持LCD显示屏控制指令，支持批量控制LCD显示屏。
7.  V3.2.2版本串口通讯接收指令数据拆包处理代码优化，网口通讯增加支持三色报警灯控制指令。
8.  V3.2.3版本串口通讯增加支持三色报警灯控制指令，串口通讯接收指令数据拆包处理代码优化。
9.  V3.2.4版本使用netty集成Rxtx对串口数据进行数据拆包处理，并且对指静脉指令进行优化。
10. V3.2.5版本客户端模式支持同时连接多个服务端下发和接收指令数据。
11. V3.2.6版本支持设备上线、掉线、处理业务异常监听处理。
12. V3.2.7版本优化客户端模式掉线/断网向服务端发起重连的机制。
13. V3.2.8版本优化服务端上线、掉线监听处理，以及对客户端心跳检测。

#### 软件架构
软件架构说明
基础架构采用Spring Boot2.x + Netty4.X + Maven3.6.x，日志采用logback。

#### 安装教程

1.  系统Windows7以上；
2.  安装Jdk1.8以上；
2.  安装Maven3.6以上；
3.  代码以Maven工程导入Eclipse或Idea。

#### 使用说明

1.  工程结构说明：
- iot-modbus                //物联网通讯父工程
- ├── doc                   //文档管理
- ├── iot-modbus-client     //netty通讯客户端
- ├── iot-modbus-common     //公共模块子工程
- ├── iot-modbus-netty      //netty通讯子工程
- ├── iot-modbus-serialport //串口通讯子工程
- ├── iot-modbus-server     //netty通讯服务端
- ├── iot-modbus-test       //使用样例子工程
- └── tools                 //通讯指令调试工具
2.  配置文件查看iot-modbus-test子工程resources目录下的application.yml文件；
3.  启动文件查看iot-modbus-test子工程App.java文件；
4.  服务启动后，服务端端口默认为：8080，网口通讯端口默认为：4000，串口通讯默认串口为：COM1；
5.  通讯指令调试工具，TCP通讯模式使用tools目录下的NetAssist.exe，串口通讯模式使用tools目录下的UartAssist.exe；
6.  通讯指令采用Hex编码（十六进制）；
7.  串口通讯依赖文件查看doc/serialport目录，Windows环境下rxtxParallel.dll和rxtxSerial.dll文件拷贝到JDK安装的bin目录下，Linux环境将librxtxParallel.so和librxtxSerial.so文件拷贝到JDK安装的bin目录下；
8.  postman指令下发样例请查看doc/postman/通讯指令下发.postman_collection.json文件，包括门锁（单锁/多锁）、扫码、背光灯、LCD显示屏、三色报警灯指令。

#### 指令格式

1.  以心跳指令（7E 04 00 BE 01 00 00 74 77 7F）作为样例说明，下标从0开始；
2.  第0位为起始符，长度固定占1个字节，固定格式：7E；
3.  第1、2位为数据长度，计算方法是从命令符到数据位（即：从3位到指令长度-3位），长度固定占2个字节，例如：04 00，表示长度为4；
4.  第3位为指令符，长度固定占1个字节，例如：BE，表示心跳指令；
5.  第4位为设备号，长度固定占1个字节，例如：01，表示设备号为1；
6.  第5位为层地址，长度固定占1个字节，例如：00，表示设备所有的层不执行；
7.  第6位为槽地址，长度固定占1个字节，例如：00，表示设备所有的槽不执行；
8.  指令长度-3位到-2位为校验位，采用CRC16_MODBUS（长度,命令,地址,数据）校验，例如：74 77，详细查看：ModbusCrc16Utils.java工具类；
9.  末位为结束符，长度固定占1个字节，固定格式：7F。

#### 通讯指令

1.  心跳上传指令
- iot-modbus作为服务端，通过心跳来维持通讯，启动服务端后，打开NetAssist.exe指令调试工具，先往服务端发送心跳指令；
- 硬件往服务端发送：7E 04 00 BE 01 00 00 74 77 7F ，为必要指令。
2.  背光灯上传指令
- 硬件往服务端发送：7E 05 00 88 01 00 00 00 AF E3 7F 
3.  扫码指令下发
- 服务端往硬件下发：7E 05 00 08 01 00 00 01 6F FD 7F 
- 第7位为数据位，长度固定占1个字节，例如：01，表示开开启扫码头。
4.  扫码指令上传
- 硬件往服务端发送：7E 24 00 8F 01 00 00 03 45 30 30 34 30 31 30 38 32 38 30 32 41 36 39 33 0D 02 00 00 01 02 13 73 02 00 00 01 02 13 73 9B 79 7F
- 数据位：03 45 30 30 34 30 31 30 38 32 38 30 32 41 36 39 33 0D 02 00 00 01 02 13 73 02 00 00 01 02 13 73为条码信息。
5.  刷卡指令上传
- 硬件往服务端发送：7E 08 00 84 01 00 00 86 14 AE 02 7C 53 7F 
- 数据位：86 14 AE 02为卡号信息。
6.  单开锁下发指令
- 服务端往硬件下发：7E 05 00 03 01 00 00 01 CA 3C 7F
- 第7位为数据位，长度固定占1个字节，例如：01，表示开1号锁。
7.  多开锁下发指令
- 服务端往硬件下发：7E 08 00 03 FF FF FF 01 00 02 01 7F B0 7F 
- FF FF FF为指令做兼容填补位，后面 01 00 02 01是数据位，其中：01表示1号锁，00表示上锁；02表示2号锁，01表示开锁。
8.  锁状态上传指令
- 硬件往服务端发送：7E 0D 00 83 01 00 00 FF FF FF 01 00 05 02 00 01 EE 99 7F 
- FF FF FF为指令做兼容填补位，后面 01 00 05 02 00 01是数据位，其中：01表示1号锁，00表示上锁，05表示传感器状态码；02表示2号锁，00表示上锁，01表示传感器状态码。
9.  指静脉和温湿度指令（不作说明，详细查看代码）；
10. LCD显示屏批量控制下发指令（不作说明，详细查看代码）；
11. 三色报警灯控制下发指令（不作说明，详细查看代码）。

#### 调试说明

1.  找到iot-modbus-test子工程App.java文件启动服务端，如下图所示：
- ![输入图片说明](doc/picture/1%E9%A1%B9%E7%9B%AE%E5%90%AF%E5%8A%A8.png)
- 说明：项目启动成功后，控制台日志输出服务端的端口为：8080；项目服务名为：iot-modbus-test；服务端开启socket通讯端口为：4000。
2.  将工程tools目录下通讯指令调试工具NetAssist拷贝到Windows桌面，双击打开，并配置参数，如下图所示：
- ![输入图片说明](doc/picture/2NetAssist%E5%8F%82%E6%95%B0%E9%85%8D%E7%BD%AE.png)
- 说明：协议类型选择TCP Client（调试工具作为模拟硬件通讯的客服端）；远程主机地址输入本地电脑的IP地址；远程主机端口输入服务端端口4000；接收和发送的编码选择HEX；最后点击连接按钮进行连接，连接成功后服务端控制台日志输出如下图所示：
- ![输入图片说明](doc/picture/3%E8%BF%9E%E6%8E%A5%E6%88%90%E5%8A%9F.png)
3.  客户端往服务端上传心跳指令，如下图所示：
- ![输入图片说明](doc/picture/4%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%BF%83%E8%B7%B3%E6%8C%87%E4%BB%A4%E4%B8%8A%E4%BC%A0.png)
- 说明：拷贝心跳指令到通讯指令调试工具NetAssist的数据发送窗口粘贴进来，然后点击发送按钮；此时服务端将接收到心跳指令，如下图所示：
- ![输入图片说明](doc/picture/5%E6%9C%8D%E5%8A%A1%E7%AB%AF%E6%8E%A5%E6%94%B6%E5%88%B0%E5%BF%83%E8%B7%B3%E6%8C%87%E4%BB%A4.png)
- 说明：客服端与服务端的通讯连接需要通过客户端定时往服务端发送心跳指令来维持，在生产环境中发送频率一般可设置为每5秒一次，如果通讯连接断开则客服端与服务端无法通讯。注意：在调试的过程中，如果通讯指令调试工具NetAssist与服务端通讯连接断开，则要手动点击NetAssist的连接按钮，重新往服务端发送一条心跳的指令。
4.  使用Postman请求服务端往客户端下发控制单锁指令，如下图所示：
- ![输入图片说明](doc/picture/6postman%E8%AF%B7%E6%B1%82%E4%B8%8B%E5%8F%91%E6%8E%A7%E5%88%B6%E5%8D%95%E9%94%81%E6%8C%87%E4%BB%A4.png)
- 说明：在Postman输入服务端发送控制单锁指令接口，填入请求地址和参数：http://127.0.0.1:8080/iot-modbus-test/test/openlock/1/1，服务端控制台日志输出如下图所示：
- ![输入图片说明](doc/picture/7%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8B%E5%8F%91%E6%8E%A7%E5%88%B6%E5%8D%95%E9%94%81%E6%8C%87%E4%BB%A4.png)
- 客服端指令调试工具NetAssist接收到控制单锁指令，如下图所示：
- ![输入图片说明](doc/picture/8%E6%9C%8D%E5%8A%A1%E7%AB%AF%E6%8E%A5%E6%94%B6%E5%88%B0%E6%8E%A7%E5%88%B6%E5%8D%95%E9%94%81%E6%8C%87%E4%BB%A4.png)
5.  其他上传和下发的指令不作详细介绍，感兴趣的同学可以参考通讯指令和工程目录doc/postman目录下的请求样例。

#### 创作不易，别忘了点亮Star，你们的支持，是我源源不断的动力。

#### 欢迎加入交流群

- 微信公众号 
- ![输入图片说明](doc/picture/TF%EF%BC%88%E8%85%BE%E9%A3%9E%EF%BC%89%E5%BC%80%E6%BA%90%E5%85%AC%E4%BC%97%E5%8F%B7%E4%BA%8C%E7%BB%B4%E7%A0%81.jpg)
- QQ技术交流群
- ![输入图片说明](doc/picture/9%E7%89%A9%E8%81%94%E7%BD%91%E9%80%9A%E8%AE%AF%E5%8D%8F%E8%AE%AE%EF%BC%88iot-modbus%EF%BC%89%E4%BA%A4%E6%B5%81%E7%BE%A4%E7%BE%A4%E4%BA%8C%E7%BB%B4%E7%A0%81.png)
