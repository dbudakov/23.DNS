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
Был ряд действий по изменения, playbook и named.conf. В playbook добавлено правило включения параметра `named_write_master_zones` в `SELinux` для прав записи демона `named`. Также переписамы маршруты для зон из `/etc/named` в `/var/named/zones` и проставлены соотвертствующие изменения контекста каталога `/var/named/zones/`. В начале `playbook` для всех хостов настроена сихнронизация времи, через `ntpdate`. Включена защита от перезаписи на файл `/etc/resol.conf` и собственно для демонстрации SELinux, настройка зон производится с клиентов, через `nsupdate`. Файлы `named.conf` переписаны с использованием `view`.     
Для проверки стенда нужно с каждого клиента проверить вводные. Это резолв для `client1` имен `web1.dns.lab` и `www.newdns.lab`  
```
dig @192.168.50.10 web1.dns.lab
dig @192.168.50.10 www.newdns.lab
```
И резолв `web1.dns.lab` и `web2.dns.lab` от `client2`  
```
dig @192.168.50.10 web1.dns.lab
dig @192.168.50.10 web2.dns.lab
```
остальные имена за исключением `ns01` и `ns02` должны быть недоступны  


#### Дополнительная информация:
аудит файла /etc/resolvconf [auditctl] (https://1cloud.ru/help/security/audit-linux-c-pomoshju-auditd)  
запись resolv.conf debian[rdnssd](https://linux.die.net/man/8/rdnssd)  
синхронизация времени [ntpdate](https://serveradmin.ru/ustanovka-nastroyka-i-sinhronizatsiya-vremeni-v-centos/), [ntpdate_v2](https://serveradmin.ru/ntpdate-pool-ntp-org/)  
[SOA](http://www.bog.pp.ru/work/bind.html)  

для allow-transfer и allow-update обычно используют разные ключи (один для обмена данными между серверами, а другой для разрешения изменять зоны и клинетов, dhcp-севреров и пр.)
А ответ  на вопрос,  здесь: https://kb.isc.org/docs/aa-00296
  
rndc.conf](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/4/html/reference_guide/s2-bind-rndc-configuration-rndcconf) и [man rndc](https://linux.die.net/man/8/rndc)  
В работе используется автоприветствие текстом:  
```motd - сокращение от message of the day; содержимое этого файла вам показывается при входе в систему. Иногда это удобно. В данной работе он используется чтобы```

