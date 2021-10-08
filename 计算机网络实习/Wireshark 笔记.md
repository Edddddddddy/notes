# Wireshark 笔记



## Terminal order

```
dumpcap -i 1 -w /users/apple/data/sample.pcapng -b filesize:10000 -b files:10
```

## Filter

```
frame contain Google	//spesific to that string
frame matches Google	//insensitive 
```

## 实验一 intro

1. 列出上述步骤7中出现在未过滤的分组列表窗口的协议列中的3种不同的协议。

   答：tcp quic  arp udp tlsv1.2 dns http mdns icmv6  ssdp ocsp 

2. 从HTTP GET消息发送到HTTP OK回复需要多长时间？ (默认情况下，分组列表窗口中的时间列的值是自Wireshark开始捕获以来的时间(以秒为单位)。要想以日期格式显示时间，请选择Wireshark的“视图”下拉菜单，然后选择“时间显示格式”，然后选择“日期和时间”。)

   答：delta time = [Time since request: 0.333299000 seconds]

3. gaia.cs.umass.edu(也称为wwwnet.cs.umass.edu)的Internet地址是什么？您的计算机的Internet地址是什么？

   答：Internet Protocol Version 4, Src: 10.151.79.137, Dst: 128.119.245.12

4. 打印问题2提到的两个HTTP消息(GET和OK)。要这样做，从Wireshark的“文件”菜单中选择“打印”，然后选择“仅选中分组”和“按当前显示”按钮，然后单击确定。



## 实验二 http

【实验结果】

1. 您的浏览器是否运行HTTP版本1.0或1.1？服务器运行什么版本的HTTP？
    GET /wireshark-labs/HTTP-wireshark-file1.html HTTP/1.1\r\n

2. 您的浏览器会从接服务器接受哪种语言（如果有的话）？
    Accept-Language: zh-CN,zh;q=0.9\r\n

3. 您的计算机的IP地址是什么？ gaia.cs.umass.edu服务器地址呢？
    Source Address: 10.151.79.137
    Destination Address: 128.119.245.12

4. 服务器返回到浏览器的状态代码是什么？
    [Expert Info (Chat/Sequence): HTTP/1.1 200 OK\r\n]

5. 服务器上HTML文件的最近一次修改是什么时候？

   Last-Modified: Wed, 22 Sep 2021 05:59:01 GMT\r\n

6. 服务器返回多少字节的内容到您的浏览器？

   File Data: 371 bytes

7. 通过检查数据包内容窗口中的原始数据，你是否看到有协议头在数据包列表窗口中未显示？ 如果是，请举一个例子。

   *（未见）

8. 检查第一个从您浏览器到服务器的HTTP GET请求的内容。您在HTTP GET中看到了“IF-MODIFIED-SINCE”行吗？

   *（未见）


9. 检查服务器响应的内容。服务器是否显式返回文件的内容？ 你是怎么知道的？

   ![image-20210922152131400](/Users/apple/Library/Application Support/typora-user-images/image-20210922152131400.png)


10. 现在，检查第二个HTTP GET请求的内容。 您在HTTP GET中看到了“IF-MODIFIED-SINCE:”行吗？ 如果是，“IF-MODIFIED-SINCE:”头后面包含哪些信息？

    没有

    ![image-20210922153712469](/Users/apple/Library/Application Support/typora-user-images/image-20210922153712469.png)

11. 针对第二个HTTP GET，从服务器响应的HTTP状态码和短语是什么？服务器是否明确地返回文件的内容？请解释。

    304 modified  没有返回，因为调用了cache

    ### 长文件

12. 您的浏览器发送多少HTTP GET请求消息？哪个数据包包含了美国权利法案的消息？

    发送了一个get请求

    65号 

    ![image-20210922154956132](/Users/apple/Library/Application Support/typora-user-images/image-20210922154956132.png)

13. 哪个数据包包含响应HTTP GET请求的状态码和短语？

    ![image-20210922155037266](/Users/apple/Library/Application Support/typora-user-images/image-20210922155037266.png)

14. 响应中的状态码和短语是什么？

    200 ok

15. 需要多少包含数据的TCP段来执行单个HTTP响应和权利法案文本？

    四个

    ![image-20210922155242371](/Users/apple/Library/Application Support/typora-user-images/image-20210922155242371.png)

    ### 具有嵌入对象的html

16. 您的浏览器发送了几个HTTP GET请求消息？ 这些GET请求发送到哪个IP地址？

    3个get 发送到：前两个128.119.245.12 最后一个 178.79.137.164

    ![image-20210922155916921](/Users/apple/Library/Application Support/typora-user-images/image-20210922155916921.png)

17. 浏览器从两个网站串行还是并行下载了两张图片？请说明。

    此处应该顺序下载两张图片，但一张301 moved permanently

    ### HTTP认证

18. 对于您的浏览器的初始HTTP GET消息，服务器响应（状态码和短语）是什么响应？

    401 Unauthorized 

    ![image-20210922160600167](/Users/apple/Library/Application Support/typora-user-images/image-20210922160600167.png)

19. 当您的浏览器第二次发送HTTP GET消息时，HTTP GET消息中包含哪些新字段？

    第一次

    ![image-20210922161024729](/Users/apple/Library/Application Support/typora-user-images/image-20210922161024729.png)

    第二次 

    多了cache-control , authorization

    ![image-20210922161002302](/Users/apple/Library/Application Support/typora-user-images/image-20210922161002302.png)

## 实验三 dns

【实验结果】

### 常用命令

```
##nslookup 通过指定dns和选项访问url
nslookup -option1 -option2 host-to-find dns-server

##ifconfig

```

1. 运行*nslookup*以获取一个亚洲的Web服务器的IP地址。该服务器的IP地址是什么？

   166.111.4.100

   ![image-20210922172507762](/Users/apple/Library/Application Support/typora-user-images/image-20210922172507762.png)

2. 运行*nslookup*来确定一个欧洲的大学的权威DNS服务器。

   *无权威

   ![image-20210922173431687](/Users/apple/Library/Application Support/typora-user-images/image-20210922173431687.png)

3. 运行*nslookup*，使用问题2中一个已获得的DNS服务器，来查询Yahoo!邮箱的邮件服务器。它的IP地址是什么？

   *can't find

   ![image-20210922173453067](/Users/apple/Library/Application Support/typora-user-images/image-20210922173453067.png)

4. 找到DNS查询和响应消息。它们是否通过UDP或TCP发送？

   ![image-20210923132903543](/Users/apple/Library/Application Support/typora-user-images/image-20210923132903543.png)

5. DNS查询消息的目标端口是什么？ DNS响应消息的源端口是什么？

   （上图）目标端口是53

   响应端口3163

6. DNS查询消息发送到哪个IP地址？使用ipconfig来确定本地DNS服务器的IP地址。这两个IP地址是否相同？

   一致，都为本地，没有设置dns （因为用了作者提供的包，）

7. 检查DNS查询消息。DNS查询是什么"Type"的？查询消息是否包含任何"answers"？

   typeA 

   无ansers， Answer RRS：0

   ![image-20210922174859238](/Users/apple/Library/Application Support/typora-user-images/image-20210922174859238.png)

8. 检查DNS响应消息。提供了多少个"answers"？这些答案具体包含什么？

   3个

   1 type=CNAME //cname记录是别名记录，也成为规范名字。这种记录允许将多个名字映射到同一台计算机

   2 type=A //指定主机名或域名对应的IP记录

   3 type=A

   ![image-20210922175225576](/Users/apple/Library/Application Support/typora-user-images/image-20210922175225576.png)

9. 考虑从您主机发送的后续TCP SYN数据包。 SYN数据包的目的IP地址是否与DNS响应消息中提供的任何IP地址相对应？

   对应第一个

   ![image-20210923133708804](/Users/apple/Library/Application Support/typora-user-images/image-20210923133708804.png)

   ![image-20210923133723364](/Users/apple/Library/Application Support/typora-user-images/image-20210923133723364.png)

10. 这个网页包含一些图片。在获取每个图片前，您的主机是否都发出了新的DNS查询？

    没有，使用了dns缓存

    ### nslookup

11. DNS查询消息的目标端口是什么？ DNS响应消息的源端口是什么？

    目标端口53，源端口60482

    ![image-20210923134943591](/Users/apple/Library/Application Support/typora-user-images/image-20210923134943591.png)

