# TCP与UDP之区别

[Differences between TCP and UDP](https://www.geeksforgeeks.org/differences-between-tcp-and-udp/)

| Basis                    | Transmission control protocol (TCP)                          | User datagram protocol (UDP)                                 |
| :----------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Type of Service          | TCP is a connection-oriented protocol. Connection-orientation means that the communicating devices should establish a connection before transmitting data and should close the connection after transmitting the data. | UDP is the Datagram-oriented protocol. This is because there is no overhead for opening a connection, maintaining a connection, and terminating a connection. UDP is efficient for broadcast and multicast types of network transmission. |
| Reliability              | TCP is reliable as it guarantees the delivery of data to the destination router. | The delivery of data to the destination cannot be guaranteed in UDP. |
| Error checking mechanism | TCP provides extensive error-checking mechanisms. It is because it provides flow control and acknowledgment of data. | UDP has only the basic error checking mechanism **using checksums.** |
| Acknowledgment           | An acknowledgment segment is present.                        | No acknowledgment segment.                                   |
| Sequence                 | Sequencing of data is a feature of Transmission Control Protocol (TCP). this means that packets arrive in order at the receiver. | There is no sequencing of data in UDP. If the order is required, it has to be managed by the application layer. |
| Speed                    | TCP is comparatively slower than UDP.                        | UDP is faster, simpler, and more efficient than TCP.         |
| Retransmission           | Retransmission of lost packets is possible in TCP, but not in UDP. | There is no retransmission of lost packets in the User Datagram Protocol (UDP). |
| Header Length            | TCP has a **(20-60) bytes variable length header.**          | UDP has an 8 bytes fixed-length header.                      |
| Weight                   | TCP is heavy-weight.                                         | UDP is lightweight.                                          |
| Handshaking Techniques   | Uses handshakes such as SYN, ACK, SYN-ACK                    | It’s a connectionless protocol i.e. No handshake             |
| Broadcasting             | TCP doesn’t support Broadcasting.                            | UDP supports Broadcasting.                                   |
| Protocols                | TCP is used by HTTP, HTTPs, FTP, SMTP and Telnet.            | UDP is used by DNS, DHCP, TFTP, SNMP, RIP, and VoIP.         |
| Stream Type              | The TCP connection is a byte stream.                         | UDP connection is message stream.                            |
| Overhead                 | Low but higher than UDP.                                     | Very low.                                                    |



## 头部

UDP

![](TCP与UDP之区别.assets\udp_header.png)

TCP

![](TCP与UDP之区别.assets\tcp_header.jpg)

**序号：**用于对字节流进行编号，例如序号为 301，表示第一个字节的编号为 301，如果携带的数据长度为 100 字节，那么下一个报文段的序号应为 401。

**确认号：**期望收到的下一个报文段的序号。例如 B 正确收到 A 发送来的一个报文段，序号为 501，携带的数据长度为 200 字节，因此 B 期望下一个报文段的序号为 701，B 发送给 A 的确认报文段中确认号就为 701。

**数据偏移：**指的是数据部分距离报文段起始处的偏移量，实际上指的是首部的长度。

**控制位：**八位从左到右分别是 CWR，ECE，URG，ACK，PSH，RST，SYN，FIN。

**CWR：**CWR 标志与后面的 ECE 标志都用于 IP 首部的 ECN 字段，ECE 标志为 1 时，则通知对方已将拥塞窗口缩小；

**ECE：**若其值为 1 则会通知对方，从对方到这边的网络有阻塞。在收到数据包的 IP 首部中 ECN 为 1 时将 TCP 首部中的 ECE 设为 1；

**URG：**该位设为 1，表示包中有需要紧急处理的数据，对于需要紧急处理的数据，与后面的紧急指针有关；

**ACK：**该位设为 1，确认应答的字段有效，TCP规定除了最初建立连接时的 SYN 包之外该位必须设为 1；

**PSH：**该位设为 1，表示需要将收到的数据立刻传给上层应用协议，若设为 0，则先将数据进行缓存；

**RST：**该位设为 1，表示 TCP 连接出现异常必须强制断开连接；

**SYN：**用于建立连接，该位设为 1，表示希望建立连接，并在其序列号的字段进行序列号初值设定；

**FIN：**该位设为 1，表示今后不再有数据发送，希望断开连接。当通信结束希望断开连接时，通信双方的主机之间就可以相互交换 FIN 位置为 1 的 TCP 段。

每个主机又对对方的 FIN 包进行确认应答之后可以断开连接。不过，主机收到 FIN 设置为 1 的 TCP 段之后不必马上回复一个 FIN 包，而是可以等到缓冲区中的所有数据都因为已成功发送而被自动删除之后再发 FIN 包；

**窗口：**窗口值作为接收方让发送方设置其发送窗口的依据。之所以要有这个限制，是因为接收方的数据缓存空间是有限的。

## 有序性

- TCP保证了消息的有序性，即使到达客户端顺序不同，TCP也会排序。
- UDP不提供有序性保证。

## 数据边界

- TCP不保存数据边界。
  - 虽然TCP也将在收集所有字节之后生成一个完整的消息，但是这些信息在传给传输给接受端之前将储存在TCP缓冲区，以确保更好的使用网络带宽。
- UDP保证。
  - 在UDP中，数据包单独发送的，只有当他们到达时，才会再次集成。包有明确的界限来哪些包已经收到，这意味着在消息发送后，在接收器接口将会有一个读操作，来生成一个完整的消息。

## 拥塞或流控制

- TCP有流量控制。
  - 在任何用户数据可以被发送之前，TCP需要三数据包来设置一个套接字连接。TCP处理的可靠性和拥塞控制。
- UDP不能进行流量控制。

## 应用

- 由于TCP提供可靠交付和有序性的保证，它是最适合需要高可靠并且对传输时间要求不高的应用。
- UDP是更适合的应用程序需要快速，高效的传输的应用，如游戏。
- UDP是无状态的性质，在服务器端需要对大量客户端产生的少量请求进行应答的应用中是非常有用的。
- 在实践中，TCP被用于金融领域，如FIX协议是一种基于TCP的协议，而UDP是大量使用在游戏和娱乐场所。

## 上层使用的协议

- 基于TCP协议的：Telnet，FTP以及SMTP协议。
- 基于UDP协议的：DHCP、DNS、SNMP、TFTP、BOOTP。

### 运行在TCP协议上的协议：

HTTP（Hypertext Transfer Protocol，超文本传输协议），主要用于普通浏览。
HTTPS（HTTP over SSL，安全超文本传输协议）,HTTP协议的安全版本。
FTP（File Transfer Protocol，文件传输协议），用于文件传输。
POP3（Post Office Protocol, version 3，邮局协议），收邮件用。
SMTP（Simple Mail Transfer Protocol，简单邮件传输协议），用来发送电子邮件。
TELNET（Teletype over the Network，网络电传），通过一个终端（terminal）登陆到网络。
SSH（Secure Shell，用于替代安全性差的TELNET），用于加密安全登陆用。

### 运行在UDP协议上的协议：

BOOTP（Boot Protocol，启动协议），应用于无盘设备。
NTP（Network Time Protocol，网络时间协议），用于网络同步。
DHCP（Dynamic Host Configuration Protocol，动态主机配置协议），动态配置IP地址。

### 运行在TCP和UDP协议上：

DNS（Domain Name Service，域名服务），用于完成地址查找，邮件转发等工作。
ECHO（Echo Protocol，回绕协议），用于查错及测量应答时间（运行在TCP和UDP协议上）。
SNMP（Simple Network Management Protocol，简单网络管理协议），用于网络信息的收集和网络管理。
DHCP（Dynamic Host Configuration Protocol，动态主机配置协议），动态配置IP地址。
ARP（Address Resolution Protocol，地址解析协议），用于动态解析以太网硬件的地址。
