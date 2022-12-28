
# Implementação do DNS master

&nbsp;

ETAPA 1

- Passo 1: Primeiramente, deve-se fazer a instalação da aplicação do DNS, bind9, com o comando:

```sudo apt-get install bind9 dnsutils bind9-doc```

> O sudo permite que o usuário comum tenha privilégios de acesso.
> O apt-get permite a instalação e atualização de pacotes.

![juliaredes1](https://user-images.githubusercontent.com/103438145/209394647-13ffbcc8-c72b-4fd8-a94a-74d75a841a15.png)

- Passo 2: É necessário que haja a verificação do status do serviço, com o seguinte comando:

```sudo systemctl status bind9```

1. O 'active' precisa estar como 'active (running)'.

> O systemctl funciona como um gerenciador.

![juliaredes2 (1)](https://user-images.githubusercontent.com/103438145/209394841-2e9df730-12ec-4f26-9a54-4445b33786f7.png)

  - Passo 3: Caso o serviço não esteja funcionando, utilize o comando ```sudo systemctl enable bind9```


&nbsp;
  
ETAPA 2

- Passo 1: Como os arquivos do bind precisam estar no diretório "/etc/bind", deve-se criar um diretório "/zones" que será responsável por armazenar os arquivos de zona no diretório /etc/bind/zones com o comando:

```sudo mkdir /etc/bind/zones```

> O mkdir é o comando que cria diretórios ou subdiretórios.

![Captura de tela 2022-12-23 160510](https://user-images.githubusercontent.com/103438145/209395339-bd56fc0b-a306-491e-a76a-ef1873197aba.png)

- Passo 2 (Para a zona direta): Criar arquivo db, cópia do /etc/bind/db.empty, que conterá os nomes das máquinas dentro do seu domínio. Utlize o comando abaixo.

```sudo cp /etc/bind/db.empty /etc/bind/zones/db.dominio```

> Arquivos db armazenam os nomes das máquinas, funcionando como bancos de dados.

- Passo 3 (Para a zona reversa): Criar arquivo db, cópia do /etc/bind/db.127.

```sudo cp /etc/bind/db.127 /etc/bind/zones/db.10.9.13.rev```

![Captura de tela 2022-12-23 160644](https://user-images.githubusercontent.com/103438145/209395467-9b284760-b083-4717-9731-1098111212d8.png)

- Passo 4: Edite os arquivos, tanto da zona direta quanto da reversa, para que se possa incrementar as informações do domínio (lembrar de entrar no diretório que os arquivos se encontram), com os comandos:

```sudo nano db.dominio ```

> Nano é um editor de texto.

![JULIAREDES6 (1)](https://user-images.githubusercontent.com/103438145/209395674-2e803946-ba2e-417f-a8b9-a9596f5d073a.png)
![juliaredes7 (1)](https://user-images.githubusercontent.com/103438145/209395913-74c57839-e042-4594-9602-7a1b7c4d5755.png)

&nbsp;

ETAPA 3

Passo 1: Utilize o comando ```sudo named-checkconf``` para checar os arquivos.

Passo 2: Para checagem da sintaxe dos arquivos use o comando ```sudo named-checkzone nome_dominio nome_arquivo_db````

> O named-checkzone é utilizado para verificar os arquivos da zona DNS.

![Captura de tela 2022-12-23 161511](https://user-images.githubusercontent.com/103438145/209396272-c204be48-eb29-4fb4-9998-45749d50bebd.png)

O mesmo deve ser feito pra o arquivo da zona reversa.

![juliaredes13 (1)](https://user-images.githubusercontent.com/103438145/209396431-fd38c228-2121-47ef-bebc-8c6620025114.png)

Passo 3: Use o comando ```sudo nano /etc/default/named``` a fim de configurar para resolver endereços IPv4, adicionando no arquivo a linha ```OPTIONS="-4 -u bind```.

![julçiaredes16](https://user-images.githubusercontent.com/103438145/209396630-34e97fef-7873-488c-9e8e-709b53b1878f.png)

Passo 4: Habilite o bind9 e reinicie com os comandos:

```sudo systemctl enable bind9```
```sudo systemctl restart bind9```

![juliaredes17](https://user-images.githubusercontent.com/103438145/209396788-077ce470-85ab-4871-9747-dd9bef6b68f9.png)

&nbsp;

ETAPA 4

Passo 1: Como é necessário configurar o dns nas máquinas (master e slave), incrementando os endereços IPs dessas máquinas nas interfaces de rede local, utilize o comando ```ls /etc/netplan``` para descobrir o nome do seu arquivo de configuração e logo em seguida:

```sudo nano /etc/netplan/nome_arquivo_configuração```

> Abaixo de nameserves e addresses adicione os endereços das máquinas.
> Em search, adicione o nome do domínio que a máquina participa.

![juredes1](https://user-images.githubusercontent.com/103438145/209853734-8a390946-d33e-4d20-84bc-20a5dcf58b40.png)
![Captura de tela 2022-12-23 162531](https://user-images.githubusercontent.com/103438145/209397123-df746f16-faeb-4cec-8e52-5fd23dfb4c17.png)

&nbsp;

## Testes e Resultados 

Obs: Primeiramente, para testar o serviço DNS deve-se verificar que os campos DNS servers e DNS Domain estão certos com o comando ```systemd-resolve --status ens160```. 

&nbsp;

![juliaredes22](https://user-images.githubusercontent.com/103438145/209397188-5b9b3e06-647d-487f-be34-fddc2ff4db6e.png)
- Teste do serviço DNS para a máquina ns1 com dig.

&nbsp;

![juliaredes23](https://user-images.githubusercontent.com/103438145/209397396-37145c05-11be-439b-a8eb-b6528887624a.png)
- Teste do serviço DNS reverso para a máquina ns1 com dig .

&nbsp;

![juliaredes24](https://user-images.githubusercontent.com/103438145/209397702-8a2abebc-8c5d-4b93-9693-9af14c8a812f.png)
- Teste do serviço DNS reverso para a máquina ns2 com dig.

&nbsp;
