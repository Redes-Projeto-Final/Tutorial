# Implementção do DNS Slave (NS2)


## ETAPA 1 - Passos iniciais

* Se conectar com a VPN com os seguintes comandos:
  
  ```$ openvpn3 session-start --config CONFIG_NAME```
  
  ```ssh administrador@10.9.13.124```
  
## ETAPA 2 - Configuração na interface de rede
  
 * Passo 1: Editar o arquivo Netplan para permitir o acesso a internet por meio do DNS Master
  
   ```$ sudo nano /etc/netplan/00-instaler-config.yaml```
   
  * Aplique as configurações:
  
    ```$ sudo netplan apply```

    ```$ ifconfig```
   
## ETAPA 3 - Configuração e instalação do servidor DNS secundário (slave)

* Passo 1: Instalar o bind 9

```$ sudo apt-get install bind9 dnsutils bind9-doc -y```

* Passo 2: Veriicar status

```$ sudo systemctl status bind9```

