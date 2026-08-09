Análise de Evento 4625 - Falha de Autenticação
## Objetivo

Investigar falhas de autenticação registradas no Windows e identificar de onde partiram as tentativas de acesso.

## Ambiente
Sistema analisado: Windows 7
Máquina utilizada nos testes: Kali Linux
Rede: 192.168.56.0/24
IP do Windows: 192.168.56.10
IP do Kali: 192.168.56.20
Evento identificado
Event ID: 4625
Tipo: Falha de autenticação
Usuário alvo: Thiago
Logon Type: 3
Authentication Package: NTLM
Workstation: KALI
IP de origem: 192.168.56.20
## Análise

O Event ID 4625 é registrado quando uma tentativa de autenticação falha.

Durante os testes, as tentativas partiram da máquina Kali Linux, utilizando o endereço IP 192.168.56.20.

O Logon Type 3 indica que a tentativa de logon ocorreu através da rede.

O evento também mostra o uso do NTLM como pacote de autenticação.

Foram identificadas várias falhas de autenticação durante os testes realizados no laboratório.

O que foi observado nos testes

Antes da tentativa de autenticação, foi verificado que a porta TCP 445 do Windows estava acessível a partir do Kali Linux.

Em seguida, foi realizada uma tentativa de acesso ao compartilhamento SMB utilizando o usuário Thiago. A autenticação não foi aceita pelo Windows.

Após a tentativa, o Visualizador de Eventos registrou o Event ID 4625, permitindo relacionar a tentativa realizada no Kali com a falha registrada no Windows.

Interpretação dos códigos

O evento apresentou os seguintes códigos:

Status: 0xc000006d
SubStatus: 0xc000006a

Esses códigos indicam uma falha na autenticação, sendo o 0xc000006a relacionado a uma senha incorreta.

Isso confirma que a tentativa realizada durante o teste não conseguiu concluir a autenticação.

## Evidências

### Evidência 01 - Detalhes do evento

![Evidência 01](evidencias/evidence-01-event-4625-details.png)

Mostra os principais campos do evento 4625, incluindo o usuário, Logon Type, NTLM e a máquina de origem.

### Evidência 02 - Origem da tentativa

![Evidência 02](evidencias/evidence-02-event-4625-source.png)

Mostra o endereço IP de origem (192.168.56.20) e a porta utilizada na tentativa.

### Evidência 03 - Repetição das falhas

![Evidência 03](evidencias/evidence-03-multiple-4625-events.png)

Mostra vários eventos 4625 registrados em sequência durante os testes.
### Evidência 04 - Detalhes da tentativa mais recente

![Evidência 04](evidencias/evidence-04-event-4625-details.png)

Mostra os detalhes da tentativa mais recente, incluindo o usuário, Logon Type 3, NTLM, KALI e o IP de origem.

## Resultado

Os eventos analisados mostram tentativas de autenticação malsucedidas originadas da máquina Kali Linux contra o Windows 7.

Até este ponto, as evidências confirmam falhas de autenticação, mas ainda não permitem afirmar que houve um ataque de força bruta.

## O que poderia ser feito

- Acompanhar a quantidade de eventos 4625 em um determinado período.
- Comparar os horários das falhas e o IP de origem.
- Verificar se várias tentativas estão sendo direcionadas ao mesmo usuário.
- Criar alertas para um número elevado de falhas consecutivas.
- Avaliar políticas de bloqueio de conta.
