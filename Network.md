# tcpdump

$ sudo tcpdump -i any udp port 23001
-n 显示IP地址
-xx -tt 十六进制打印报文
$ sudo tcpdump -i any src 192.168.12.32 and port 33011 -n -vv -X

抓 em3接口上，来往于192.168.12.33:33005，并且ip包长度大于55的包。使用高精度时钟。
$ tcpdump -n -i em3 -vv -X host 192.168.12.33 and port 33005 and 'ip[2:2] > 55' -j host_hiprec
$ tcpdump -n -i em3 -vv -X host 192.168.12.33 and port 33005 and greater 55
- https://linuxwiki.github.io/NetTools/tcpdump.html

## tcpdump write to file permission denied
the file suffix must be ".pcap"

# tsocks
$ ssh -qTfnN -D 127.0.0.1:11111 test1@192.168.0.221

# ethtool
## Cannot get device ring settings: Operation not supported
The driver doesn't support the operation

$ ethtool -g eth0
$ ethtool -i eth0

# route
$ route add -net 10.81.1.104/29 gw 192.168.0.11
$ route add -net 239.100.0.0/24 dev eth0

## /etc/network/interfaces
up route add -net 192.168.0.0 netmask 255.255.0.0 gw 192.168.1.1
up route add -net 192.168.10.0 netmask 255.255.255.0 dev enp2s0

- https://askubuntu.com/questions/168033/how-to-set-static-routes-in-ubuntu-server
- http://www.configserverfirewall.com/ubuntu-linux/add-permanent-static-route-ubuntu/

# nc
## keep listening on a port 
$ nc -lk 12345

## specify a source port 
$ nc 192.168.0.19 5000 -p 5001

## donot send data
$ nc -z 192.168.0.19 5000

## set time out
$ nc -w 3 192.168.0.19 5000

- https://www.computerhope.com/unix/nc.htm
- https://unix.stackexchange.com/questions/332163/netcat-send-text-to-echo-service-read-reply-then-exit

## bind failed: Address already in use
use ncat or nmap instead.
$ ncat -w 3 192.168.0.19 5000
$ sudo nmap -sS --source-port 5002 -p 5001 192.168.0.1

$ sudo nmap -sS --source-port 8140 -p 9090-9093 10.X.X.9

- https://unix.stackexchange.com/questions/121230/nc-bind-failed-address-already-in-use
- https://unix.stackexchange.com/questions/121230/nc-bind-failed-address-already-in-use
- http://man7.org/linux/man-pages/man1/ncat.1.html

# ping 
The time output is Round Trip Time(RTT).

# strace ping -c 1 192.168.0.1 # use root to execute.
- https://blog.csdn.net/kwu_ganymede/article/details/50942881
- https://www.cnblogs.com/shengulong/p/7545560.html

# tcpping
$ tcpping 192.168.223.20 22 # probe for the ssh port

# nmap

- https://www.cyberciti.biz/security/nmap-command-examples-tutorials/

# iftop
$ iftop -i p4p1 -B -b -t -n
$ iftop -i p4p1 -B -b -t -n -f 'port 33005 and host 192.168.12.38' -P
$ iftop -i p4p1 -B -b -t -n -f 'tcp port 33005 and ( dst 192.168.12.38 or src 192.168.12.38)' -P

- http://www.draconyx.net/articles/iftop-finding-traffic-hogs.html
- man pcap-filter

# traceroute
$ traceroute 192.168.223.20
$ traceroute 192.168.223.20 -T -p 22 # use TCP to do the job, and probe for dest port 22, need sudo right on ubuntu. For simplicity we can use tcptraceroute, no need to be root.
$ tcptraceroute 192.168.223.20 -p 22

# Scapy
- https://scapy.readthedocs.io/en/latest/usage.html

# lspci
## lspci -vvv

# ethtool
## ethtool -i eth1 # 查看网卡信息，包含网络驱动

# locate
## locate -4 igb.ko

# modinfo
## modinfo /lib/modules/4.4.0-104-generic/kernel/drivers/net/ethernet/intel/igb/igb.ko
- https://blog.51cto.com/buguoruci/1621335

