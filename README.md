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

<pre>
------------------------------------------------------------------
| IP da Subrede:    |   10.9.13.0/24   |   192.168.13.64/28      |
| IP de Broadcast:  |  10.9.13.255/24  |   192.168.13.79/28      |
------------------------------------------------------------------
</pre>

&nbsp;

<pre>
            -----------------------------------------------------------------------------------
            |    IP (ens160)    |    IP (ens192)    |                  FQDN                   |
-----------------------------------------------------------------------------------------------
|    GW     |    10.9.13.102    |   192.168.13.73   | gw.grupo5.turma913.local.ifalara.local  |
|   SAMBA   |    10.9.13.103    |   192.168.13.74   | smb.grupo5.turma913.local.ifalara.local |
|    NS1    |    10.9.13.117    |   192.168.13.75   | ns1.grupo5.turma913.local.ifalara.local |
|    NS2    |    10.9.13.124    |   192.168.13.76   | ns2.grupo5.turma913.local.ifalara.local |
|    WEB    |    10.9.13.219    |   192.168.13.77   | www.grupo5.turma913.local.ifalara.local |
|    BD     |    10.9.13.220    |   192.168.13.78   | bd.grupo5.turma913.local.ifalara.local  |
-----------------------------------------------------------------------------------------------
</pre>

&nbsp;

### Usuários da VMs

- antony;
- beatriz;
- eduarda;
- julia;

Com o comando ```sudo cat/etc/passwd``` os usuários podem ser visualizados.

![Captura de tela 2022-12-28 143519](https://user-images.githubusercontent.com/103438145/209850850-f6378101-8f20-4ae4-a364-309007d523e3.png)


&nbsp;
