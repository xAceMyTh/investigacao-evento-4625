# Análise de Evento 4625 - Falha de Autenticação

## 1. Objetivo

Investigar eventos de falha de autenticação registrados no Windows e identificar a origem e as características das tentativas de acesso.

## 2. Ambiente

- Sistema analisado: Windows 7
- Máquina de análise: Kali Linux
- Rede: 192.168.56.0/24
- IP do Windows: 192.168.56.10
- IP do Kali: 192.168.56.20

## 3. Evento identificado

- Event ID: 4625
- Tipo: Falha de autenticação
- Usuário alvo: Thiago
- Logon Type: 3
- Authentication Package: NTLM
- Workstation: KALI
- IP de origem: 192.168.56.20

## 4. Análise

O evento 4625 indica que uma tentativa de autenticação falhou.

A origem identificada foi a máquina Kali Linux, através do endereço IP 192.168.56.20.

O Logon Type 3 indica uma tentativa de logon através da rede.

O uso do protocolo NTLM também foi identificado no evento.

Foram observadas múltiplas falhas de autenticação durante os testes controlados realizados no laboratório.

## 5. Evidências

### Evidência 01 - Detalhes do evento

![Event 4625](evidencias/evidence-01-event-4625-details.png)

### Evidência 02 - Origem da tentativa

![Origem](evidencias/evidence-02-event-4625-source.png)

## 6. Conclusão

Os eventos analisados demonstram tentativas de autenticação malsucedidas originadas da máquina Kali Linux contra o Windows 7.

O comportamento foi reproduzido em ambiente de laboratório controlado para fins de estudo e análise de segurança.

## 7. Recomendações

- Monitorar eventos 4625 recorrentes.
- Correlacionar IP de origem, usuário e horário.
- Investigar múltiplas falhas consecutivas de autenticação.
- Avaliar políticas de bloqueio de contas.
- Utilizar mecanismos de monitoramento e alertas para identificar padrões anormais.
