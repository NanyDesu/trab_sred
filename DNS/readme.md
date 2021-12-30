# Configuração do DNS
O DNS é um sistema que realiza a conversão dos domínios de sites para IPs que serão legíveis pelos computadores, para que a identificação do servidor final seja controlada para que seja possível obter a conexão com ele, sendo necessário configurá-lo para posterior uso.

## Como configurar o DNS
O firewall foi habilitado usando o comando `$ sudo ufw enable` e o acesso SSH foi permitido com o comando `$ sudo ufw allow ssh`.

O comando `$ nano /etc/netplan/00-installer-config.yaml` foi utilizado para permitir a adição de linhas de configuração do IP, como ilustra o resultado na imagem:

![image](https://user-images.githubusercontent.com/28654388/147712512-3d76d193-0d25-4556-b897-a8be985c8ad0.png)

O arquivo foi criado, e foram usados os comandos `$ sudo netplan apply` e `$ ifconfig -a` para verificar o estado da configuração.

Posteriormente o arquivo rc.local foi recriado usando o comando  `$ sudo nano /etc/rc.local` e o seguinte script adicionado a ele:
```
#!/bin/bash

# /etc/rc.local

# Default policy to drop all incoming packets.
# Politica padrão para bloquear (drop) todos os pacotes de entrada
iptables -P INPUT DROP
iptables -P FORWARD DROP

# Accept incoming packets from localhost and the LAN interface.
# Aceita pacotes de entrada a partir das interfaces localhost e the LAN.
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -i enp0s8 -j ACCEPT

# Accept incoming packets from the WAN if the router initiated the connection.
# Aceita pacotes de entrada a partir da WAN se o roteador iniciou a conexao
iptables -A INPUT -i enp0s3 -m conntrack \
--ctstate ESTABLISHED,RELATED -j ACCEPT

# Forward LAN packets to the WAN.
# Encaminha os pacotes da LAN para a WAN
iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT

# Forward WAN packets to the LAN if the LAN initiated the connection.
# Encaminha os pacotes WAN para a LAN se a LAN inicar a conexao.
iptables -A FORWARD -i enp0s3 -o enp0s8 -m conntrack \
--ctstate ESTABLISHED,RELATED -j ACCEPT

# NAT traffic going out the WAN interface.
# Trafego NAT sai pela interface WAN
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

# rc.local needs to exit with 0
# rc.local precisa sair com 0
exit 0
```

O nome da interface foi conferida, como mostra a imagem:

![image](https://user-images.githubusercontent.com/28654388/147712548-4c8e6a76-bc58-4f01-b424-2db81ce25d5d.png)

O comando `$ sudo chmod 755 /etc/rc.local` foi usado para torná-lo iniciável pelo boot, foi conferido se o firewall estava operante com o comando `$ sudo ufw status` e a máquina foi reiniciada com  `$ sudo reboot`.

Após, iniciamos a [configuração do Samba](https://github.com/NanyDesu/trab_sred/blob/main/SAMBA/readme.md).
