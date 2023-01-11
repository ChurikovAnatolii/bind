# DNS Lab

## Описание выполнения задания

***Задание выполнено с помощью Vagrantfile и Ansible plabook. Для запуска стенда нужно выполнить команду Vagrant up.***
<br>
<br>

**1. В vagrantfile добавлен сервер client2**
<br>
<br>

**2. В зоне dns.lab добавлены**

web1            IN      A       192.168.50.15  
web2            IN      A       192.168.50.16

<br>

**3. В папке provision добавлена зона newdns.lab в ней добавлен запись**

www            IN      A       192.168.50.15  
www            IN      A       192.168.50.16

**4. Настроен split-dns: client видит обе зоны, в зоне dns.lab только web1. Client2 видит только зону dns.lab**
<br>
<br>

**Проверим работу стенда: для этого подключимся на клиентов и попингуем разные адреса**
---

```console
[vagrant@client ~]$ ping www.newdns.lab
PING www.newdns.lab (192.168.50.15) 56(84) bytes of data.
64 bytes from client (192.168.50.15): icmp_seq=1 ttl=64 time=0.046 ms
64 bytes from client (192.168.50.15): icmp_seq=2 ttl=64 time=0.062 ms
^C
--- www.newdns.lab ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.046/0.054/0.062/0.008 ms
[vagrant@client ~]$ ping web1.dns.lab
PING web1.dns.lab (192.168.50.15) 56(84) bytes of data.
64 bytes from client (192.168.50.15): icmp_seq=1 ttl=64 time=0.043 ms
64 bytes from client (192.168.50.15): icmp_seq=2 ttl=64 time=0.044 ms
^C
--- web1.dns.lab ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.043/0.043/0.044/0.006 ms
[vagrant@client ~]$ ping web2.dns.lab
ping: web2.dns.lab: Name or service not known
```
**С client пингуем www.newdns.lab, web1.dns.lab. web2.dns.lab не пингуем.**  
---
<br>

**Проверим работу с client2**
```console
[vagrant@client2 ~]$ ping www.newdns.lab
ping: www.newdns.lab: Name or service not known
[vagrant@client2 ~]$ ping web1.dns.lab
PING web1.dns.lab (192.168.50.15) 56(84) bytes of data.
64 bytes from 192.168.50.15 (192.168.50.15): icmp_seq=1 ttl=64 time=1.29 ms
64 bytes from 192.168.50.15 (192.168.50.15): icmp_seq=2 ttl=64 time=0.852 ms
^C
--- web1.dns.lab ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.852/1.075/1.299/0.225 ms
[vagrant@client2 ~]$ ping web2.dns.lab
PING web2.dns.lab (192.168.50.16) 56(84) bytes of data.
64 bytes from client2 (192.168.50.16): icmp_seq=1 ttl=64 time=0.047 ms
64 bytes from client2 (192.168.50.16): icmp_seq=2 ttl=64 time=0.145 ms
^C
--- web2.dns.lab ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 0.047/0.096/0.145/0.049 ms
[vagrant@client2 ~]$ ping www.newdns.lab
```
**С client2 не пингуем www.newdns.lab, web1.dns.lab. web2.dns.lab пингуем. Вывод: задание выполнено в полном объеме.**  
---