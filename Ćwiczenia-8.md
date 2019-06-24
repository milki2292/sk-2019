Zadanie 1
---------



1. Zaprojektuj oraz przygotuj prototyp rozwiązania z wykorzystaniem oprogramowania ``VirtualBox`` lub podobnego. 
Zaproponuj rozwiązanie spełniające poniższe wymagania:
   * Usługodawca zapewnia domunikację z siecią internet poprzez interfejs ``eth0`` ``PC0``
   * Zapewnij komunikację z siecią internet na poziomie ``LAN1`` oraz ``LAN2``
   * Dokonaj takiego podziału sieci o adresie ``172.22.128.0/17`` aby w ``LAN1`` można było zaadresować ``500`` adresów natomiast w LAN2 ``5000`` adresów    
   * Przygotuj dokumentację powyższej architektury w formie graficznej w programie ``DIA``
 
* Tworzymy 3 maszyny wirtualne: PC0 - jako router i PC1,PC2 jako urządzenia<br>
* Dzielimy sieci: LAN1 otrzymała adres `172.22.160.0/23` żeby pomieściła 500 adresów<br>
                     LAN2 otrzymała adres `172.22.128.0/19` żeby pomieściła 5000 adresów<br>
* ustalamy adresy PC1 i PC2:<br>
  `ip a add 172.22.160.6/23 dev enp0s3`<br>
  `ip a add 172.22.128.6/19 dev enp0s3`<br>
* ustalamy adresy w PC0:<br>
 ` ip a add 172.22.160.1/23 dev enp0s8`<br>
 ` ip a add 172.22.128.1/19 dev enp0s9`<br>
* podnosimy interfejsy:<br>
 ` ip link set enp0s8 up`<br>
 ` ip link set enp0s9 up`<br>
* Podczas wykonywania zadania pomyliłem się i musiałem pozbyć się błędnie wpisanych adresów:<br>
 ` ip flush`<br>
* Używając powyższej komendy usuną się również adres sieci NAT, przywróciłem ją wpisując następujące polecenia:<br>
  `dhclient -r`<br>
  `dhclient`<br>
 * W pliku `sysctl.conf` trzeba było odkomentować 1 linijkę dotyczącą ipv4<br>
 * Z PC0 pingowało już do PC1 i PC2 ale odwrotnie już nie<br>
 * problemem był nameserver w pliku `/etc/resolv.conf` po zmianie działało.<br>
 * Żeby pingowało z PC1 i PC2 do google trzeba było dodać:<br>
  `ip route add default via 172.22.160.1` (PC1) <br>
  `ip route add default via 172.22.128.1` (PC2)<br>
  `echo 1 > /proc/sys/net/ipv4/ip_forward`(PC0)<br>
  `iptables -t nat -A POSTROUTING -s 172.22.160.0/23 -o enp0s3 -j MASQUERADE`(PC0)<br>
  `iptables -t nat -A POSTROUTING -s 172.22.128.0/19 -o enp0s3 -j MASQUERADE`(PC0)<br>
  ![zadanie 1](65541061_2310417072551858_7093711403621548032_n.png)
  Diagram
  ---------
  ![zadanie 1](diagram.png)
