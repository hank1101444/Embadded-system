```python
#! /usr/bin/env python3
# _*_ coding: utf-8 _*_
​
from socket import *
​
# 变量名大写，约定作为配置项的意思，这里定义了三个配置变量作为实例化的参数
# 定义IP地址、端口、缓存，缓存的单位是Byte
IP = '127.0.0.1'
PORT = 60000
BUFLEN = 1024
​
# 实例化一个socket赋值给变量listen_socket
# 参数AF_INET就是IP地址族
# 参数SOCK_STREAM就是TCP、参数SOCK_DGRAM就是UDP、参数SOCK_RAW是原始套接字，为TCP、UDP之外的协议提供接口
listen_socket = socket(AF_INET, SOCK_STREAM)
​
# 为这个socket绑定IP和端口，bind的对象必须是一个包含IP和端口元组
listen_socket.bind((IP, PORT))
​
# 使socket处于监听状态，等待客户端的请求，只有服务端能使用监听
# 参数5表示，最多接收5个等待连接的客户端
listen_socket.listen(5)
print(f'Server: 服务启动成功，在{PORT}端口，等待 Client 连接... ...')
​
# 监听socket的accept方法，用来接收客户端的连接，如果没有客户端连接，就一直处于监听状态（阻塞状态）直到有客户端连接，一旦客户段发起连接（TCP三次握手）accept方法就会返回一个元组，一个data_socket来传输数据，一个ipaddress包含IP和PORT
data_socket, ip_port = listen_socket.accept()
print(f'Server>>> 接受一个客户端{ip_port}连接... ...')
​
while True:
   # 尝试读取客户端发来的消息
   # BUFLEN指定从接收缓冲里最多读取多少Bytes，recved是一个字节串变量
   recved = data_socket.recv(BUFLEN)

   # 如果返回空的Bytes，意味着对方关闭了连接
   # 退出循环，结束消息收发
   if not recved:
       break
   
   # 把读取的Bytes数据，转换成字符串编码，赋值给变量info，然后打印出来
   info = recved.decode('utf-8')
   print(f'Server>>> 收到客户端{ip_port}消息: {info}')
   
   # 通知客户端的字符串，编码为utf-8的Bytes类型数据
   data_socket.send(f'Server>>> 服务器{(IP, PORT)}收到消息 {info}'.encode('utf-8'))
​
data_socket.close()
listen_socket.close()
```
