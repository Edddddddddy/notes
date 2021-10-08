# Wireshark 实验2

## 实验五

【实验目的】

1. 快速简单了解 UDP 协议
2. 了解 UDP 的标头数据，报文段数据结构

【实验步骤】 下载作者的实验结果，并且打开(略)。 使用 UDP 过滤器过滤实验结果。

【实验结果】

1. 从跟踪中选择一个 UDP 数据包。从此数据包中，确定 UDP 标头中有多少字段。(建议不要查看课本，直接根据您的数据包跟踪结果回答)，并为这些字段命名.

   Ans:

   报头4个，每个4B，一共16B

   源端口，目标端口，报文长度，校验和

   ![image-20210924154959607](/Users/apple/Library/Application Support/typora-user-images/image-20210924154959607.png)

2. 通过查询 Wireshark 的数据包内容字段中显示的信息，确定每个 UDP 报头字段的长 度(以字节为单位)

   Ans:

   同qes1

3. 长度字段中的值是指的是什么?(此问题您可以参考课本)。使用捕获的 UDP 数据 包验证您的声明。

   Ans:

   length=40+8

   8byte为udp报头

4. UDP 有效负载中可包含的最大字节数是多少?(提示:这个问题的答案可以通过你 对上述 2 的回答来确定

   Ans:

   长度占2byte，可表示最大长度（头+data）为2e16=65536byte，去掉udp头8byte，为65528byte

5. 最大可能的源端口号是多少?(提示:见 4 中的提示)

   Ans:

   source port 2byte，从0开始，因此最大65535

6. UDP 的协议号是什么?以十六进制和十进制表示法给出答案。要回答这个问题，您 需要查看包含此 UDP 段的 IP 数据报的 Protocol 字段(参见书中的图 4.13 和 IP 头字段的讨论)

   Ans:

   Udp17 0x11

   Tcp 6 0x6

7. 观察发送 UDP 数据包后接收响应的 UDP 数据包，这是对发送的 UDP 数据包的回复， 请描述两个数据包中端口号之间的关系。(提示:对于响应 UDP 目的地应该为发送 UDP 包的地址

   Ans:

   应该是交换，我这里两个都相同

   ![image-20210924160345506](/Users/apple/Library/Application Support/typora-user-images/image-20210924160345506.png)

![image-20210924160406093](/Users/apple/Library/Application Support/typora-user-images/image-20210924160406093.png)



## 实验六 IP

【实验目的】

1. 快速简单了解 IP 协议，特别是 IP 数据报
2. 简单了解 ICMP 协议，TTL 的作用
3. 了解 IP 数据报的字段的意义
4. 研究 IP 数据的分片方法

【实验步骤】

1. 去下载 pingplotter(我以我的 Windows 平台为例)，安装并选择专业版

   14 天试用。

2. 打开 wireshark 抓包，并且 pingplotter 设置 Edit->Options->Default

   Settings->Engine 包大小设置为 56Byte。

3. 然后打开 gaia.cs.umass.edu 的跟踪，大约跟踪 3 次(由于无法控制，可 以稍微超一些)，然后暂停。

4. 重复 2-3 步，只不过将大小设置 2000Byte 以及 3500Byte，然后也大约跟 踪 3 次。

   这期间保持 wireshark 一直打开，总共设置 3 次，每次差不多也跟踪 3 次。

由于暂停 pingplotter 再恢复是接着上次的跟踪统计继续，因此序号会一 直增加。
 当然也可以选择重新开一个跟踪。

5. 关闭 wireshark，并且分析实验结果。 

【实验结果】

PingPlotter 是通过 ICMP 协议发送不同 TTL 值 PING 包，来计算和获取访问网 站的所经过路由。TTL 是存活时间，每经过一次路由器，存活时间就会减一，当 其为 0 时候，路由会丢掉这个包并且发出 TTL 超时给原始的发出者，防止死包耗 尽网络资源。然后每次 PingPlotter 都会发送起始从 TTL=1 的数据包，然后逐渐 增大 TTL，用来获取所访问网站经过的路由。但是有些路由会因为安全不回应这 些包，所以我们还会看到有些请求并没有回复，而 TTL 会持续增加获取下一个路 由的回复。

1. 选择计算机发送的第一个 ICMP Echo Request 消息，然后在 packet details window 中展开数据包的 Internet 协议部分。您的计算机的 IP 地址是多 少?

   Ans:

   10.151.77.76

   ![image-20210926135434258](/Users/apple/Library/Application Support/typora-user-images/image-20210926135434258.png)

2. 在 IP header 中，上层协议字段的值是多少?

   ![image-20210926135512686](/Users/apple/Library/Application Support/typora-user-images/image-20210926135512686.png)

3. ## IP header 有多少 bytes?IP datagram 的有效负载中有多少 bytes?说明如何确定 payload bytes 的数。

   ip长总长度56byte，其中ip头20byte，icmp协议36byge

   ![image-20210926135631421](/Users/apple/Library/Application Support/typora-user-images/image-20210926135631421.png)

4. 此 IP 数据报是否已被分段(fragmented)?解释您如何确定数据报是否已被分段 (fragmented)。

   Ans:没有分段,more fragments not set

   ![image-20210926140244476](/Users/apple/Library/Application Support/typora-user-images/image-20210926140244476.png)

5. 在您的计算器发送的这一系列 ICMP 消息中，IP 数据报中的哪些字段一直改变?

   Ans:

   ip变化项有：identivication（标识符）, time to live（存活时间）, header checksum（ip头检验吗）

   ![image-20210926140527725](/Users/apple/Library/Application Support/typora-user-images/image-20210926140527725.png)

   ![image-20210926140510453](/Users/apple/Library/Application Support/typora-user-images/image-20210926140510453.png)

   icmp变化项有：sequence number （be），sequence number （le）

   **windows**为**LE**：**little-endian byte order**，**Linux**为**BE**：**big-endian**，发送顺序不一样，此处wireshark分别显示

   ![image-20210926141155371](/Users/apple/Library/Application Support/typora-user-images/image-20210926141155371.png)

6. 哪些字段保持不变?哪个字段必须保持不变?哪些字段必须更改?为什么?

   Ans:

   必须保持不变：版本，首部长度，区分服务（弃用），协议

   保持不变（可能会变）：总长度，标志，片偏移，目标ip，本地ip，选项

   必须要变：标识符，ttl，校验和，data

   ![image-20210926144041395](/Users/apple/Library/Application Support/typora-user-images/image-20210926144041395.png)

7. 描述您在 IP datagram 的 Identification field 中的值中所看到的

   Ans:

   Id field 随着每一个icmp ping 增值

   ![image-20210926145046865](/Users/apple/Library/Application Support/typora-user-images/image-20210926145046865.png)

8. ID 字段和 TTL 字段的值是多少?

   Ans:

   Id = 0xbd4b

   Til = 1

9. 对于最近(第一跳)路由器发送到您的计算器的所有 ICMP TTL 超出的回复，这些值 是否保持不变?为什么?

   Ans:

   ip数据包的id字段改变，因为是唯一的，ttl不变，而icmp的id不变，TTL字段保持不变，因为第一跳路由器的TTL始终相同。

   ![image-20210926151448478](/Users/apple/Library/Application Support/typora-user-images/image-20210926151448478.png)

   ![image-20210926151438442](/Users/apple/Library/Application Support/typora-user-images/image-20210926151438442.png)

10. 在将 pingplotter 中的数据包大小更改为 2000 后，查找计算机发送的第一个 ICMP Echo Request 消息。该消息是否已碎片化为多个 IP 数据报

    Ans:

    是的，ip数据包显示ipv4的ip破碎片，dierge

    ![image-20210926160253882](/Users/apple/Library/Application Support/typora-user-images/image-20210926160253882.png)

11. 打印出碎片 IP 数据报的第一个片段。IP 头中的哪些信息表明数据报已碎片化?IP 头中的哪些信息表明这是第一个片段还是后一个片段?这个 IP 数据报有多长

    Ans:

    长度为1500，14为以太网上层长度

    已经被分段，off=0，说明是第一个分段

    ![image-20210926160247564](/Users/apple/Library/Application Support/typora-user-images/image-20210926160247564.png)

12. 打印出碎片 IP 数据报的第二个片段。IP 标头中的哪些信息表明这不是第一个数据报 片段?是否还有更多的片段?你是如何知道的?

    Ans:

    偏移量为1480，因为第一个是1480+20+14

    ![image-20210926160311034](/Users/apple/Library/Application Support/typora-user-images/image-20210926160311034.png)

13. 在第一个和第二个片段中，IP 标头中哪些字段发生了变化?

    Ans:

    id，标志位，校验码

14. 从原始数据报创建了多少个片段

    Ans:

    创建了三个片段，（答案提供了快速查找方法，即：找到第一个连续两个ipv4，后面跟一个icmp即为第一个3500byte的request，因为被切割成两份）

    ![image-20210926204127554](/Users/apple/Library/Application Support/typora-user-images/image-20210926204127554.png)

15. 片段中 IP 标头中的哪些字段发生了变化?

    Ans:

    变化：id,fragment offset and checeksum，长度为1500 1500 540 ，more fragment bit 前两个是1，第三个是0

    

    

## 实验七 探究icmp，ping和traceroute

【实验目的】

1. 快速简单了解 ICMP 协议
2. 了解 ICMP 的格式和内容
3. 了解 Ping 命令以及 Tracert 命令(Windows)与 ICMP 数据报的之间关系。
4. 了解 Traceroute 命令与 tracert 命令的区别

【实验步骤】

1. 打开 Wireshark 启动捕获(略)

2. 打开命令提示符使用 ping 命令 ping 其他大陆的服务器 10 次,这里以

   gaia.cs.umass.edu 为例,命令语法

3. 保存数据包，并且启动下一次捕获(略)

4. 打开命令提示符进行路由跟踪，跟踪 gaia.cs.umass.edu，命令语法

CMD > ping -n 10 gaia.cs.umass.edu

CMD > tracert gaia.cs.umass.edu

5. 保存数据包，并且启动下一次捕获(略)，捕获接口选择虚拟机虚拟网卡 6. 在 Linux 虚拟机中进行路由跟踪，地址仍然是 gaia.cs.umass.edu，命令语法

   可以看到除了我虚拟机的虚拟路由网关，其他网关为了安全，均使用防火墙 屏蔽过滤我的 UDP 路由跟踪请求，所以会出现都是*情况，后文有说。

6. 同样，Linux 还可以指定路由跟踪使用 ICMP 数据报。命令语法

\# traceroute gaia.cs.umass.edu

\# traceroute gaia.cs.umass.edu -I

8. 进行实验数据结果分析。 

【实验结果】

1. 您的主机的 IP 地址是多少? 目标主机的 IP 地址是多少? 

   ANS:

   我的 IP 10.151.77.76。目标 IP 128.119.245.12

2. 为什么 ICMP 数据包没有源端口号和目的端口号?
   ANS:

   icmp是网络层协议，不需要tcp和udp，直接用ip报承载，通常不在网络中直接使用，icmp直接连接用户进程（应用层），ip连接tcp/udp运输层或直接连接应用层

3. 查看任意的请求 ICMP 数据包，ICMP 类型和代码是什么? 该 ICMP 数据 包还有哪些其他字段? 校验和，序列号和标识符字段有多少字节
   ANS: ICMP 类型是 8(代表 ICMP 请求)，代码是 0，ICMP 数据包还包括校 验码，ID 值，以及序号。校验和，序列号，标识符都是 16 个字符，4 个字 节。

4. 查看任意的响应 ICMP 数据包，ICMP 类型和代码是什么? 该 ICMP 数据 包还有哪些其他字段? 校验和，序列号和标识符字段有多少字节?
   ANS:

   icmp类型是8，代码是0，还有校验码，序列号，id

   ![image-20210926222627420](/Users/apple/Library/Application Support/typora-user-images/image-20210926222627420.png)

5. 您的主机的 IP 地址是多少? 目标目标主机的 IP 地址是多少? 

   ANS: 

   

6. 如果 ICMP 发送了 UDP 数据包(如在 Unix / Linux 中)，那么探测数据包 的 IP 协议号仍然是 01 吗? 如果没有，它会是什么?
   ANS:

   我的答案：

   协议号是UDP（17）

   ![image-20211004134857812](/Users/apple/Library/Application Support/typora-user-images/image-20211004134857812.png)

   ![image-20211004134351071](/Users/apple/Library/Application Support/typora-user-images/image-20211004134351071.png)

   网络答案：

   根据我抓包 Linux 虚拟机路由跟踪结果，发送请求路由跟踪的数据包 时 UDP 数据包，因此 IP 承载上层协议号时 17。虽然说是路由跟踪请求， 但是 Wireshark 把这里的请求全部识别成了 SKYPE 协议，我不知道为什么， 但是可以确认这就是 TTL 和发送端口逐渐增大的路由跟踪请求，且除了我 的虚拟机虚拟网络网关响应，其他路由网关均未响应。所以可以看到除了 我的虚拟机虚拟网络网关发了三个 TTL 超时 ICMP 消息，其他只有请求没有 回复。

   ![image-20211004091319220](/Users/apple/Library/Application Support/typora-user-images/image-20211004091319220.png)

7. 检查屏幕截图中的 ICMP 响应数据包。 这与本实验的前半部分中的 ICMP ping 查询数据包不同吗? 如果不同，请解释为什么?

   Ans:

   Type不同，ping是8（echo ping request），traceroute是11 （time-to-live exceeded）

8. 检查屏幕截图中的 ICMP 错误数据包。 它具有比 ICMP 响应数据包更多字段。 这个数据包含哪些内容?
   ANS:

   多了ipv4的请求包

   ![image-20211004092431117](/Users/apple/Library/Application Support/typora-user-images/image-20211004092431117.png)

9. 检查源主机收到的最后三个 ICMP 数据包。 这些数据包与 ICMP 错误数据 包有何不同? 他们为什么不同?
   ANS:

   网络答案：最后三个 ICMP(响应)数据包是目标主机发送给我的 ICMP 回应数据 包，因为路由查询是使用逐渐递增 TTL 的 ICMP 查询数据包，最后的 ICMP 查询数据包的 TTL 已经大于到达目的主机中间路由跃点数，因此不会被目 标主机丢弃，发送 ICMP 超时的数据包，所以只会收到 ICMP 响应数据包。

10. 在 tracert 跟踪测量中，是否有一个连接的延迟比其他连接长得多? 请参阅图 4 中的屏幕截图，是否有连接的延迟明显长于其他连接? 根据路由 器名称，您能猜出这个连接末端的两个路由器的位置吗?

     ANS:

    我的答案：

    这里TTL=18之后延迟明显增大，使用best trace查询到此处为北京到加利福尼亚

    ![image-20211004135641561](/Users/apple/Library/Application Support/typora-user-images/image-20211004135641561.png)

    ![image-20211004140047428](/Users/apple/Library/Application Support/typora-user-images/image-20211004140047428.png)

    

