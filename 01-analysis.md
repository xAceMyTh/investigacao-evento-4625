# Análise do Event ID 4625 - Falha de Autenticação

## Objetivo

Investigar falhas de autenticação registradas no Windows e identificar a origem das tentativas de acesso.

## Ambiente

- **Sistema analisado:** Windows 7
- **Máquina utilizada nos testes:** Kali Linux
- **Rede:** `192.168.56.0/24`
- **IP do Windows:** `192.168.56.10`
- **IP do Kali:** `192.168.56.20`

## Evento identificado

- **Event ID:** `4625`
- **Tipo:** Falha de autenticação
- **Usuário alvo:** `Thiago`
- **Logon Type:** `3`
- **Authentication Package:** `NTLM`
- **Workstation:** `KALI`
- **IP de origem:** `192.168.56.20`

## Análise

O Event ID `4625` é registrado quando uma tentativa de autenticação falha no Windows.

Durante os testes, as tentativas partiram da máquina Kali Linux utilizando o endereço IP `192.168.56.20`.

O `Logon Type 3` indica que a tentativa de autenticação ocorreu através da rede.

O evento também mostra o uso do `NTLM` como pacote de autenticação.

Foram identificadas várias falhas de autenticação durante os testes realizados no laboratório.

## O que foi observado nos testes

Antes da tentativa de autenticação, foi verificado que a porta TCP `445` do Windows estava acessível a partir do Kali Linux.

Em seguida, foi realizada uma tentativa de acesso ao compartilhamento SMB utilizando o usuário `Thiago`.

Como uma senha incorreta foi utilizada, a autenticação não foi aceita pelo Windows.

Após a tentativa, o Visualizador de Eventos registrou o Event ID `4625`, permitindo relacionar a atividade realizada no Kali Linux com a falha registrada no Windows.

## Interpretação dos códigos

O evento apresentou os seguintes códigos:

- **Status:** `0xc000006d`
- **SubStatus:** `0xc000006a`

O código `0xc000006d` indica falha no processo de autenticação.

O código `0xc000006a` está relacionado ao uso de uma senha incorreta.

Esses dados são consistentes com a tentativa realizada durante o laboratório.

## Evidências

### Evidência 01 - Detalhes do evento

![Evidência 01](evidencias/evidence-01-event-4625-details.png)

Mostra os principais campos do evento `4625`, incluindo usuário, Logon Type, NTLM e máquina de origem.

### Evidência 02 - Origem da tentativa

![Evidência 02](evidencias/evidence-02-event-4625-source.png)

Mostra o endereço IP de origem `192.168.56.20` e informações relacionadas à conexão.

### Evidência 03 - Repetição das falhas

![Evidência 03](evidencias/evidence-03-multiple-4625-events.png)

Mostra vários eventos `4625` registrados em sequência durante os testes.

### Evidência 04 - Detalhes de outra tentativa

![Evidência 04](evidencias/evidence-04-event-4625-details.png)

Mostra os detalhes de outra tentativa, incluindo usuário, `Logon Type 3`, NTLM, workstation `KALI` e IP de origem.

## Resultado

Os eventos analisados mostram tentativas de autenticação malsucedidas originadas da máquina Kali Linux contra o Windows 7.

Neste ponto da investigação, as evidências confirmam múltiplas falhas de autenticação, mas isoladamente não são suficientes para afirmar que ocorreu um ataque de força bruta.

## Possíveis ações em um ambiente real

- Acompanhar a quantidade de eventos `4625` em determinado período
- Comparar os horários das falhas e o endereço IP de origem
- Verificar se várias tentativas estão sendo direcionadas ao mesmo usuário
- Criar alertas para um número elevado de falhas consecutivas
- Avaliar políticas de bloqueio de conta

Todo o procedimento foi realizado em ambiente de laboratório controlado.
