# README #

### OpenVPN with certificate auth ###

* Version 14.07.01

### How do I get set up? ###

* Server side

* First step - update system:
  `apt-get update && apt-get upgrade`

* After install OpenVPN:
  `apt-get install openvpn`

* Then
  `cd /usr/share/doc/openvpn/examples/easy-rsa/2.0/`
  `nano ./vars`

* Initialize enviroment 
  `. ./vars`
  `. ./clean-all`

* Build Certificate
  `./build-ca # ROOT certificate`
  `./build-key-server server # server certificate`
  `./build-dh # Diffie-Hellman key`

* If you want use tls-auth, to protect hide port/service, protect from DDoS and some more
  `openvpn --genkey --secret ./keys/ta.key`

* After enable tls-auth don't forget give tls key to clients

* Copy certificates to work directory
  `cp ./keys/ca.crt /etc/openvpn`
  `cp ./keys/server.crt /etc/openvpn`
  `cp ./keys/server.key /etc/openvpn`
  `cp ./keys/dh1024.pem /etc/openvpn`

* For tls-auth copy tls key
  `cp ./keys/ta.key /etc/openvpn`

* Create user certificate, dont forget enter certificate passwork to protect user key
  `./build-key-pkcs12 vpn.android`
  `./build-key-pkcs12 vpn.windows`
  `./build-key-pkcs12 vpn.debian`
  `./build-key-pkcs12 vpn.ddwrt`
  `./build-key-pkcs12 vpn.home`

* Copy user certificate from directory /usr/share/doc/openvpn/examples/easy-rsa/2.0/keys/

* If you want create more certificate later, just connect to server
  `cd /usr/share/doc/openvpn/examples/easy-rsa/2.0/`
  `. ./vars`
  `./build-key-pkcs12 vpn.newuser1`
  `./build-key-pkcs12 vpn.newuser2`

* Server config 
  `nano /etc/openvpn/server.conf`

* Edit 
  `nano /etc/rc.local`
 for create permanent firewall rules

* Edit
  `nano /etc/sysctl.conf`
 for enable ip forwarding

* Reboot server to prove all works

* Client side settings:

* Copy certificate

* Install to Android device https://play.google.com/store/apps/details?id=de.blinkt.openvpn

* Start application

* VPN Profiles > Add enter connection name

* Basic > Server Address IP address or domain name server

* Type: PKCS12 File

* Select: select our certificate *.p12

* PKCS12 Password: input our certificate password

* remote-cert-tls server switch on this option (at newest version enabled by default)

* If need enable tls-auth

* Use OpenVPN client for Linux or Windows with myvpnconfig.ovpn config file.

* For start without GUI use start script start_vpn.run

### А кому можно доверять в этом мире? ###

Инь Фу Во два дня настраивал VPN-туннель для своего персонального компьютера. Когда туннель заработал, Инь уселся, почтительно повернувшись лицом к югу, и стал читать свою френдленту.

– О, Учитель, – спросил его Сисадмин, – я не могу понять, зачем вам VPN?

– Ты разве не знаешь, что в VPN-туннеле весь трафик шифруется? – удивился Инь.

– Знаю. Но ваш туннель терминируется на обычном сервере в стране западных варваров. А далее весь ваш яшмовый трафик идёт по Сети в открытом виде.

– Сети нет дела до моего трафика, чего не скажешь о провайдере, – ответил Учитель. Видя, что Сисадмин не понял, он добавил. – Вот, например, ты доверил свои деньги банку.

Сисадмин кивнул.

– Но ты не можешь доверить все свои деньги собственной супруге, – продолжал мудрый Инь. – Почему? Потому что она может посчитать эти деньги своими. А с банком такого не случится.

Просветлённый Сисадмин ушёл поднимать себе VPN-туннель.

### Who do I talk to? ###

* Repo owner CyberManiac
