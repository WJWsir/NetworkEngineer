---
layout: post
author: WEI LIN
---

# 源代码

## 用户代理(User Agent)程序 SMTPClient.py
```python
from socket import *
import base64 # Use for Tencent Mail Server Authentication

msg = "\r\n I love computer networks!"
endmsg = "\r\n.\r\n"

# Choose a mail server (e.g. Google mail server) and call it mailserver
mailserver = 'smtp.exmail.qq.com'# Tencent mail server: 'smtp.qq.com' for Tencent QQ mail  or 'smtp.exmail.qq.com' for Tencent Corporate Mail

# Create socket called clientSocket and establish a TCP connection with mailserver
#Fill in start
clientSocket = socket(AF_INET,SOCK_STREAM)
clientSocket.connect((mailserver, 25))
#Fill in end
recv = clientSocket.recv(1024).decode()
print(recv)
if recv[:3] != '220':
    print('220 reply not received from server.')

# Send HELO command and print server response.
heloCommand = 'HELO Alice\r\n'
clientSocket.send(heloCommand.encode())
recv1 = clientSocket.recv(1024).decode()
print(recv1)
if recv1[:3] != '250':
    print('250 reply not received from server.')

# Send AUTH LOGIN command to Authentication Login and print server response
authloginCommand = 'AUTH LOGIN\r\n'
clientSocket.send(authloginCommand.encode())
recv2 = clientSocket.recv(1024).decode()
print(recv2)
if recv2[:3] != '334':
    print('334 reply not received from server.')

# Send Username and Password to login
username = input('username: ') # tencent company sender mail name
base64username = base64.encodebytes(username.encode('utf8')).decode()
print(base64username)
password = input('password: ')
base64password = base64.encodebytes(password.encode('utf8')).decode()
print(base64password)

clientSocket.send((base64username + '\r\n').encode())
recv3 = clientSocket.recv(1024).decode()
print(recv3)
if recv3[:3] != '334':
    print('334 reply not received from server.')
clientSocket.send((base64password + '\r\n').encode())
recv4 = clientSocket.recv(1024).decode()
print(recv4)
if recv4[:3] != '235':
    print('235 reply not received from server.')

# Send Mail FROM command and print server response.
#Fill in start
mailfromCommand = 'MAIL FROM: ' + '<' + username + '>' + '\r\n'
clientSocket.send(mailfromCommand.encode())
recv5 = clientSocket.recv(1024).decode()
print(recv5)
if recv5[:3] != '250':
    print('250 reply not received from server.')
#Fill in end

# Send RCPT TO command and print server response.
#Fill in start
recipientMailName = input('recipient mail name: ') # get recipient mail name
rcpttoCommand = 'RCPT TO: ' + '<' + recipientMailName + '>' + '\r\n'
clientSocket.send(rcpttoCommand.encode())
recv6 = clientSocket.recv(1024).decode()
print(recv6)
if recv6[:3] != '250':
    print('250 reply not received from server.')
#Fill in end

# Send message data.
#Fill in start
dataCommand = 'DATA' + '\r\n'
clientSocket.send(dataCommand.encode())
recv7 = clientSocket.recv(1024).decode()
print(recv7)
if recv7[:3] != '354':
    print('354 reply not received from server.')

# 1. head lines
messege = 'From: ' + username + '\r\n'
messege += 'To: ' + recipientMailName + '\r\n'
subject = input('Subject: ')
messege += 'Subject: ' + subject + '\r\n'
clientSocket.send(messege.encode())
# 2. blank line
clientSocket.send('\r\n'.encode())
# 3. entity body
clientSocket.send(msg.encode())
#Fill in end

# Message ends with a single period.
#Fill in start
clientSocket.send(endmsg.encode())
#Fill in end

# Send QUIT command and get server response.
#Fill in start
quitCommand = 'QUIT\r\n'
clientSocket.send(quitCommand.encode())
recv8 = clientSocket.recv(1024).decode()
print(recv8)
#Fill in end

clientSocket.close()
```

## [问题]({{ site.siteurl }}/docs/ComputerNetworking_ATop-DownApproach_SeventhEdition/Cha_Two/Python-socket-programming-assignments/SMTP/Problem.html)