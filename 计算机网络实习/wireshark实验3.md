Wireshark 实验3

### 实验八 DHCP 协议的研究

【实验目的】

1. 快速简单了解 DHCP 协议
2. 了解 DHCP 协议的 4 次步骤

【实验步骤】

1. 打开命令提示符，输入 ipconfig /release 释放当前的 IP

2. 打开 wireshark 进行抓包(略)

3. 命令提示符输入 ipconfig /renew 重新获取一个 IP

4. 命令提示符再次输入 ipconfig /renew

5. 再次输入 ipconfig /release 释放当前的 IP

6. 最后输入 ipconfig /renew 重新获取一个 IP

7. 停止抓包并且分析实验结果 

   【实验结果】

1. DHCP 消息是通过 UDP 还是 TCP 发送的? 

ANS:UDP

![image-20211004154652173](/Users/apple/Library/Application Support/typora-user-images/image-20211004154652173.png)

2. 绘制时间流图形。说明客户端和服务器之间第一次四个 DHCP 发现，DHCP 提供，DHCP 请求以及 DHCP 响应的顺序，说明您的结果中对于每个数据包，指示源和目标端口号 是否与本实验分配中给出的示例相同?
   ANS: 

   dhcp discover -> offer-> transaction -> ack

   source 为 本地和路由器设置，方式是广播

   ![image-20211004161152575](/Users/apple/Library/Application Support/typora-user-images/image-20211004161152575.png)

   

网络答案：（有广播申请，单播返回）

课本中说 4 个过程都是广播消息，但是在我实际抓包过程中并不是，我上网查了下这 种现象:知乎车小胖的回答:DHCP offer 报文到底是单播还是广播，他说消息可以广

播也可以单播，看是否支持(https://www.zhihu.com/question/280872108)。 因为使用单播更高效，不会打扰影响同网段的其他主机，猜测在 DHCP 获取过程中， DHCP 服务器会和交换机交互，了解是那个端口，那个 MAC 的电脑在请求 DHCP 获取 IP，因此单播回复更加高效。

3. 主机的链路层(例如以太网)地址是什么?

ANS: 00:08:74:4f:36:23

![image-20211004161641906](/Users/apple/Library/Application Support/typora-user-images/image-20211004161641906.png)

4. DHCP 发现消息中的哪些值将此消息与 DHCP 请求消息区不同?

ANS: 53 type不同，116 discover多了 dhcp auto-configuration , 50 discover多了request ip

5. 第一次四个 DHCP 发现，DHCP 提供，DHCP 请求以及 DHCP 响应的 Transaction-ID 值是 多少?Transaction-ID 字段目的是什么。
    ANS: 

   相同，为了保证请求和服务器相同，保证分配安全

6. 主机使用 DHCP 获取 IP 地址。主机在 DHCP 的 4 次问询和回答之后获取了地址。请问 如果在这 4 次 DHCP 问询和回答中，如果主机没有 IP 地址，那么 IP 数据报的值是什 么?请分别指出这 4 个 DHCP 的消息 IP 数据报源头和目标 IP。
    ANS: 

   主机如果没有 IP 地址，IP 数据报的值是 0.0.0.0，

   目的地址广播地址 255.255.255.255。

7. 您的 DHCP 服务器的 IP 地址是多少?
    ANS: 

   ![image-20211004163407923](/Users/apple/Library/Application Support/typora-user-images/image-20211004163407923.png)

8. 发送 DHCP Offer 消息的 DHCP 服务器 IP 是什么，指示哪条 DHCP 消息包含提供的 DHCP 地址。

ANS: 

如上图

9. 在作者的例子中，主机和 DHCP 服务器之间没有中继代理。跟踪中的哪些值表明没有中 继代理?您的实验中是否有中继代理?如果是这样，代理的 IP 地址是什么?
    ANS: 

   没有中继代理，没有通过中继网关和代理服务器。

网络：

DHCP 中续代理意义是，我们可以不在每个子网域配置多个 DHCP 服务器进行 DHCP 服务提供，而仅仅设置一台 DHCP 服务器，这样每个子网域路由当接受到客户端的。DHCP 请求时会自动转发请求给 DHCP 服务器完成服务。 

10. 解释 DHCP offer 消息中路由器和子网掩码字段的用途。
     ANS: 路由器起中继代理作用，子网掩码区分不同网段

11. 在脚注 2 作者提供的抓包结果中，DHCP 服务器会向作者提供特定的 IP 地址(请见上 面问题 8)。请问客户端接受使用是否对第一个提供 DHCP offer 消息的 DHCP 地址?客 户端的响应(DHCP 请求中)哪里是它所要求的地址。
     ANS:

    offer中，提供了ip； request 中使用了ip

    ![image-20211004165505102](/Users/apple/Library/Application Support/typora-user-images/image-20211004165505102.png)

    ![image-20211004165551036](/Users/apple/Library/Application Support/typora-user-images/image-20211004165551036.png)

12. 解释租约时间的目的。 您的实验中的租约时间有多长? 

    ANS: 1d

    ![image-20211004165651539](/Users/apple/Library/Application Support/typora-user-images/image-20211004165651539.png)

13. DHCP 释放消息的目的是什么?DHCP 服务器是否发出收到客户端 DHCP 释 放请求的确认。如果客户端的 DHCP 释放消息丢了会发生什么。 

    ANS:

    客户端用来发出释放该 IP 不再租用的信息。

    如图服务器并没有发出 DHCP 释放请 求的确认，而是直接收回了 IP;

    如果客户端的 DHCP 释放消息丢了，猜测就会继续使 用这个 IP 判断是否到达租用时间，以及是否续订。

    ![image-20211004170037349](/Users/apple/Library/Application Support/typora-user-images/image-20211004170037349.png)

14. 从 Wireshark 窗口中清除 bootp 过滤器。 在 DHCP 数据包交换期间是否发送或接 收了任何 ARP 数据包? 如果接收到了，请说明这些 ARP 数据包的用途。
     ANS : 

    ![image-20211004171517101](/Users/apple/Library/Application Support/typora-user-images/image-20211004171517101.png)arp广播用来获取路由的mac地址并且到本机的arp缓存表。

    ![image-20211004171652757](/Users/apple/Library/Application Support/typora-user-images/image-20211004171652757.png)

