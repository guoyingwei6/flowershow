---
created: 2023-11-05 20:16
updated: 2025-06-10 09:55
dg-publish: true
---

## 为什么 SSH 会关闭连接？

The short answer is that it all comes down to TCP timeouts. TCP timeout refers to the duration that a TCP connection or a network operation waits for a response before considering the process failed.
简短的回答是，这一切都归结为TCP超时。TCP 超时是指 TCP 连接或网络操作在认为进程失败之前等待响应的持续时间。

In Linux, TCP timeout settings determine how long a TCP connection or operation should wait before assuming that a packet has been lost or a connection has become unresponsive.
在 Linux 中，TCP 超时设置确定 TCP 连接或操作在假定数据包已丢失或连接无响应之前应等待多长时间。

In the case of keeping the SSH connection alive, there are three key system parameters that we will briefly discuss below.
在保持 SSH 连接处于活动状态的情况下，我们将在下面简要讨论三个关键系统参数。

`tcp_keepalive_time`: determines the interval between sending out TCP keepalive probes on an idle TCP connection. Keepalive probes check whether a remote peer is still alive and responsive, even when no data is being transferred.
tcp_keepalive_time：确定在空闲的 TCP 连接上发送 TCP keepalive 探测之间的间隔。Keepalive 探测器检查远程对等体是否仍处于活动状态且响应迅速，即使没有传输任何数据也是如此。
`**tcp_keepalive_probes**`: a small packet sent by a TCP endpoint to check the health and responsiveness of the remote endpoint in an idle connection. It detects if the remote endpoint has become unreachable or the connection has been lost due to network issues.
tcp_keepalive_probes：TCP端点发送的小数据包，用于检查空闲连接中远程端点的运行状况和响应能力。它检测远程终结点是否变得无法访问或连接是否由于网络问题而丢失。
`tcp_keepalive_intvl`: controls the interval between sending keepalive probes on an idle TCP connection.
tcp_keepalive_intvl：控制在空闲的 TCP 连接上发送 keepalive 探测之间的间隔。

## 如何使 SSH 会话保持活动状态

### 客户端配置 （Linux）Client-Side Configuration (Linux)  

在客户端，即您的 Linux 桌面系统，在您的主目录（如果尚不存在）“~/.ssh/config”中创建一个文件。

```shell
touch ~/.ssh/config
```

但是，如果“~/.ssh”目录不存在，则必须创建它，然后设置适当的权限。

```shell
mkdir ~/.ssh 
chmod 700 ~/.ssh
```

添加一下配置：

```shell
Host *
ServerAliveInterval 120
ServerAliveCountMax 30
```

Here’s what each option means:  
以下是每个选项的含义：

- **Host**: The configurations specified only apply to the hosts listed following the “_Host_” keyword. Because we used a wildcard character (*), they apply to all hosts.  
    主机：指定的配置仅适用于“主机”关键字后面列出的主机。由于我们使用了通配符 （*），因此它们适用于所有主机。
- **ServerAliveInterval**: Sets a timeout interval in seconds, after which, if no data has been received from the server, SSH will send a message through the encrypted channel to request a response from the server. The default is 0, indicating that these messages will not be sent to the server.  
    ServerAliveInterval：设置超时间隔（以秒为单位），超过该间隔后，如果服务器没有收到任何数据，SSH 将通过加密通道发送消息，请求服务器响应。默认值为 0，表示这些消息不会发送到服务器。
- **ServerAliveCountMax**: Sets the number of server alive messages which may be sent without SSH receiving any messages back from the server. If this threshold is reached while server-alive messages are being sent, SSH will disconnect from the server, terminating the session. The default value is 3.  
    ServerAliveCountMax：设置在不通过 SSH 接收从服务器返回的任何消息的情况下可以发送的服务器活动消息数。如果在发送服务器活动消息时达到此阈值，SSH 将断开与服务器的连接，从而终止会话。默认值为 3。

In other words, the client will send a keepalive message to the server every 120 seconds (2 minutes), 30 times. 120 * 30 = 3600 seconds, or one hour. This is the total amount of time for which, even without activity, our SSH session will be kept alive.  
换句话说，客户端将每 120 秒（2 分钟）向服务器发送一条 keepalive 消息，共 30 次。120 * 30 = 3600 秒，即 1 小时。这是即使没有活动，我们的 SSH 会话也将保持活动状态的总时间。




### 服务器端配置
更改服务器的超时选项会影响连接到服务器的所有客户端。您需要编辑“`/etc/ssh/sshd_config`”文件才能执行此操作。  

```shell
sudo vi /etc/ssh/sshd_config
```

然后设置以下三个选项：

```shell
TCPKeepAlive yes
ClientAliveInterval 120 
ClientAliveCountMax 30
```

Here’s what each of these three options means:  
以下是这三个选项中每个选项的含义：

- **TCPKeepAlive**: Specifies whether the system should send TCP keepalive messages to the client.  
    TCPKeepAlive：指定系统是否应向客户端发送 TCP keepalive 消息。
- **ClientAliveInterval**: Sets a timeout interval in seconds, after which, if no data has been received from the client, the SSH server will send a message through the encrypted channel to request a response from the client.  
    ClientAliveInterval：设置超时间隔（以秒为单位），超过该间隔后，如果客户端没有收到任何数据，SSH服务器将通过加密通道发送消息，请求客户端响应。  
    The default is 0, indicating that these messages will not be sent to the client.  
    默认值为 0，表示这些消息不会发送到客户端。
- **ClientAliveCountMax**: Sets the number of client alive messages which may be sent without the SSH server receiving any messages back from the client. If this threshold is reached while client-alive messages are being sent, the SSH server will disconnect the client, terminating the session.  
    ClientAliveCountMax：设置在SSH服务器未从客户端接收任何消息的情况下可以发送的客户端活动消息数。如果在发送客户端活动消息时达到此阈值，SSH 服务器将断开客户端连接，从而终止会话。  
    The default value is 3.  
    默认值为 3。


