# xcp-ng-zabbix
How to install Zabbix agent in XCP-NG hypervisor server

## Install the repository

`rpm -ihv http://repo.zabbix.com/zabbix/5.4/rhel/7/x86_64/zabbix-release-5.4-1.el7.noarch.rpm`

## Edit zabbix_agentd.conf

`vi /etc/zabbix/zabbix_agentd.conf`

Here is my example remoded coments and empty lines:

```
[xcp-server01 ~]# grep -v '#\|^$' /etc/zabbix/zabbix_agentd.conf
PidFile=/var/run/zabbix/zabbix_agentd.pid
LogFile=/var/log/zabbix/zabbix_agentd.log
LogFileSize=10
Server=192.168.200.1
ServerActive=192.168.200.1
HostnameItem=system.hostname
HostMetadata=linux
Include=/etc/zabbix/zabbix_agentd.d/*.conf
[xcp-server01 ~]#
```

## Start the service

`service zabbix-agent start`

## Enable auto start at boot

`chkconfig zabbix-agent on`

## Open firewall ports

`vi /etc/sysconfig/iptables`

Add the this lines before REJECT as follows

```
[xcp-server01 ~]# grep -A2 -B2  REJECT /etc/sysconfig/iptables
-A RH-Firewall-1-INPUT -p tcp -m tcp --dport 10050 -j ACCEPT
-A RH-Firewall-1-INPUT -p tcp -m tcp --dport 10051 -j ACCEPT
-A RH-Firewall-1-INPUT -j REJECT --reject-with icmp-host-prohibited
COMMIT
[xcp-server01 ~]#
```

Save and restart the iptables service

`service iptables restart`

I hope it helps you.


Source: http://oinformata.eti.br/wp/instalando-o-zabbix-agent-no-xcp-ng/



