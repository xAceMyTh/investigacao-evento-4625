# Investigação de Eventos 4625 e 4624 no Windows

## Objetivo

O objetivo deste projeto foi investigar eventos de autenticação do Windows em um ambiente de laboratório, correlacionando tentativas de login malsucedidas com uma autenticação posterior bem-sucedida.

A análise foi realizada a partir dos Event IDs `4625` e `4624`, observando informações como usuário, endereço IP de origem, tipo de logon, protocolo de autenticação e sequência temporal dos eventos.

## Ambiente do laboratório

- **Sistema analisado:** Windows 7
- **Máquina de origem:** Kali Linux
- **Rede do laboratório:** `192.168.56.0/24`
- **IP do Windows:** `192.168.56.10`
- **IP do Kali:** `192.168.56.20`
- **Serviço utilizado:** SMB
- **Porta:** TCP `445`

Todo o procedimento foi realizado em um ambiente controlado para fins de estudo e investigação.

## Cenário da investigação

Foram realizadas tentativas de autenticação SMB a partir da máquina Kali Linux contra o Windows.

As primeiras tentativas utilizaram credenciais incorretas e geraram eventos `4625`, indicando falha de autenticação.

Posteriormente, uma autenticação com a senha correta foi aceita pelo Windows e gerou o evento `4624`.

A correlação desses registros permitiu acompanhar a sequência entre as tentativas malsucedidas e o logon bem-sucedido.

## Fluxo observado

```
Kali Linux
192.168.56.20
        ↓
Tentativas de autenticação SMB
        ↓
Event ID 4625
Falha de autenticação
        ↓
Várias tentativas consecutivas
        ↓
Credencial correta
        ↓
Event ID 4624

Logon bem-sucedido
        ↓
Acesso ao compartilhamento SMB
```
## Análise do Event ID 4625

O Event ID `4625` é gerado quando ocorre uma falha de autenticação no Windows.

Durante o laboratório, foram registradas várias tentativas malsucedidas de acesso SMB utilizando o usuário `Thiago`.

Os principais dados observados foram:

- **Event ID:** `4625`
- **Usuário:** `Thiago`
- **Logon Type:** `3`
- **Authentication Package:** `NTLM`
- **Workstation:** `KALI`
- **IP de origem:** `192.168.56.20`

O `Logon Type 3` indica que a tentativa de autenticação ocorreu através da rede.

O evento também registrou os seguintes códigos:

- **Status:** `0xc000006d`
- **SubStatus:** `0xc000006a`

O código `0xc000006a` está relacionado a uma tentativa de autenticação utilizando senha incorreta.

### Evidência - Falha de autenticação

![Detalhes do Event ID 4625](evidencias/evidence-01-event-4625-details.png)

A evidência mostra os principais campos do evento, permitindo identificar o usuário alvo, o tipo de logon, o protocolo de autenticação e a origem da tentativa.

## Sequência de tentativas

Durante a investigação, foram observadas várias falhas de autenticação em sequência antes do logon bem-sucedido.

```
18:35:59 - Event ID 4625
18:36:06 - Event ID 4625
18:36:11 - Event ID 4625
18:36:15 - Event ID 4625
18:36:19 - Event ID 4625
18:36:29 - Event ID 4624
```

## Análise do Event ID 4624

Após as tentativas malsucedidas, uma autenticação utilizando a credencial correta foi aceita pelo Windows.

O Event ID `4624` indica que o processo de autenticação foi concluído com sucesso.

Os principais dados observados foram:

- **Event ID:** `4624`
- **Usuário:** `Thiago`
- **Logon Type:** `3`
- **Authentication Package:** `NTLM`
- **Workstation:** `KALI`
- **IP de origem:** `192.168.56.20`
- **Porta de origem:** `60410`
- **NTLM:** `NTLM V2`

Assim como nos eventos `4625`, o `Logon Type 3` indica uma autenticação realizada através da rede.

A origem registrada também corresponde à máquina Kali Linux utilizada durante o laboratório.

### Evidência - Logon bem-sucedido

![Detalhes do Event ID 4624](evidencias/evidence-02-event-4624-details.png)

A evidência mostra que a autenticação foi aceita pelo Windows e permite correlacionar o evento `4624` com as tentativas anteriores originadas da mesma máquina.

## Validação do serviço SMB

Após confirmar a autenticação, foi verificado o serviço utilizado durante os testes.

Primeiro, a porta TCP `445` foi analisada com o Nmap:

```
nmap -p 445 192.168.56.10
```

## MITRE ATT&CK

O comportamento observado durante o laboratório pode ser relacionado a técnicas presentes no MITRE ATT&CK.

### T1078 - Valid Accounts

O evento `4624` confirmou uma autenticação bem-sucedida utilizando uma conta válida.

Em um ambiente real, o uso de credenciais válidas por uma origem não esperada poderia exigir investigação para verificar possível comprometimento da conta.

### T1110 - Brute Force

A sequência de vários eventos `4625` antes de um evento `4624` pode ser um comportamento relevante durante a investigação de tentativas de adivinhação de credenciais.

Neste laboratório, porém, as tentativas foram realizadas manualmente e de forma controlada. Portanto, os eventos observados não são suficientes para classificar o caso como um ataque de força bruta real.

## Conclusão

A investigação permitiu correlacionar várias falhas de autenticação registradas pelo Event ID `4625` com um logon posteriormente aceito pelo Windows através do Event ID `4624`.

Os eventos apresentaram a mesma origem, `192.168.56.20`, correspondente à máquina Kali Linux utilizada no laboratório, além de `Logon Type 3` e autenticação NTLM.

Após o logon bem-sucedido, foi confirmada a disponibilidade do serviço SMB na porta TCP `445` e realizado o acesso ao compartilhamento `Users`.

O laboratório demonstrou como eventos de autenticação do Windows podem ser utilizados para reconstruir uma sequência de atividade e identificar informações importantes como usuário, origem da conexão, tipo de logon e resultado da autenticação.

Todo o procedimento foi realizado em ambiente controlado.

## Ações recomendadas em um ambiente real

Caso uma sequência semelhante fosse identificada em um ambiente corporativo, algumas ações de investigação poderiam incluir:

- Verificar a quantidade de eventos `4625` em um intervalo curto de tempo
- Correlacionar falhas e sucessos de autenticação para o mesmo usuário
- Investigar o endereço IP de origem
- Verificar se o acesso ocorreu a partir de um equipamento esperado
- Analisar outros eventos relacionados à conta
- Revisar acessos a compartilhamentos SMB
- Avaliar políticas de bloqueio de conta
- Criar alertas para múltiplas falhas seguidas de autenticação bem-sucedida

## Análises detalhadas

As etapas completas da investigação estão documentadas nos arquivos abaixo:

- [Análise do Event ID 4625](01-analysis.md)
- [Análise do Event ID 4624 e SMB](02-analysis-4624.md)
