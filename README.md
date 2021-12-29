# Serviço de Redes

INSTITUTO FEDERAL DE ALAGOAS - CAMPUS ARAPIRACA

ELAINE CLIS DE MENEZES SILVA - 914
BRUNO MARTINS - 914

# Objetivos:

Roteirizar o processo de implementação e configuração da VM com o serviço de commpartilhamento de arquivo (SAMBA) e o servidor de resolução de DNS.

# TabelaS de configuração da Rede

- Definições de rede interna

| DESCRIÇÃO  |  IP  |
| ------------------- | ------------------- |
| REDE |  10.9.14.107 |
|  MÁSCARA |  255.255.255.0 |
| VIRTUALBOX | 10.9.14.1 |
|  BROADCAST |  10.9.14.255 |
|  NS1 |  Célula de conteúdo |
| SAMBA |  Célula de conteúdo |


- Definições de rede externa

| DESCRIÇÃO  |  IP  |
| ------------------- | ------------------- |
| REDE |  Célula de conteúdo |
|  MÁSCARA |  Célula de conteúdo |
| VIRTUALBOX |  Célula de conteúdo |
|  BROADCAST |  Célula de conteúdo |


- Nome dos servidores


| NOME DA VM  |  NOME  |
| ------------------- | ------------------- |
| GATEWAY | gw.elaine_914.laberedes.ifalarapiraca.local |
|  NAMESERVER1 |  ns1.elaine_914.laberedes.ifalarapiraca.local |
| VIRTUALBOX |  samba.elaine_914.laberedes.ifalarapiraca.local |