12. DNS查询消息的目标IP地址是什么？这是你的默认本地DNS服务器的IP地址吗？

    是设置的公共dns服务器

    ![image-20210923135527868](/Users/apple/Library/Application Support/typora-user-images/image-20210923135527868.png)

13. 检查DNS查询消息。DNS查询是什么"Type"的？查询消息是否包含任何"answers"？

    type=A，无ansers

    ![image-20210923135655026](/Users/apple/Library/Application Support/typora-user-images/image-20210923135655026.png)

14. 检查DNS响应消息。提供了多少个"answers"？这些答案包含什么？

    提供了三个ansers，包含两个规范主机地址，一个A类型，

    ![image-20210923140000361](/Users/apple/Library/Application Support/typora-user-images/image-20210923140000361.png)

15. 提供屏幕截图。

    ### nslookup 权威渠道

16. DNS查询消息发送到的IP地址是什么？这是您的默认本地DNS服务器的IP地址吗？

    同上

    ![image-20210923141649607](/Users/apple/Library/Application Support/typora-user-images/image-20210923141649607.png)

17. 检查DNS查询消息。DNS查询是什么"Type"的？查询消息是否包含任何"answers"？

    第一个是走设置腾讯的dns服务器，type=A

    ![image-20210923142005477](/Users/apple/Library/Application Support/typora-user-images/image-20210923142005477.png)

    第二个是ns类型，两者都不包含ansers

    ![image-20210923141846754](/Users/apple/Library/Application Support/typora-user-images/image-20210923141846754.png)

18. 检查DNS响应消息。响应消息提供的MIT域名服务器是什么？此响应消息还提供了MIT域名服务器的IP地址吗？

    提供了权威dns域名，没提供ip

    ![image-20210923142130258](/Users/apple/Library/Application Support/typora-user-images/image-20210923142130258.png)

19. 提供屏幕截图。

20. DNS查询消息发送到的IP地址是什么？这是您的默认本地DNS服务器的IP地址吗？如果不是，这个IP地址是什么？

    发送到我设置的dns服务器，

    ![image-20210923142600515](/Users/apple/Library/Application Support/typora-user-images/image-20210923142600515.png)

    *//根据网络上的答案反应，后使用作者提供的包*

    ![image-20210923142945939](/Users/apple/Library/Application Support/typora-user-images/image-20210923142945939.png)

    这个ip是：

    ![image-20210923143133506](/Users/apple/Library/Application Support/typora-user-images/image-20210923143133506.png)

21. 检查DNS查询消息。DNS查询是什么"Type"的？查询消息是否包含任何"answers"？

    type=PTR，A，A，均没有ansers

    ![image-20210923143920946](/Users/apple/Library/Application Support/typora-user-images/image-20210923143920946.png)

22. 检查DNS响应消息。提供了多少个"answers"？这些答案包含什么？

    1）ptr查询的response，返回了权威dns服务器域名和ip

    ![image-20210923144207644](/Users/apple/Library/Application Support/typora-user-images/image-20210923144207644.png)

    ![image-20210923144302854](/Users/apple/Library/Application Support/typora-user-images/image-20210923144302854.png)

    2）两次type=A，查询权威dns服务器，失败则no such name，成功则no error，并补上其他（权威？）dns域名

    ![image-20210923144400690](/Users/apple/Library/Application Support/typora-user-images/image-20210923144400690.png)

23. 提供屏幕截图。

## 实验四 tcp

【实验目的】

1. 了解 TCP 的 SEQ 和 ACK 序列号和确认号在 TCP 协议中的作用
2. 了解 TCP 建立连接三次握手的过程
3. 了解 TCP 的拥塞控制算法
4. 进行 TCP 连接性能的计算
5. 加强对 TCP 报文段结构的了解

【实验步骤】

1. 下载爱丽丝梦游记并且进行上传抓包操作(略)
2. 分析结果(见下部分)

【实验结果】

1. 将文件传输到 gaia.cs.umass.edu 的客户端计算机(源)使用的 IP 地址和 TCP 端口 号是什么

   ans:

   客户端ip：10.151.77.67 port:62727

   ![image-20210923152158915](/Users/apple/Library/Application Support/typora-user-images/image-20210923152158915.png)

2. gaia.cs.umass.edu 的 IP 地址是什么?在哪个端口号上发送和接收此连接的 TCP 区 段?

   ans：

   Ip:128.119.245.12 port:80

