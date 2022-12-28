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

### Usuários da VMs

&nbsp;

## Configurações

&nbsp;

### Gateway Server

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



## Testes e Resultados 





