---
layout: post
author: WEI LIN
---

# 源代码

## 客户端程序 UDPPingerServer.py
```python
from socket import *
import time

# Generate a string of 32 bytes after utf8 encoding
message = 'A' * 32

# specify Server host name and Port
serverName = 'localhost' # "127.0.0.1"
serverPort = 12000
# Create client socket
clientSocket = socket(AF_INET, SOCK_DGRAM)
# Set a timeout on blocking socket operations - one second
clientSocket.settimeout(1.0)

# The client send 10 pings to the server
print('正在 Ping '+ serverName + '[' + gethostbyname(serverName) + ']' + ' 具有 32 字节的数据:')
for i in range(0, 10):
    sequence_number = i+1
    send_time = time.time()# Record request message sended time
    message = 'Ping' + ' ' + "{0:0>2}".format(str(sequence_number)) + ' ' + str(time.asctime(time.localtime(send_time)))
    clientSocket.sendto(message.encode(), (serverName, serverPort))#(1) send the ping message using UDP

    try:
        responseMessage, serverAddress = clientSocket.recvfrom(1024)
        receive_time = time.time()# Record response message received time
    except timeout:
        print('Request timed out')#(4)otherwise, print "Request timed out"
        continue
    if(len(responseMessage) > 0):
        print('From' + ' ' + serverAddress[0] + ' ' + '\'s response: ', end="")
        print('Bytes=' + str(len(responseMessage)) + ' ', end="")
        print('Message=' + responseMessage.decode() + ' ', end="")#(2)print the response message from server, if any
        print('RTT=' + str(receive_time - send_time) + 'seconds', end="")#(3)calculate and print the round trip time(RTT), in seconds, of each packet, if server responses
        print()
print('')
clientSocket.close()
```


## 服务端程序 UDPPingerServer.py
```python
# We will need the following module to generate randomized lost packets
import random
from socket import *

#Create a UDP socket
#Notice the use of SOCK_DGRAM for UDP packets
serverSocket = socket(AF_INET, SOCK_DGRAM)
#Assign IP address and port number to socket
serverSocket.bind(('', 12000))

while True:
    # Generate random number in the range of 0 to 10
    rand = random.randint(0, 10)
    # Receive the client packet along with the address it is coming from
    message, address = serverSocket.recvfrom(1024)
    # Capitalize the message frmo the client
    message = message.upper()
    # if rand is less than 4, we consider the packet lost and do not respond
    if rand < 4:
        continue
    # Otherwise, the server responds
    serverSocket.sendto(message, address)
```

## [问题]({{ site.siteurl }}/docs/ComputerNetworking_ATop-DownApproach_SeventhEdition/Cha_Two/Python-socket-programming-assignments/UDPPinger/Problem.html)
