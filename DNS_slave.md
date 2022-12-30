# Implementção do DNS Slave (NS2)


## ETAPA 1 - Passos iniciais

* Se conectar com a VPN com os seguintes comandos:
  
  ```$ openvpn3 session-start --config CONFIG_NAME```
  
  ```ssh administrador@10.9.13.124```
  
## ETAPA 2 - Configuração na interface de rede
  
 * Passo 1: Editar o arquivo ```/etc/netplan/00-installer-config.yaml``` para permitir o acesso a internet por meio do DNS Master
  
   ```$ sudo nano /etc/netplan/00-instaler-config.yaml```
   
   ![Captura de tela de 2022-12-27 11-32-26](https://user-images.githubusercontent.com/103398796/210071227-551e4547-956e-4119-aa5b-51de542d1dbc.PNG)
   
   
  * Aplique as configurações:
  
    ```$ sudo netplan apply```

    ```$ ifconfig```  
    
    ![Captura de tela de 2022-12-27 11-33-02](https://user-images.githubusercontent.com/103398796/210071260-7b5ad40d-afc0-45bd-9387-de61fbace72e.PNG)
    
    &nbsp;
   
## ETAPA 3 - Configuração e instalação do servidor DNS secundário (slave)

* Passo 1: Instalar o bind 9

```$ sudo apt-get install bind9 dnsutils bind9-doc -y```

![Captura de tela de 2022-12-21 13-52-05](https://user-images.githubusercontent.com/103398796/210071216-9ad7ffa9-223a-47e2-bbbd-e39712a934d1.PNG)
![Captura de tela de 2022-12-21 13-52-10](https://user-images.githubusercontent.com/103398796/210071223-1c6eaa81-d05e-4b9c-89ec-12246be93830.PNG)

&nbsp;

* Passo 2: Verificar status

```$ sudo systemctl status bind9```

![Captura de tela de 2022-12-21 14-13-47](https://user-images.githubusercontent.com/103398796/210071243-1e5a6ca9-c837-427e-873c-bf02e3e218a2.PNG)

&nbsp;

## ETAPA 4 - Configuração de zonas

* Passo 1: Editar o arquivo ```/etc/bind/named.conf.local```: 

```$ sudo nano /etc/bind/named.conf.local```

![Captura de tela de 2022-12-27 11-38-42](https://user-images.githubusercontent.com/103398796/210071270-885e07ec-bb58-4c78-870e-b697c34b6829.PNG)

&nbsp;

## ETAPA 5 - Checagem da sintaxe

```$ sudo named-checkconf```

> Se não houver retorno após esse comando, significa que está tudo certo.

![Captura de tela de 2022-12-27 11-39-33](https://user-images.githubusercontent.com/103398796/210071277-81266773-a60e-4b51-8207-fec5e8600e68.PNG)

&nbsp;

## ETAPA 6 - Testes

* Teste com DIG

```$ dig @10.9.13.117 ns1.grupo5.turma913.ifalara.local```

![Captura de tela de 2022-12-27 11-41-14](https://user-images.githubusercontent.com/103398796/210071282-3a491fbd-6561-418b-bdfc-cd69462a1e1e.PNG)

* Teste com nslookup

![nslookup5](https://user-images.githubusercontent.com/103398796/210071899-8fc973d1-5b39-4653-8939-c3bf61cf1f18.png)

> nslookup do GW

![nslookup6](https://user-images.githubusercontent.com/103398796/210071902-7e87f817-c847-489d-9e69-70dcc33d4a51.png)

> nslookup do SMB

![nslookup8](https://user-images.githubusercontent.com/103398796/210071908-164f1c32-6a63-4774-803b-b0ecd7f42237.png)

> nslookup do NS1

![nslookup9](https://user-images.githubusercontent.com/103398796/210071928-2e35829d-0415-4a2c-9647-5ed8290216fe.png)

> nslookup do NS2

