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

  -- Passo 2.1: Caso o serviço não esteja funcionando, utilize o comando ```sudo systemctl enable bind9```
  
ETAPA 2

- Passo 1: Crie o diretório que será responsável por armazenar os arquivos de zona que deverá ser encontrado no diretório /etc/bind/zones com o comando:

```sudo mkdir /etc/bind/zones```

> O mkdir é o comando que cria diretórios ou subdiretórios.

- Passo 2: Após entrar 

&nbsp;

### Implementação do Servidor Web LAMP

&nbsp;

## Resultados

