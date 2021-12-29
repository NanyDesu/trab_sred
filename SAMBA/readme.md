# Configuração do Samba

- O Samba é um "software servidor" para Linux (e outros sistemas baseados em Unix) que permite o gerenciamento e compartilhamento de recursos em redes formadas por computadores com o Windows.

Inicialmente iremos definir o nome da máquina virtual para ns1

```bash
 sudo nano /etc/hostname
```
```bash
 sudo nano /etc/hosts
```

Após atualizar ja podemos instalar o samba na VM
```bash
 sudo apt update
 ```
 ![update](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt9_update.PNG)
 
```bash
 sudo apt install samba
```
```bash
 whereis samba
```
![update](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt10_install_samba.PNG)
![update](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt12_status_samba_listen.PNG)

 
```bash
 sudo cp /etc/samba/smb.conf{,.backup}
 ```
 (utilizado para fazer um backup da configuração do samba)
 
 ```bash
 ls -la /etc/samba
```
 (verificando se há um backup do arquivo)
 
 ```bash
sudo bash -c 'grep -v -E "^#|^;" /etc/samba/smb.conf.backup | grep . > /etc/samba/smb.conf'
```

```bash
 sudo nano /etc/samba/smb.conf
```


```bash
 sudo nano /etc/samba/smb.conf
```

```bash
  interfaces = 127.0.0.1/8 ens160 ens192
  netbios name = ns1
```

![smbd.conf](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt14_smb_conf.PNG)

```bash
 sudo systemctl restart smbd
```
(reinicia o serviço)

```bash
 sudo adduser aluno
```
```bash
 sudo smbpasswd -a aluno
```
(criando usuário e senha para compartilhamento do samba)

```bash
 sudo usermod -aG sambashare aluno
```
(Alteração do GID)

```bash
 sudo mkdir /home/aluno/sambashare/
```
(Criar diretório em home e aluno (sambashare))


```bash
 sudo mkdir -p /samba/publico
```
(Criando diretório público)

 ```bash
sudo chown -R nobody:nogroup /samba/public
```

```bash
sudo chmod -R 0775 /samba/public
```

```bash
sudo chgrp sambashare /samba/public
```
![chgrp](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt11_install_samba_final.PNG)

```bash
 sudo ufw disable
```

```bash
 sudo ufw status
```
![chgrp](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt16_ufw_disable.PNG)
![chgrp](https://github.com/NanyDesu/trab_sred/blob/main/imagens/pt17_teste_de_compartilhamento.PNG)

(através do explorador de arquivos podemos verificar se está funcionando através do ip)

- Próximo passo:

[Configuração DNS](https://github.com/NanyDesu/trab_sred/tree/main/DNS)
