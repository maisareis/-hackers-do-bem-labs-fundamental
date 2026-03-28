# Atividade 11.1 – Explorando a Tabela ARP no Kali Linux

## Objetivo
Explorar o comportamento da tabela ARP no Kali Linux, observando como ela é populada automaticamente durante conexões de rede e como pode ser limpa manualmente.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **ARP** (Address Resolution Protocol) é o protocolo responsável por mapear endereços IP (camada 3) a endereços MAC (camada 2) em redes locais. Quando um dispositivo precisa se comunicar com outro na mesma rede, ele consulta a **tabela ARP** (cache ARP) para descobrir o endereço MAC correspondente ao IP de destino.

A tabela ARP é dinâmica — ela é populada automaticamente quando conexões são estabelecidas e suas entradas expiram após um período sem uso.

| Campo da tabela ARP | Descrição |
|--------------------|-----------|
| **Endereço IP / hostname** | IP do dispositivo mapeado |
| **Endereço MAC** | Endereço físico correspondente |
| **[ether]** | Tipo de protocolo de camada de enlace |
| **Interface** | Interface de rede usada para o mapeamento |

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Mozilla Firefox disponível

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Visualizar a tabela ARP atual
```bash
arp -a
```
A saída exibe os mapeamentos IP → MAC conhecidos pelo sistema no momento.

### 3. Limpar a tabela ARP
```bash
ip -s -s neigh flush all
```
O comando remove todas as entradas da tabela ARP. A flag `-s -s` exibe as entradas que foram removidas.

### 4. Confirmar que a tabela foi reduzida
```bash
arp -a
```
A tabela estará vazia ou com menos entradas do que antes.

### 5. Gerar tráfego de rede
Abrir o Mozilla Firefox e acessar:
```
https://www.google.com
```

### 6. Verificar a tabela ARP atualizada
```bash
arp -a
```
A entrada do gateway padrão voltará a aparecer na tabela, pois o tráfego web gerou uma nova resolução ARP.

---

## Explicação dos Comandos

| Comando | Descrição |
|---------|-----------|
| `arp -a` | Exibe a tabela ARP atual |
| `ip -s -s neigh flush all` | Limpa todas as entradas da tabela ARP |

---

## Aprendizado
- A tabela ARP é populada automaticamente sempre que o sistema precisa se comunicar com outro dispositivo na rede local
- Limpar a tabela ARP não afeta a conectividade — ela é repovoada na próxima comunicação
- Analisar a tabela ARP é útil para identificar dispositivos ativos na rede e detectar anomalias como ARP Poisoning
- O gateway padrão sempre aparece na tabela ARP quando há tráfego de saída para a internet
