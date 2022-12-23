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

```su redes```

```cd ~```

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



### Servidores de Nomes

&nbsp;

ETAPA 1

- Passo 1: Primeiramente, deve-se fazer a instalação da aplicação do DNS, bind9, com o comando:

```sudo apt-get install bind9 dnsutils bind9-doc```

> O sudo permite que o usuário comum tenha privilégios de acesso.
> O apt-get permite a instalação e atualização de pacotes.

- Passo 2: É necessário que haja a verificação do status do serviço, com o seguinte comando:

```sudo systemctl status bind9```

> O systemctl funciona como um gerenciador.

  - Passo 3: Caso o serviço não esteja funcionando, utilize o comando ```sudo systemctl enable bind9```
  
ETAPA 2

- Passo 1: Crie o diretório que será responsável por armazenar os arquivos de zona que deverá ser encontrado no diretório /etc/bind/zones com o comando:

```sudo mkdir /etc/bind/zones```

> O mkdir é o comando que cria diretórios ou subdiretórios.

- Passo 2 (Para a zona direta): Criar arquivo, cópia do /etc/bind/db.empty, que conterá os nomes das máquinas dentro do seu domínio. Utlize o comando abaixo.

```sudo cp /etc/bind/db.empty /etc/bind/zones/db.dominio```

> Arquivos db armazenam os nomes das máquinas, funcionando como bancos de dados.

- Passo 3 (Para a zona reversa): Criar arquivo db, cópia do /etc/bind/db.127.

```sudo cp /etc/bind/db.127 /etc/bind/zones/db.10.9.13.rev```

1. Foi utilizado "10.9.13" já que a rede é 10.9.14.0.

- Passo 4: Edite os arquivos para que se possa incrementar as informações do domínio (lembrar de entrar no diretório que os arquivos se encontram), com os comandos:

```sudo nano db.dominio ```

> Nano é um editor de texto.

ETAPA 3

Passo 1: Utilize o comando ```sudo named-checkconf``` para checar os arquivos.

Passo 2: Para checagem da sintaxe dos arquivos use o comando ```sudo named-checkzone nome_dominio nome_arquivo_db````

> O named-checkzone é utilizado para verificar os arquivos da zona DNS.

O mesmo deve ser feito pra o arquivo da zona reversa

Passo 3: Use o comando ```sudo nano /etc/default/named``` a fim de configurar para resolver endereços IPv4, adicionando no arquivo a linha ```OPTIONS="-4 -u bind```.

Passo 4: Habilite o bind9 e reinicie com os comandos:

```sudo systemctl enable bind9```
```sudo systemctl restart bind9```

ETAPA 4

Passo 1: Como é necessário configurar o dns nas máquinas (master e slave), incrementando os endereços IPs dessas máquinas nas interfaces de rede local, utilize o comando ```ls /etc/netplan``` para descobrir o nome do seu arquivo de configuração e logo em seguida:

```sudo nano /etc/netplan/nome_arquivo.yaml```

> Abaixo de nameserves e addresses adicione os endereços das máquinas.
> Em search, adicione o nome do domínio que a máquina participa.

&nbsp;

### Implementação do Servidor Web LAMP

&nbsp;

## Resultados

