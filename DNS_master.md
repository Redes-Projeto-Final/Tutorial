
# Implementação do DNS master

&nbsp;

**ETAPA 1**

Passo 1: Primeiramente, deve-se fazer a instalação da aplicação do DNS, bind9, com o comando:  

```sudo apt-get install bind9 dnsutils bind9-doc```

&nbsp;

![juredes2](https://user-images.githubusercontent.com/103438145/209967946-ead1a71f-b062-481b-9033-e07ce3e0b04a.png)
> O sudo permite que o usuário comum tenha privilégios de acesso.  
> O apt-get permite a instalação e atualização de pacotes.

&nbsp;

Passo 2: É necessário que haja a verificação do status do serviço, com o seguinte comando:
```sudo systemctl status bind9```

&nbsp;


1. O 'active' precisa estar como 'active (running)'.

![juliaredes2 (1)](https://user-images.githubusercontent.com/103438145/209394841-2e9df730-12ec-4f26-9a54-4445b33786f7.png)
> O systemctl funciona como um gerenciador.

&nbsp;

 Passo 3: Caso o serviço não esteja funcionando, utilize o comando ```sudo systemctl enable bind9```

&nbsp;
  
**ETAPA 2**

Passo 1: Como os arquivos do bind precisam estar no diretório "/etc/bind", deve-se criar um diretório "/zones" que será responsável por armazenar os arquivos de zona no diretório /etc/bind/zones com o comando:
```sudo mkdir /etc/bind/zones```

&nbsp;

![Captura de tela 2022-12-23 160510](https://user-images.githubusercontent.com/103438145/209395339-bd56fc0b-a306-491e-a76a-ef1873197aba.png)
> O mkdir é o comando que cria diretórios ou subdiretórios.

&nbsp;

Passo 2 (Para a zona direta): Criar arquivo db, cópia do /etc/bind/db.empty, que conterá os nomes das máquinas dentro do seu domínio. Utilize o comando como abaixo.  

```sudo cp /etc/bind/db.empty /etc/bind/zones/db.grupo5.turma913.ifalara.local```

&nbsp;


> Arquivos db armazenam os nomes das máquinas, funcionando como bancos de dados.

&nbsp;

Passo 3 (Para a zona reversa): Criar arquivo db, cópia do /etc/bind/db.127.  

```sudo cp /etc/bind/db.127 /etc/bind/zones/db.10.9.13.rev```

![Captura de tela 2022-12-23 160644](https://user-images.githubusercontent.com/103438145/209395467-9b284760-b083-4717-9731-1098111212d8.png)

&nbsp;

Passo 4: Edite os arquivos, tanto da zona direta quanto da reversa, para que se possa incrementar as informações do domínio (lembrar de entrar no diretório que os arquivos se encontram), com o comando como no exemplo abaixo:  

```sudo nano db.dominio ```
> Nano é um editor de texto.

&nbsp;

![juredes6](https://user-images.githubusercontent.com/103438145/209967958-b6b4e9d6-3195-415c-b7b9-62a9de38fb47.png)

&nbsp;

![juredes7](https://user-images.githubusercontent.com/103438145/209967966-c4c54ae4-03b2-4c04-b497-2b995825f4ed.png)

&nbsp;

**ETAPA 3**

Passo 1: Utilize o comando ```sudo named-checkconf``` para checar os arquivos.  

Passo 2: Para checagem da sintaxe dos arquivos use o comando ```sudo named-checkzone nome_dominio nome_arquivo_db```

![Captura de tela 2022-12-23 161511](https://user-images.githubusercontent.com/103438145/209396272-c204be48-eb29-4fb4-9998-45749d50bebd.png)
> O named-checkzone é utilizado para verificar os arquivos da zona DNS.

&nbsp;

O mesmo deve ser feito pra o arquivo da zona reversa.

![juredes1](https://user-images.githubusercontent.com/103438145/209967379-07714a93-f52b-4704-855d-d1814def3eea.png)

&nbsp;

Passo 3: Use o comando ```sudo nano /etc/default/named``` a fim de configurar para resolver endereços IPv4, adicionando no arquivo a linha ```OPTIONS="-4 -u bind```.

![juredes10](https://user-images.githubusercontent.com/103438145/209967975-997c3f41-2189-4f43-bb0e-6b7c05065508.png)

&nbsp;

Passo 4: Habilite o bind9 e reinicie com os comandos:

```sudo systemctl enable bind9```   
```sudo systemctl restart bind9```

![juliaredes17](https://user-images.githubusercontent.com/103438145/209396788-077ce470-85ab-4871-9747-dd9bef6b68f9.png)

&nbsp;

**ETAPA 4**

Passo 1: Como é necessário configurar o dns nas máquinas (master e slave), incrementando os endereços IPs dessas máquinas nas interfaces de rede local, utilize o comando ```ls /etc/netplan``` para descobrir o nome do seu arquivo de configuração e logo em seguida:  

```sudo nano /etc/netplan/nome_arquivo_configuração```

&nbsp;

![juredes1](https://user-images.githubusercontent.com/103438145/209853734-8a390946-d33e-4d20-84bc-20a5dcf58b40.png)
> Abaixo de nameserves e addresses adicione os endereços das máquinas.
> Em search, adicione o nome do domínio que a máquina participa.

&nbsp;

## Testes e Resultados 

Obs: Primeiramente, para testar o serviço DNS deve-se verificar que os campos DNS servers e DNS Domain estão certos com o comando ```systemd-resolve --status ens160```. 

![Captura de tela 2022-12-23 162531](https://user-images.githubusercontent.com/103438145/209397123-df746f16-faeb-4cec-8e52-5fd23dfb4c17.png)

&nbsp;

![juredes8](https://user-images.githubusercontent.com/103438145/209854428-55209750-3ae1-4835-86b8-d116b5ef68ce.png)
- Ping para a máquina smb.

&nbsp;

![juredes3](https://user-images.githubusercontent.com/103438145/209854431-eff16dce-d025-47a7-ad0a-b52e43c05f6e.png)
- Ping para a máquina ns2.

&nbsp;

![juredes4](https://user-images.githubusercontent.com/103438145/209854433-90f35f5b-e2d9-40bf-ab89-28915d3d3286.png)
- Ping para a máquina ns2.

&nbsp;

![juredes6](https://user-images.githubusercontent.com/103438145/209854436-ff4153c2-066d-4839-97fa-8f3c736a6f3e.png)
- Ping para a máquina gw.

&nbsp;

![juredes7](https://user-images.githubusercontent.com/103438145/209854438-63fe4725-dccf-469c-bf25-4ca83c2b6262.png)
- Ping para a máquina gw.

&nbsp;

![digx5](https://user-images.githubusercontent.com/103438145/209996341-fa716598-c675-4b92-9da0-9be4f5771b68.png)
- Pesquisa reversa de DNS do ns1 usando dig.

&nbsp;

![judigx1](https://user-images.githubusercontent.com/103438145/209996387-874d4e8e-8de6-4a2a-961b-83f52631e69f.png)
- Pesquisa reversa de DNS do ns2 usando dig.

&nbsp;

![judigx2](https://user-images.githubusercontent.com/103438145/209996389-193733d9-ca1b-4fe8-860b-560f1a2dd06d.png)
- Pesquisa reversa de DNS do gw usando dig.

&nbsp;

![judigx3](https://user-images.githubusercontent.com/103438145/209996390-5d143164-3dc1-40b2-b268-7060bebc8d40.png)
- Pesquisa reversa de DNS do smb usando dig.

&nbsp;

![dig2](https://user-images.githubusercontent.com/103438145/209996381-f9fad5db-c5b9-4c2e-9bb5-4c1cda665f15.png)
- Pesquisa de DNS do ns1 usando dig.

&nbsp;

![judig1](https://user-images.githubusercontent.com/103438145/209996382-ac6d8d0c-a046-4d73-90a7-ad51b70a3806.png)
- Pesquisa de DNS do gw usando dig.

&nbsp;

![judig4](https://user-images.githubusercontent.com/103438145/209996383-b6df4941-7cf7-4f57-b1aa-ceabd7fdf122.png)
- Pesquisa de DNS do smb usando dig.

&nbsp;

![judig7](https://user-images.githubusercontent.com/103438145/209996385-34325128-99f1-4b75-82b9-6794387ed55a.png)
- Pesquisa de DNS do ns2 usando dig.
