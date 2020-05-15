Суть процесса получения адреса по DHCP описана в RFC 2131([Request for Comments: 2131 ](https://tools.ietf.org/html/rfc2131))  
DHCP работает только в пределах действия широковещательного домена,если есть ограничения физического сегмента или VLAN'а, то и работать DHCP будет в пределах этого VLAN'a.  
Итак первый запрос 'DHCP Discover' идет с неназначеного адреса на броадкастовый адрес(255.255.255.255).Cуть первого запроса в том чтобы клиент обозначил себя и запрос на получения адреса. В ответ DHCP сервер делает широковещательную рассылку 'DHCP Offer(Unicast/Broadcast)' броадкастом(так как адреса у источника запроса нет) и предлагает использовать конкретный адрес. Далее клиент в случае отсутствия ограничений, отправляет широковещательной(broadcast) запрос на использование указанного(ранее предложенного) адреса 'DHCP Request(Broadcast)'. На что сервер подтверждает использование этого адреса(DHCP Ack(Unicast/Broadcast))  ![](https://github.com/dbudakov/23.DNS/blob/master/images/DHCP/DHCP.png)  

![](https://github.com/dbudakov/23.DNS/blob/master/images/DHCP/DHCP_relay_2.png)
![](https://github.com/dbudakov/23.DNS/blob/master/images/DHCP/DHCP_relay_3.png)
