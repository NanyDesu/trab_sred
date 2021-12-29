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
[ifconfig](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt2_ifconfig.PNG)
