## Testes e Resultados 

Obs: Primeiramente, para testar o serviço DNS deve-se verificar que os campos DNS servers e DNS Domain estão certos com o comando ```systemd-resolve --status ens160```. 

![Captura de tela 2022-12-23 162531](https://user-images.githubusercontent.com/103438145/209397123-df746f16-faeb-4cec-8e52-5fd23dfb4c17.png)

&nbsp;

![juredes8](https://user-images.githubusercontent.com/103438145/209854428-55209750-3ae1-4835-86b8-d116b5ef68ce.png)
- Ping para a máquina smb.

&nbsp;

![Captura de tela 2022-12-29 160450](https://user-images.githubusercontent.com/103438145/209997708-a1b5e4be-8bd7-4851-80c8-ff1191d7b97e.png)
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

&nbsp;

![3nslookupns1](https://user-images.githubusercontent.com/103438145/210069196-b873552a-d6b0-42d5-a3c5-4258033b85d2.png)
- Nslookup do gw.

&nbsp;

![2nslookupns1](https://user-images.githubusercontent.com/103438145/210069201-1f14ccd3-103d-4599-ac8f-26ad9735092d.png)
- Nslookup do smb.

&nbsp;

![1nslookupns1](https://user-images.githubusercontent.com/103438145/210069204-eec9657f-5d70-4ccb-a4d0-dc6dc6af8b9e.png)
- Nslookup do ns.



![Captura de tela de 2022-12-30 09-44-26](https://user-images.githubusercontent.com/104701006/210071487-f8d23502-a782-4b54-98ab-be280600b446.png)
- Ping para a máquina smb.

&nbsp;
![Captura de tela de 2022-12-30 09-47-20](https://user-images.githubusercontent.com/104701006/210071680-6ca15230-cf90-47c1-a668-98d8e0e85a34.png)
- Ping para a máquina ns1.

&nbsp;
![Captura de tela de 2022-12-30 09-46-10](https://user-images.githubusercontent.com/104701006/210071571-7f6689ff-b414-4abc-8fd5-8c3854d22e54.png)
- Ping para a máquina ns2.


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
