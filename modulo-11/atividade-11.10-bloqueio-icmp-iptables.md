# Atividade 11.10 – Bloqueando Pacotes ICMP com iptables no Kali Linux

## Objetivo
Bloquear respostas a pacotes ICMP (ping) no Kali Linux usando iptables, demonstrar o efeito a partir do Windows Server 2022 (cliente), e remover a regra ao final.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **ICMP** (Internet Control Message Protocol) é usado para diagnóstico de rede, sendo o protocolo por trás do comando `ping`. O tipo **echo-request** (tipo 8) é enviado pelo remetente, e o **echo-reply** (tipo 0) é a resposta.

Ao bloquear os **echo-requests** recebidos no Kali Linux, os pings enviados pelo Windows Server não recebem resposta — o Kali simplesmente descarta os pacotes silenciosamente.

Bloquear ICMP é uma prática comum em servidores de produção para dificultar o reconhecimento de rede por atacantes.

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Windows Server 2022 (cliente) com acesso ao Command Prompt
- Ambos na mesma rede local

---

## Passo a Passo

### 1. Acessar o Windows Server 2022 (cliente)
Conectar via RDP ao servidor cliente.

### 2. Abrir o Command Prompt
Na barra de pesquisa, escrever `cmd` e abrir.

### 3. Verificar o IP do Windows Server
```cmd
ipconfig
```
Anotar o IPv4 do servidor cliente.

### 4. Minimizar a sessão RDP do Windows Server

### 5. Acessar o Kali Linux via RDP
Conectar via RDP ao Kali Linux.

### 6. Acessar como superusuário
```bash
sudo -i
```

### 7. Confirmar o IP do Kali Linux
```bash
ifconfig
```
Verificar o IP da interface `eth0`.

### 8. Testar ping do Windows → Kali (antes do bloqueio)
Maximizar o Windows Server → no CMD:
```cmd
ping [IP_DO_KALI]
```
O ping deve retornar respostas com sucesso.

### 9. Adicionar regra de bloqueio ICMP no Kali
Maximizar o Kali → no Terminal:
```bash
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```

### Parâmetros do comando

| Parâmetro | Descrição |
|-----------|-----------|
| `-A INPUT` | Adiciona à chain de entrada |
| `-p icmp` | Protocolo ICMP |
| `--icmp-type echo-request` | Tipo 8 — requisição de ping |
| `-j DROP` | Descartar silenciosamente |

### 10. Testar ping do Windows → Kali (após bloqueio)
Maximizar o Windows Server → no CMD:
```cmd
ping [IP_DO_KALI]
```
Resultado esperado:
```
Request timed out.
Request timed out.
Request timed out.
Request timed out.
Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)
```

### 11. Remover a regra de bloqueio
Maximizar o Kali:
```bash
iptables -D INPUT -p icmp --icmp-type echo-request -j DROP
```

### 12. Confirmar que o ping voltou a funcionar
Maximizar o Windows Server → repetir o ping → as respostas devem retornar normalmente.

Fechar as janelas de ambas as máquinas.

---

## Por que bloquear ICMP?

| Motivo | Descrição |
|--------|-----------|
| **Reconhecimento** | Atacantes usam ping para descobrir hosts ativos |
| **DDoS** | Ping flood pode consumir recursos da rede |
| **Privacidade** | Oculta a existência do host na rede |

---

## Aprendizado
- O DROP descarta o pacote silenciosamente — o remetente não sabe se o host existe ou se foi bloqueado
- Bloquear ICMP não afeta conexões TCP/UDP — apenas o ping e outros diagnósticos ICMP são afetados
- Servidores de produção frequentemente bloqueiam ICMP de fora da rede como medida de segurança perimetral
- As regras iptables são temporárias — são perdidas após reboot sem uso de `iptables-save`
