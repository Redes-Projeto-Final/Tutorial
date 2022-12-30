### SAMBA

&nbsp; 

### Etapa inicial

- Passo 1: Primeiramente é feito o login no usuário ```redes``` usando os seguintes comandos:

```
su rede

cd ~
```
![b1](https://user-images.githubusercontent.com/103438080/210071163-dcdf7301-713c-499c-904b-5be0ac6d0ad3.png)

- Passo 2: Realizado o login, deve-se iniciar a conexão com a VPN com o comando:

```openvpn3 session-start --config vpn913.labredes```

A conexão depende do usuário e senha que devem ser previamente configurados na máquina virtual.

- Passo 3: Agora acesse a sua VM remotamente, pelo terminal do computador, usando o SSH:

```ssh administrador@<insira aqui o endereço IP da sua VM>```

Ex.: ```ssh administrador@10.9.13.103```


![b2](https://user-images.githubusercontent.com/103438080/210071252-8ffc9c92-250b-4b8d-9083-5ade19724767.png)


- Passo 4: Use o comando ```hostname``` para verificação do nome do host.

![b3](https://user-images.githubusercontent.com/103438080/210071299-ca3039df-5e12-4a87-94f3-540fcbbf709a.png)

- Passo 5: Use o comando ```hostname -I``` para verificação do IP e da máscara de rede.

![b4](https://user-images.githubusercontent.com/103438080/210071315-9823ec05-abb1-4ac7-97b2-ee0df3528690.png)

- Passo 6: Com ```$ resolvectl status ens160``` verificamos o status do DNS.


![b5](https://user-images.githubusercontent.com/103438080/210071337-ca5dae9d-83d3-4189-afad-4b345d493b96.png)


### Instalação e configuração

- Passo 1: Deve-se verificar e definir o IP da rede interna para o Samba. Fazemos isso com o comando:

```$ sudo nano /etc/netplan/00-installer-config.yaml```

![b7](https://user-images.githubusercontent.com/103438080/210071362-29d9a47c-d189-48cd-a3df-f8ec2b638089.png)


Caso seja preciso realizar alguma alteração, é necessário salvar o arquivo e aplicar a configuração. Para isso, use o comando:

```$ sudo netplan apply```


![b8](https://user-images.githubusercontent.com/103438080/210071380-3360d3b4-3015-4be0-b696-df3774d39299.png)


- Passo 2: Use o comando ```$ ifconfig -a``` para verificar informações da configuração das interfaces de rede.

- Passo 3: Faça o ping para a máquina virtusl usando ```$ ping 10.9.24.1```. Se tudo estiver funcionando, prossiga com a conexão entre a máquina e o terminal via SSH.

Para a conexão, use: ```ssh administrador@<insira aqui o endereço IP da sua VM>```

Ex.: ```ssh administrador@10.9.13.103```

![b9](https://user-images.githubusercontent.com/103438080/210071392-78353d10-09ce-4356-aab8-7b3ae611240a.png)

- Passo 4: Agora realize a instalação do SAMBA, para isso use os comandos:

```$ sudo apt update```

![b10](https://user-images.githubusercontent.com/103438080/210071441-2dd15a7f-651b-4bf1-a8d6-615c68929307.png)

```$ sudo apt install samba```

- Passo 5: Depois de instalado, verifique o status de funcionamento do SAMBA usando o comando: 

```$ whereis samba```

![b11](https://user-images.githubusercontent.com/103438080/210071461-889b25e8-e164-4e5c-acc0-90510d455790.png)

- Os comandos abaixo servem para verificação de status.

![b12](https://user-images.githubusercontent.com/103438080/210071480-47e4f785-73ba-4755-a0c4-7b3104720801.png)


![b13](https://user-images.githubusercontent.com/103438080/210071528-4658f19a-7b8a-436f-832f-43b843bab9fa.png)


![b14](https://user-images.githubusercontent.com/103438080/210071549-07719e00-9778-4b15-ad9a-7a2b62ebeee4.png)

- Passo 6: Os comandos abaixo deverão ser utilizados para a realização do backup do arquivo e a criação de um novo contendo os comandos necessários.


```
$ sudo cp /etc/samba/smb.conf{,.backup}

$ ls -la

-rw-r--r--  1 root root 8942 Mar 22 20:55 smb.conf

-rw-r--r--  1 root root 8942 Mar 23 01:42 smb.conf.backup

$

$ sudo bash -c 'grep -v -E "^#|^;" /etc/samba/smb.conf.backup | grep . > /etc/samba/smb.conf'
```


![b15](https://user-images.githubusercontent.com/103438080/210071564-9792b2ea-c236-41f8-b831-9eeb79ebfe59.png)


- Passo 7: Use ```$ sudo nano /etc/samba/smb.conf``` para verificar o arquivo de configuração e depois edite o arquivo para adicionar as interfaces da máquina utilizada. 

![b26](https://user-images.githubusercontent.com/103438080/210071866-c58d1cae-a1d2-4089-921b-1d1f1a27fc7b.png)


- Passo 8: Após salvar as modificações, reinicie o ```smbd service````

```$ sudo systemctl restart smbd```


- Passo 9: Criação de usuários e vinculação do usuário criado ao Samba.

![b16](https://user-images.githubusercontent.com/103438080/210071593-8febcd30-91c1-4f75-bb8c-e40e6a328073.png)

![b17](https://user-images.githubusercontent.com/103438080/210071618-ae17fa93-8b80-4137-9fd6-46e39dbb7da3.png)
![b18](https://user-images.githubusercontent.com/103438080/210071636-3f4f5814-0e3a-47e7-b933-108c5fc64f63.png)

![b19](https://user-images.githubusercontent.com/103438080/210071663-025f760d-1fa7-44b8-80a7-d19628f96d44.png)


![b20](https://user-images.githubusercontent.com/103438080/210071694-0f59aad8-d2d5-41ce-a6c3-953395f24837.png)

![b21](https://user-images.githubusercontent.com/103438080/210071740-a590dd71-2f1f-43a8-b0de-d9e8ed04219b.png)




- Passo 10: Crie um diretório para viabilizar o compartilhamento do Samba na rede.

```
sudo chown -R nobody:nogroup /samba/public
sudo chmod -R 0775 /samba/public
sudo chgrp sambashare /samba/public
```


![b22](https://user-images.githubusercontent.com/103438080/210071757-8a7f03d4-20a5-4cb9-9b09-6950dc13b6f2.png)
![b23](https://user-images.githubusercontent.com/103438080/210071789-8ba60623-aea8-44fe-8ad5-0323de2e1008.png)

![b24](https://user-images.githubusercontent.com/103438080/210071808-98958f9d-aa4a-4492-b087-89f5ee77fc46.png)

![b25](https://user-images.githubusercontent.com/103438080/210071826-6059592c-8262-4111-a5d3-45ab78cee5b7.png)


![b27](https://user-images.githubusercontent.com/103438080/210071891-8ae04fff-8749-4008-bd6e-5fdefabc4b5a.png)


![b28](https://user-images.githubusercontent.com/103438080/210071924-e7a5b2e0-74d4-499b-a1a2-1a2022395701.png)


&nbsp;
