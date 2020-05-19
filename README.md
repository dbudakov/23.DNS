## Домашнее задание  
Настраиваем split-dns  
Цель: В результате выполнения ДЗ студент настроит split-dns.  
взять стенд https://github.com/erlong15/vagrant-bind  
добавить еще один сервер client2  
завести в зоне dns.lab  
имена  
web1 - смотрит на клиент1  
web2 смотрит на клиент2  
  
завести еще одну зону newdns.lab  
завести в ней запись  
www - смотрит на обоих клиентов  
  
настроить split-dns  
клиент1 - видит обе зоны, но в зоне dns.lab только web1  
  
клиент2 видит только dns.lab  
  
\* настроить все без выключения selinux  
Критерии оценки: 4 - основное задание сделано, но есть вопросы  
5 - сделано основное задание  
6 - выполнено задания со звездочкой  
  
## Решение    
  
В файл `/etc/named.conf` добавлены директивы `view`, для разделения рeзолвинга имён для `client1` и `client2`, добалена зона `newdns.lab.` и настроен SplitDNS для client'ов через DNS-имя `www.newdns.lab.`   
Чтобы проверить стенд необходимо зайти на `client1` и выполнить следующие команды, чтобы убедиться что в зоне dns.lab. для него доступет `web1.dns.lab.`, но не доступен `web2`.     
А также резолвится имя www.newdns.lab..  
```  
dig @192.168.50.10 www.newdns.lab.  
dig @192.168.50.10 web1.dns.lab.  
dig @192.168.50.10 web2.dns.lab. # информация должна быть недоступна   
```  
  
На втором клиенте полностью доступен сегмент `dns.lab.` но доступа к зоне `newdns.lab.` нет  
```  
dig @192.168.50.10 web1.dns.lab.  
dig @192.168.50.10 web2.dns.lab.   
dig @192.168.50.10 www.newdns.lab. # информация должна быть недоступна  
```

аудит файла /etc/resolvconf [auditctl] (https://1cloud.ru/help/security/audit-linux-c-pomoshju-auditd)  
запись resolv.conf debian[rdnssd](https://linux.die.net/man/8/rdnssd)  
синхронизация времени [ntpdate](https://serveradmin.ru/ustanovka-nastroyka-i-sinhronizatsiya-vremeni-v-centos/)  
