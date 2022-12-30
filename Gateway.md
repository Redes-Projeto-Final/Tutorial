# Gateway Server

 ### Passo 1:
  Acessar o usuário ``` redes ```:

   ```bash
      su redes 
      cd ~
   ```


 ### Passo 2:
   Iniciar a conexão VPN, utilizando seu usuário e sua senha, para acessar a VM destinada ao serviço de ```Gateway```:

   ``` openvpn3 session-start --config CONFIG_NAME ```

  ### Passo 3:
   Após acessar a VM é necessário ativar o ```Firewall``` e habilitar o acesso pelo ```ssh``` seguindo os seguintes comandos:

   ```bash
      sudo ufw enable
      sudo ufw allow ssh 
   ```
   Para certificar que seu ```Firewall``` está ativo é necessário verificar pelo seguinte comando:

   ``` sudo ufw status ```
   Estando realmente ativo, aparecerá no terminal as seguintes informações:

   ![firewall](https://user-images.githubusercontent.com/104701006/210070741-32550179-f427-4516-9efa-254d5adb92b6.png)

  ### Passo 4:
  Prosseguindo na configuração do ```Gateway```, deve ser habilitado o encaminhamneto dos pacotes vindos da ```WAN``` para a ```LAN``` e vice-versa. Essa opção é habilitada da seguinte forma: Acessar o arquivo ```/etc/ufw/sysctl.conf``` e remover o comentário ```(#)``` da linha que contém ```# net/ipv4/ip_forwarding=1```
  
  #### 4.1 - Acessando o arquivo a ser editado:
  ``` sudo nano /etc/ufw/sysctl.conf ```
  #### 4.2 - Linha a ser removido o comentário (#):
  ```# net/ipv4/ip_forwarding=1```
  #### 4.3 - Aplicar as alterações realizadas:
  ``` sudo netplan apply ```
  
  ### Passo 5:
  Verificar quais interfaces de rede estão sendo utilizadas:
  ``` ifconfig -a```
    
  O resultado exibido na tela informará os nomes das interfaces estão sendo utilizadas. No caso abaixo, os nomes das interfaces utilizadas são ```ens160``` e ```ens192```, respectivamente ```WAN``` e ```LAN```.
  
  ![Ifconfig -a](https://user-images.githubusercontent.com/104701006/210070506-caab7e44-e7a9-4652-b37e-f1860e3421dd.png)
   
   ```bash
      ens192: LAN
      ens160: WAN
   ```
 ### Passo 5:
 Após visualizar a nomenclatura das interfaces, é necessário editar no arquivo ```/etc/netplan/00-installer-config.yaml``` as configurações de cada interface (gateway, ip estático, etc). Seguindo esse modelo:
 
  #### 5.1 - Acessando o arquivo a ser editado:
  ``` sudo nano /etc/netplan/00-installer-config.yaml```
  #### 5.2 - Modelo a ser seguido:
  ```                     
   network:
       renderer: networkd
       ethernets:
           ens160:
               addresses: [10.9.13.102/24]  #IP da sua máquina na WAN
               dhcp4: false
               gateway4: 10.9.13.1          #Gateway da WAN
               nameservers:
                   addresses:
                     - 10.9.13.117                           #IP do máquina configurada para NS1 na WAN
                     - 10.9.13.124                           #IP do máquina configurada para NS2 na WAN
                   search: [grupo5.turma913.ifalara.local]   #Domínio 

           ens192:
               addresses: [192.168.13.73/28] #IP da sua máquina na LAN
               dhcp4: false
           #    gateway4: 192.168.13.1       #Gateway da LAN deve estar comentado
               nameservers:
                   addresses:
                     - 192.168.13.75                        #IP do máquina configurada para NS2 na LAN
                     - 192.168.13.76                        #IP do máquina configurada para NS2 na LAN
                   search: [grupo5.turma913.ifalara.local]  #Domínio
       version: 2
```
  #### 5.3 - Aplicar as alterações realizadas:
  ``` sudo netplan apply ```
  #### 5.4 - Verificar as alterações realizadas:
  ``` ifconfig -a ```
  
 ### Passo 6:
 O passo seguinte é criar o arquivo ```/etc/rc.local``` e adicionar o script abaixo, a partir dele algumas configurações de recebimento e envio de pacotes serão definidas. Além de algumas definições para as redes WAN e LAN. É importante alterar as interfaces do arquivo para as suas interfaces, caso sejam diferentes das descritas aqui.
 
  #### 6.1: 
  ``` sudo nano /etc/rc.local```
  #### 6.2 - Script a ser adicionado:
  ```#!/bin/bash

# /etc/rc.local

# Default policy to drop all incoming packets.
# Politica padrão para bloquear (drop) todos os pacotes de entrada
iptables -P INPUT DROP
iptables -P FORWARD DROP

# Accept incoming packets from localhost and the LAN interface.
# Aceita pacotes de entrada a partir das interfaces localhost e the LAN.
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -i ens192 -j ACCEPT

# Accept incoming packets from the WAN if the router initiated the connection.
# Aceita pacotes de entrada a partir da WAN se o roteador iniciou a conexao
iptables -A INPUT -i ens160 -m conntrack \
--ctstate ESTABLISHED,RELATED -j ACCEPT

# Forward LAN packets to the WAN.
# Encaminha os pacotes da LAN para a WAN
iptables -A FORWARD -i ens192 -o ens160 -j ACCEPT

# Forward WAN packets to the LAN if the LAN initiated the connection.
# Encaminha os pacotes WAN para a LAN se a LAN inicar a conexao.
iptables -A FORWARD -i ens160 -o ens192 -m conntrack \
--ctstate ESTABLISHED,RELATED -j ACCEPT

# NAT traffic going out the WAN interface.
# Trafego NAT sai pela interface WAN
iptables -t nat -A POSTROUTING -o ens160 -j MASQUERADE

# rc.local needs to exit with 0
# rc.local precisa sair com 0
exit 0 
```

 #### 6.3 - Aplicando as alterações: 
  ``` sudo netplan apply```
  
### Passo 7:
Após criar o arquivo e colar o script, com o comando abaixo o arquivo se tornará um executável e será inicializado com o boot.

``` sudo chmod 755 /etc/rc.local ``` 

### Passo 8:
Verificar se o ```Firewall``` está ativo e posteriormente reiniciar a máquina.

  #### 8.1:
  ``` sudo ufw status ```
   #### 8.2:
  ``` sudo reboot ```
  
### Passo 9:
Após configurar seu ```Gateway```, ele já pode ser adicionado nas interfaces dos demais serviços no arquivo  ``` sudo nano /etc/netplan/00-installer-config.yaml```

  #### 9.1 (Os comentários podem ser removidos posteriormente):
```
network:
       renderer: networkd
       ethernets:
           ens160:
               addresses: [10.9.13.124/24]  #IP da sua máquina na WAN
               dhcp4: false
             #  gateway4: 10.9.13.102          #IP da máquina Gateway na WAN deve estar comentado
               nameservers:
                   addresses:
                     - 10.9.13.117                           #IP do máquina configurada para NS1 na WAN
                     - 10.9.13.124                           #IP do máquina configurada para NS2 na WAN
                   search: [grupo5.turma913.ifalara.local]   #Domínio 

           ens192:
               addresses: [192.168.13.76/28] #IP da sua máquina na LAN
               dhcp4: false
               gateway4: 192.168.13.73       #IP da máquina Gateway na LAN
               nameservers:
                   addresses:
                     - 192.168.13.75                        #IP do máquina configurada para NS2 na LAN
                     - 192.168.13.76                        #IP do máquina configurada para NS2 na LAN
                   search: [grupo5.turma913.ifalara.local]  #Domínio
       version: 2
```
 #### 9.2 - Aplicando as alterações:
  ``` sudo netplan apply```
 #### 9.3 - Verificar as alterações:
  ``` ifconfig -a```
  
 ### Passo 10:
 
 Nas máquinas SGATEWAY é necessário adicionar as informações de redirecionamento de portas a partir do IPTABLES, no arquivo ```/etc/rc.local```. Após as alterações no arquivo é necessário reiniciar a máquina.
 
 #### SAMBA:
 ```
 #Recebe pacotes na porta 445 da interface externa do gw e encaminha para o servidor interno na porta 445
iptables -A PREROUTING -t nat -i ens160 -p tcp --dport 445 -j DNAT --to 10.9.13.103:445
iptables -A FORWARD -p tcp -d 10.9.13.103 --dport 445 -j ACCEPT

#Recebe pacotes na porta 139 da interface externa do gw e encaminha para o servidor interno na porta 139
iptables -A PREROUTING -t nat -i ens160 -p tcp --dport 139 -j DNAT --to 10.9.13.103:139
iptables -A FORWARD -p tcp -d 10.9.13.103 --dport 139 -j ACCEPT

```
#### NS1 e NS2:
```
#Recebe pacotes na porta 53 da interface externa do gw e encaminha para o servidor DNS Master interno na porta 53
iptables -A PREROUTING -t nat -i ens160 -p udp --dport 53 -j DNAT --to 10.9.13.117:53
iptables -A FORWARD -p udp -d 10.9.13.117 --dport 53 -j ACCEPT

```