3. 客户端计算机(源)将文件传输到 gaia.cs.umass.edu 所使用的 IP 地址和 TCP 端口 号是多少?

   Ans:同上

4. 用于在客户端计算机和 gaia.cs.umass.edu 之间启动 TCP 连接的 TCP SYN 区段的序列 号是什么?将区段标识为 SYN 区段的区段有什么功能?

   Ans: 

   Seq=1，是随机的序列号，syn=1用来建立连接

   ![image-20210923154223104](/Users/apple/Library/Application Support/typora-user-images/image-20210923154223104.png)

5. gaia.cs.umass.edu 发送给客户端计算机以回复 SYN 的 SYNACK 区段的序列号是多 少?SYNACK 区段中的 Acknowledgment 栏位的值是多少?Gaia.cs.umass.edu 是如何 确定此 Acknowledgment 的数值的?在将区段标识为 SYNACK 区段的区段在连线中 有什么功能

   Ans:

   ack=2,synack区段中ack栏位为1

   ack=seq+1，作用为确认收到第一步请求，可以开始三次握手的第二步

   ![image-20210923155856984](/Users/apple/Library/Application Support/typora-user-images/image-20210923155856984.png)

6. 包含 HTTP POST 命令的 TCP 区段的序列号是多少?请注意，为了找到 POST 命令， 您需要深入了解 Wireshark 窗口底部的数据包内容字段，在其 DATA 栏位中查找带 有“POST”的区段

   ans:

   seq为1（psh代表有数据传输）

   ![image-20210923165624344](/Users/apple/Library/Application Support/typora-user-images/image-20210923165624344.png)

7. 将包含 HTTP POST 的 TCP 区段视为 TCP 连接中的第一个区段。在这个 TCP 连线中前 六个 TCP 区段的序列号是什么(包括包含 HTTP POST 的段)?

   Ans:如图 1，717，2079，3441，4803，6165

   每区段发送的时间是 什么时候?收到的每个区段的 ACK 是什么时候?鉴于发送每个 TCP 区段的时间与 收到确认的时间之间的差异，六个区段中每个区段的 RTT 值是多少?收到每个 ACK 后，EstimatedRTT 值(参见本节中的第 3.5.3 节，第 242 页)是什么?假设第一个 EstimatedRTT 的值等于第一个区段的测量 RTT，然后使用课本第 242 页的 EstimatedRTT 公式计算所有后续区段。(译注:中译本的页数可能不同)。

   ans:

   ![image-20210923165624344](/Users/apple/Library/Application Support/typora-user-images/image-20210923165624344.png)

   以seq=1为例：

   timestamp value:Timestamp value: 963054886

   Len:716

   Ack-seq:1

   Segment  rtt to ack:[The RTT to ACK the segment was: 0.339688000 seconds]

   EstimatedRTT= (1-0.125)*第一组的RTT+ 0.125*第五组的RTT （从而求解第二个开始的eRTT，懒得做了。。。）

   （注：seq= 上一个seq+len）

8. 前六个 TCP 区段的长度是多少

   ans:上图

9. 对于整个跟踪包，收到的最小可用缓冲区空间量是多少?缺少接收器缓冲区空间是 否会限制发送方传送 TCP 区段?

   ans:

   我收到最小：上图12帧的win=238

   第二问网上答案：因为win没有到0，因此没有启动等待问询协议*

10. 在跟踪文件中是否有重传的区段?为了回答这个问题，您检查了什么(在跟踪包

    中)?

    ans:

    观察序列号变化，没有发生重传

    ![image-20210923173203725](/Users/apple/Library/Application Support/typora-user-images/image-20210923173203725.png)

11. 接收器通常在 ACK 中确认多少数据?您是否可以识别接收方每隔一个接收到的区 段才发送确认的情况(参见本文第 250 页的表 3.2)

    ans:

    上图划动窗口逐渐变大，后期出现每隔一个区段才确认

12. TCP 连接的吞吐量(每单位时间传输的字节数)是多少?解释你如何计算这个值。

    ans:

    吞吐量=数据传输大小/所用时间=153037byte/1.890461s=79.05489471218925kb/s

    

    ![image-20210923174548368](/Users/apple/Library/Application Support/typora-user-images/image-20210923174548368.png)

