# Serviço de Redes

INSTITUTO FEDERAL DE ALAGOAS - CAMPUS ARAPIRACA

ELAINE CLIS DE MENEZES SILVA - 914

BRUNNO MARTINS SANTOS - 914

# Objetivos:

Roteirizar o processo de implementação e configuração da VM com o serviço de compartilhamento de arquivo (SAMBA) e o servidor de resolução de DNS.

# TabelaS de configuração da Rede

- Definições de rede interna

| DESCRIÇÃO  |  IP  |
| ------------------- | ------------------- |
| REDE |  10.9.14.107 |
|  MÁSCARA |  255.255.255.0 |
| VIRTUALBOX | 10.9.14.1 |
|  BROADCAST |  10.9.14.255 |
|  NS1 |  10.9.14.107|
| SAMBA |  10.9.14.107 |


- Definições de rede externa

| DESCRIÇÃO  |  IP  |
| ------------------- | ------------------- |
| REDE |  192.168.14.16 |
|  MÁSCARA |  255.255.255.248 |
|  BROADCAST |  192.168.14.23 |


- Nome dos servidores


| NOME DA VM  |  NOME  |
| ------------------- | ------------------- |
| GATEWAY | gw.elaine_914.laberedes.ifalarapiraca.local |
|  NAMESERVER1 |  ns1.elaine_914.laberedes.ifalarapiraca.local |
| VIRTUALBOX |  samba.elaine_914.laberedes.ifalarapiraca.local |


# Links 

## Configurações iniciais:
[interface e gateway](https://github.com/NanyDesu/trab_sred/tree/main/Initial)
## Configuração Samba:
[Samba](https://github.com/NanyDesu/trab_sred/tree/main/SAMBA)
## Configuração DNS:
[DNS](https://github.com/NanyDesu/trab_sred/tree/main/DNS)
