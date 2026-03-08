# Atividade 3.6 – Explorando ferramentas de detecção de rede no Kali Linux

## Objetivo

Conhecer e utilizar os principais comandos de diagnóstico de rede disponíveis no Linux: `ifconfig`, `ping`, `traceroute` e `netstat`, para analisar o estado da rede e das conexões ativas no sistema.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O diagnóstico de rede é uma habilidade fundamental tanto para administradores de sistemas quanto para profissionais de segurança. As ferramentas nativas do Linux permitem mapear interfaces, testar conectividade, rastrear rotas e inspecionar conexões e serviços em execução.

### Ferramentas utilizadas

| Ferramenta    | Finalidade principal                                               |
|---------------|--------------------------------------------------------------------|
| `ifconfig`    | Exibe configurações das interfaces de rede                         |
| `ping`        | Testa conectividade e descobre o IP de um host/domínio             |
| `traceroute`  | Mapeia os saltos (hops) entre origem e destino                     |
| `netstat -i`  | Estatísticas por interface de rede                                 |
| `netstat -rn` | Tabela de roteamento IP do kernel                                  |
| `netstat -s`  | Estatísticas detalhadas por protocolo (IP, ICMP, TCP, UDP)         |
| `netstat -anptu` | Conexões ativas e serviços em escuta (TCP e UDP)                |

### Campos do `netstat -anptu`

| Campo            | Descrição                                                       |
|------------------|-----------------------------------------------------------------|
| `Proto`          | Protocolo (tcp, tcp6, udp, udp6)                               |
| `Recv-Q`         | Dados na fila de recebimento do socket                         |
| `Send-Q`         | Dados na fila de envio do socket                               |
| `Endereço Local` | IP e porta local da conexão                                    |
| `Endereço Remoto`| IP e porta remota da conexão                                   |
| `Estado`         | Estado da conexão (OUÇA / ESTABELECIDA)                        |
| `PID/Program`    | PID e nome do processo associado ao socket                     |

### Parâmetros do `netstat -anptu`

| Parâmetro | Descrição                                             |
|-----------|-------------------------------------------------------|
| `-a`      | Mostra todas as conexões e sockets em escuta          |
| `-n`      | Exibe IPs e portas em formato numérico                |
| `-p`      | Mostra PID e nome do programa associado               |
| `-t`      | Filtra conexões TCP                                   |
| `-u`      | Filtra conexões UDP                                   |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Verificar as interfaces de rede e seus IPs:
```bash
ifconfig
```
Serão listadas as interfaces `docker0`, `eth0` e `lo` com seus respectivos endereços IPv4, IPv6, MAC e estatísticas de tráfego.

**4.** Testar conectividade e descobrir o IP de um domínio (encerrar com `Ctrl+C`):
```bash
ping www.google.com
```
A saída exibe o IP resolvido, o tamanho dos pacotes ICMP, o TTL e o tempo de resposta (RTT) em milissegundos.

**5.** Rastrear os saltos até um servidor web:
```bash
traceroute -I grancursos.com.br
```
A opção `-I` usa sondas ICMP. Cada linha representa um roteador intermediário (hop) com seu IP e os tempos de resposta dos três pacotes enviados. Saltos com `* * *` indicam roteadores que não respondem a ICMP por política de segurança.

**6.** Verificar estatísticas por interface de rede:
```bash
netstat -i
```
Exibe MTU, pacotes recebidos/enviados com e sem erros, e flags de cada interface.

**7.** Visualizar a tabela de roteamento IP:
```bash
netstat -rn
```
Mostra rotas configuradas no kernel: destino, gateway, máscara e interface associada. A rota `0.0.0.0` com flag `UG` indica o gateway padrão.

**8.** Visualizar estatísticas detalhadas por protocolo:
```bash
netstat -s
```
Exibe contadores para IP, ICMP, TCP e UDP — incluindo pacotes recebidos, enviados, erros, conexões estabelecidas e retransmissões.

**9.** *(Print solicitado)* Listar todas as conexões ativas e serviços em escuta:
```bash
netstat -anptu
```
Mostra os serviços em execução: SSH (porta 22), XRDP (porta 3389), containerd (porta local), e o cliente DHCP nas portas 68/546.

**10.** Fechar o Terminal.

## Aprendizado

- O `ifconfig` é o ponto de partida para qualquer diagnóstico de rede, revelando as interfaces disponíveis e seus endereços.
- O `ping` vai além do teste de conectividade — resolve nomes de domínio em IPs e mede a latência da rede.
- O `traceroute` mapeia a rota completa entre dois hosts, permitindo identificar gargalos, roteadores intermediários e proteções de firewall (saltos com `*`).
- O `netstat -anptu` é essencial para auditar serviços expostos: cada linha representa um socket aberto, com o processo responsável e o estado da conexão.
- Serviços em estado `OUÇA` (LISTEN) são pontos de entrada potenciais em uma análise de superfície de ataque.
