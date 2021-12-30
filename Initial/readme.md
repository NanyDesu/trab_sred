# Configuração de interface e gateway

## DNS (Servidor)

- Iremos utilizar o DNS para nos servir nomes de modo que o mesmo será responsável por converter o nosso ip para um nameserver

```bash
 sudo nano /etc/netplan/00-installer-config.yaml
```
(editar arquivo installer-config.yaml)

```
network:
    ethernets:
        ens160:                           # nome da interface que está sendo configurada. Verifique com o comando 'ifconfig -a'
            addresses: [10.9.14.126/24]   # IP e Máscara do Host. Aqui é só um exemplo, tenha certeza do IP do seu host, ou perderá o acesso remoto.
            gateway4: 10.9.14.1           # IP do Gateway, Aqui é só um exemplo, tenha certeza do IP do seu gateway, ou perderá o acesso remoto.
            dhcp4: false                  # dhcp4 false -> cliente DHCP está desabilitado, logo o utilizará o IP do campo 'addresses'
            nameservers:
                addresses:
                   - 8.8.8.8              # IP do servidor de nomes 1, neste caso é o IP de DNS do google
                   - 8.8.4.4              # IP do servidor de nomes 2, neste caso é outro IP de DNS do google
                search: []                # identificação do domínio, aqui neste caso está vazio.
        ens192:                           # nome da interface que está sendo configurada. Verifique com o comando 'ifconfig -a'
            addresses: [192.168.0.201/25] # IP e Máscara de interface externa.
    version: 2
```

(configuração de interface)

![sudo nano /etc/netplan/00-installer-config.yaml](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt1_installer_config.PNG)


```bash
 sudo netplan apply
```
(aplicando configurações)

```bash
 ifconfig -a
```
(configuração da interface)



![ifconfig](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt2_ifconfig.PNG)


## Gateway


- Iremos configurar o servidor gateway como NAT
- Habilitar e configurar

#### Iniciando:

```bash
 sudo ufw enable
```

(habilitar o firewall)


```bash
 sudo ufw allow ssh
```

(permitir acesso ssh)

![ufw eneble & allow ssh](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt3_ufw_nable.PNG)


```bash
 sudo nano /etc/ufw/sysctl.conf
``` 

(encaminhamento de pacotes de wan para lan)


```bash
...
net/ipv4/ip_forwarding=1
...
```

(remoção da marca de comentário)


[/etc/ufw/sysctl.conf](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt4_sysctl_conf.PNG)


```bash
 ifconfig -a
```

```bash
WAN interface: ens160
LAN interface: ens192
```

(configurar nome da interface interna e externa)


![ifconfig -a](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt2_ifconfig.PNG)


```bash
 sudo nano /etc/rc.local
```

```bash
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

(Recriação necessária do arquivo /etc/rc.local)




![rclocal](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt5_rc_local.PNG)


```bash
 sudo chmod 755 /etc/rc.local
```


(tornando o arquivo executável)


```bash
 sudo ufw status
```

ou 

```bash
 systemctl status ufw.service
```

(verificando a atividade do firewall)


```bash
 sudo reboot
```

(reiniciando a máquina)


![chmod-reboot](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt6_chmod_reboot.PNG)





        
```bash
iptables -A PREROUTING -t nat -i ens192 -p tcp –-dport 445 -j DNAT –-to 10.9.14.100:445
iptables -A FORWARD -p tcp -d 10.9.14.100 –-dport 445 -j ACCEPT
#Recebe pacotes na porta 139 da interface externa do gw e encaminha para o servidor interno na porta 139
iptables -A PREROUTING -t nat -i ens192 -p tcp –-dport 139 -j DNAT –-to 10.9.14.100:139
iptables -A FORWARD -p tcp -d 10.9.14.100 –-dport 445 -j ACCEPT
```


(Recebe de pacotes na porta 445 da interface externa do gw e encaminha para o servidor interno na porta 445)




```bash
iptables -A PREROUTING -t nat -i ens160 -p tcp –-dport 53 -j DNAT –-to 10.9.14.126:53
iptables -A FORWARD -p udp -d 10.9.14.126 –-dport 53 -j ACCEPT
```


(Recebe pacotes na porta 53 da interface externa do gw e encaminha para o servidor DNS Master interno na porta 53)





### [Configuração SAMBA](https://github.com/NanyDesu/trab_sred/tree/main/SAMBA)








