# Implementção do DNS Slave (NS2)


## ETAPA 1 - Passos iniciais

* Se conectar com a VPN com os seguintes comandos:
  
  ```$ openvpn3 session-start --config CONFIG_NAME```
  
  ```ssh administrador@10.9.13.124```
  
## ETAPA 2 - Configuração na interface de rede
  
 * Passo 1: Editar o arquivo ```/etc/netplan/00-instaler-config.yaml``` para permitir o acesso a internet por meio do DNS Master
  
   ```$ sudo nano /etc/netplan/00-instaler-config.yaml```
   
  * Aplique as configurações:
  
    ```$ sudo netplan apply```

    ```$ ifconfig```
   
## ETAPA 3 - Configuração e instalação do servidor DNS secundário (slave)

* Passo 1: Instalar o bind 9

```$ sudo apt-get install bind9 dnsutils bind9-doc -y```

* Passo 2: Veriicar status

```$ sudo systemctl status bind9```

## ETAPA 4 - Configuração de zonas

* Passo 1: Editar o arquivo ```/etc/bind/named.conf.local```: 

```$ sudo nano /etc/bind/named.conf.local```

## ETAPA 5 - Checagem da sintaxe

```$ sudo named-checkconf```

> Se não houver retorno após esse comando, significa que está tudo certo.

## ETAPA 6 - Teste com o Dig

```$ dig @10.9.13.117 ns1.grupo5.turma913.ifalara.local````
