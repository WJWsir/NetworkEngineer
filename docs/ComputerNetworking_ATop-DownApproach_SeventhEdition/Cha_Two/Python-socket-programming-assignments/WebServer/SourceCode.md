---
layout: post
author: WEI LIN
---

# 目录结构

```
/
├── WebServer.py
├── Colors.html
└── 404.html
```

## 一、WebServer.py 源代码

```python
# Skeleton Python Code for the Web Server and Implement

#import socket module

from socket import *#Import Socket library for network programming 导入包括网络通信程序组件的Socket库或模块
import sys #In order to terminate the program

serverSocket = socket(AF_INET, SOCK_STREAM)
#Prepare a server socket

# Fill in start
#必要的2个程序步骤，绑定服务器端口与指定最大请求连接数
serverPort = 6789
serverSocket.bind(('', serverPort))#bind port 6789 with server socket
serverSocket.listen(1)#server socket starts waiting and listening. it can only accept one connection request
# Fill in end

while True:
    #Establish the connection
    print('Ready to serve...')
    connectionSocket, addr = serverSocket.accept()# Fill in start  # Fill in end
    try:
        message = connectionSocket.recv(1024).decode()# Fill in start  # Fill in end
        filename = message.split()[1]
        f = open(filename[1:], encoding='utf8')
        outputdata = f.read() # Fill in start  # Fill in end

        # Fill in start
        #Send one HTTP status line into socket 发送HTTP报文状态行
        statusline = 'HTTP/1.1 200 OK'
        connectionSocket.send(statusline.encode())
        connectionSocket.send("\r\n".encode())

        #Send one or more HTTP header line into socket 发送HTTP报文首部行
        field_ContentType = "Content-Type:" + " " + "text/html" #告知浏览器或HTTP响应报文接收方实体体中的对象是HTML文本
        connectionSocket.send(field_ContentType.encode())
        connectionSocket.send("\r\n".encode())

        #Send blank line
        connectionSocket.send("\r\n".encode()) 发送空行
        # Fill in end

        #Send the content of the requested file to the client 发送HTTP报文实体体或消息体
        for i in range(0, len(outputdata)):
            connectionSocket.send(outputdata[i].encode())
        connectionSocket.send("\r\n".encode())

        #Close connection socket 关闭连接套接字
        connectionSocket.close()
    except IOError:
        print('IOError')
        #Send response message for file not found
        # Fill in start
        statusline = 'HTTP/1.1 404 Not Found'
        connectionSocket.send(statusline.encode())
        connectionSocket.send("\r\n".encode())

        field_Connection = "Connection:" + " " + "Close"
        connectionSocket.send(field_Connection.encode())
        connectionSocket.send("\r\n".encode())
        field_ContentType = "Content-Type:" + " " + "text/html"
        connectionSocket.send(field_ContentType.encode())
        connectionSocket.send("\r\n".encode())

        connectionSocket.send("\r\n".encode())

        f = open('404.html', encoding='utf8')
        outputdata = f.read()#Fill in start  # Fill in end
        print(outputdata)
        for i in range(0, len(outputdata)):
            connectionSocket.send(outputdata[i].encode())
        connectionSocket.send("\r\n".encode())
        # Fill in end

        #Close client socket
        #Fill in start
        connectionSocket.close()
        break
        #Fill in end
serverSocket.close()
sys.exit()#Terminate the program after sending the corresponding data
```

## 二、Colors.html 源代码

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>div重叠</title>
    <style>
      .div-relative {
        position: relative;
        color: #000;
        border: 1px solid #000;
        width: 500px;
        height: 400px;
      }
      .div-a {
        position: absolute;
        left: 30px;
        top: 30px;
        background: #f00;
        width: 200px;
        height: 100px;
      }
      /* css注释说明： 背景为红色 */
      .div-b {
        position: absolute;
        left: 50px;
        top: 60px;
        background: #ff0;
        width: 400px;
        height: 200px;
      }
      /* 背景为黄色 */
      .div-c {
        position: absolute;
        left: 80px;
        top: 80px;
        background: #00f;
        width: 300px;
        height: 300px;
      }
      /* DIV背景颜色为蓝色 */
    </style>
  </head>
  <body>
    <div class="div-relative">
      <div class="div-a">我背景为红色</div>
      <div class="div-b">我背景为黄色</div>
      <div class="div-c">我背景为蓝色</div>
    </div>
  </body>
</html>
```

## 三、404.html 源代码

取自百度网站

```html
<!DOCTYPE html>
<html>
<head>
    <title>404 Not Found</title>
</head>

<body>
    <h1>Not Found</h1>
    <p>The requested URL was not found on this server.</p>
</body>
</html>
```

## [问题]({{ site.siteurl }}/docs/ComputerNetworking_ATop-DownApproach_SeventhEdition/Cha_Two/Python-socket-programming-assignments/WebServer/Problem.html)