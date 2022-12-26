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

   **Imagem do teste firewall**

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
  
   **Print do ifconfig**
   
   ```bash
      ens192: LAN
      ens160: WAN
   ```
 ### Passo 5:
 Após visualizar a nomenclatura das interfaces, é necessário editar no arquivo ```/etc/netplan/50-cloud-init.yaml``` as configurações de cada interface (gateway, ip estático, etc). Seguindo esse modelo:
 
  #### 5.1 - Acessando o arquivo a ser editado:
  ``` sudo nano /etc/netplan/50-cloud-init.yaml```
  #### 5.2 - Modelo a ser seguido:
  ```-----------------```
  #### 5.3 - Aplicar as alterações realizadas:
  ``` sudo netplan apply ```

 ### Passo 5:
