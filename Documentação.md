# Projeto Final da Disciplina de Redes

Instituto Federal de Alagoas
<br>
Infraestrutura e Serviços de Rede
<br>
Turma: 913   |   Curso: Informática
<br>
Grupo 5 - Júlia Ferreira, Antony Gabriel, Beatriz Santos e Maria Eduarda Lima

&nbsp;

### Configuração de Hardware das VMs

- Sistema Operacional: Ubuntu Server;
- Processadores: 1;
- Memória RAM: 512 MB;

&nbsp;

### Tabela com as definições

&nbsp;

<pre>
------------------------------------------------------------------
| IP da Subrede:    |   10.9.13.0/24   |   192.168.13.64/28      |
| IP de Broadcast:  |  10.9.13.255/24  |   192.168.13.79/28      |
------------------------------------------------------------------
</pre>

&nbsp;

<pre>
            -----------------------------------------------------------------------------------
            |    IP (ens160)    |    IP (ens192)    |                  FQDN                   |
-----------------------------------------------------------------------------------------------
|    GW     |    10.9.13.102    |   192.168.13.73   | gw.grupo5.turma913.local.ifalara.local  |
|   SAMBA   |    10.9.13.103    |   192.168.13.74   | smb.grupo5.turma913.local.ifalara.local |
|    NS1    |    10.9.13.117    |   192.168.13.75   | ns1.grupo5.turma913.local.ifalara.local |
|    NS2    |    10.9.13.124    |   192.168.13.76   | ns2.grupo5.turma913.local.ifalara.local |
|    WEB    |    10.9.13.219    |   192.168.13.77   | www.grupo5.turma913.local.ifalara.local |
|    BD     |    10.9.13.220    |   192.168.13.78   | bd.grupo5.turma913.local.ifalara.local  |
-----------------------------------------------------------------------------------------------
</pre>

&nbsp;

### Usuários da VMs

- antony;
- beatriz;
- eduarda;
- julia;

Com o comando ```sudo cat/etc/passwd``` os usuários podem ser visualizados.

![Captura de tela 2022-12-28 143519](https://user-images.githubusercontent.com/103438145/209850850-f6378101-8f20-4ae4-a364-309007d523e3.png)


&nbsp;

### SAMBA

&nbsp; 

### Etapa inicial

- Passo 1: Primeiramente é feito o login no usuário ```redes``` usando os seguintes comandos:

```
su rede

cd ~
```

- Passo 2: Realizado o login, deve-se iniciar a conexão com a VPN com o comando:

```openvpn3 session-start --config vpn913.labredes```

A conexão depende do usuário e senha que devem ser previamente configurados na máquina virtual.

- Passo 3: Agora acesse a sua VM remotamente, pelo terminal do computador, usando o SSH:

```ssh administrador@<insira aqui o endereço IP da sua VM>```

Ex.: ```ssh administrador@10.9.13.103```

- Passo 4: Use o comando ```hostname``` para verificação do nome do host.

- Passo 5: Use o comando ```hostname -I``` para verificação do IP e da máscara de rede.

- Passo 6: Com ```$ resolvectl status ens160``` verificamos o status do DNS.

### Instalação e configuração

- Passo 1: Deve-se verificar e definir o IP da rede interna para o Samba. Fazemos isso com o comando:

```$ sudo nano /etc/netplan/00-installer-config.yaml```

Caso seja preciso realizar alguma alteração, é necessário salvar o arquivo e aplicar a configuração. Para isso, use o comando:

```$ sudo netplan apply```

- Passo 2: Use o comando ```$ ifconfig -a``` para verificar informações da configuração das interfaces de rede.

- Passo 3: Faça o ping para a máquina virtusl usando ```$ ping 10.9.24.1```. Se tudo estiver funcionando, prossiga com a conexão entre a máquina e o terminal via SSH.

Para a conexão, use: ```ssh administrador@<insira aqui o endereço IP da sua VM>```

Ex.: ```ssh administrador@10.9.13.103```

- Passo 4: Agora realize a instalação do SAMBA, para isso use os comandos:

```$ sudo apt update```

```$ sudo apt install samba```

- Passo 5: Depois de instalado, verifique o status de funcionamento do SAMBA usando o comando: 

```$ whereis samba```

- Passo 6: Os comandos abaixo deverão ser utilizados para a realização do backup do arquivo e a criação de um novo contendo os comandos necessários.

```$ sudo cp /etc/samba/smb.conf{,.backup}```

```$ ls -la```

```-rw-r--r--  1 root root 8942 Mar 22 20:55 smb.conf```

```-rw-r--r--  1 root root 8942 Mar 23 01:42 smb.conf.backup```

```$```

```$ sudo bash -c 'grep -v -E "^#|^;" /etc/samba/smb.conf.backup | grep . > /etc/samba/smb.conf'```


- Passo 7: Use ```$ sudo nano /etc/samba/smb.conf``` para verificar o arquivo de configuração. 



- CONTINUA...

&nbsp;
