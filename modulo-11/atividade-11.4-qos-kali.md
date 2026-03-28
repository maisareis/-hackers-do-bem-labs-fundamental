# Atividade 11.4 – Configurando QoS no Kali Linux

## Objetivo
Configurar parâmetros de Qualidade de Serviço (QoS) na interface eth0 do Kali Linux utilizando o comando `tc`, priorizando tráfego ICMP, criando filas hierárquicas HTB e limitando a largura de banda com Token Bucket Filter.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **QoS** (Quality of Service) é um conjunto de técnicas para gerenciar o tráfego de rede, garantindo que tipos específicos de tráfego recebam tratamento preferencial em relação a outros.

No Linux, o controle de tráfego é feito pelo comando `tc` (Traffic Control), que gerencia **qdiscs** (disciplinas de fila) e **filtros**.

### Tipos de qdisc utilizados

| qdisc | Nome | Descrição |
|-------|------|-----------|
| `prio` | Priority Queueing | Prioriza tráfego por níveis de prioridade |
| `htb` | Hierarchical Token Bucket | Cria hierarquia de filas com limites de taxa |
| `tbf` | Token Bucket Filter | Limita a taxa de transmissão de uma interface |

---

## Pré-requisitos
- Kali Linux com acesso via Terminal

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Visualizar as interfaces de rede
```bash
ifconfig
```

### 3. Priorizar tráfego ICMP com Priority Queueing
```bash
tc qdisc add dev eth0 root handle 1: prio
tc filter add dev eth0 parent 1: protocol ip prio 1 u32 match ip protocol 1 0xff flowid 1:1
```
Esses comandos criam uma disciplina de filas do tipo `prio` e um filtro que direciona pacotes ICMP (protocolo 1) para a fila de maior prioridade.

### 4. Remover a qdisc existente
```bash
tc qdisc del dev eth0 root
```
O Linux permite apenas uma qdisc raiz por interface. É necessário remover a atual antes de adicionar outra.

### 5. Verificar conectividade
```bash
ping [IP_DO_GATEWAY]
```

### 6. Configurar priorização por IP com Hierarchical Token Bucket
```bash
tc qdisc add dev eth0 root handle 1: htb
tc class add dev eth0 parent 1: classid 1:1 htb rate 10mbit
tc filter add dev eth0 parent 1: protocol ip prio 1 u32 match ip src [IP_ORIGEM] flowid 1:1
```
Esses comandos criam uma hierarquia HTB que limita a 10 Mbps o tráfego proveniente de um IP específico.

### 7. Remover a qdisc novamente (passo 4)
```bash
tc qdisc del dev eth0 root
```

### 8. Limitar a largura de banda da eth0 para 1 Mbps
```bash
tc qdisc add dev eth0 root tbf rate 1mbit burst 10kb latency 50ms
```
Após este comando, a conexão fica visivelmente mais lenta.

### Parâmetros do TBF

| Parâmetro | Valor | Descrição |
|-----------|-------|-----------|
| `rate 1mbit` | 1 Mbps | Taxa máxima de transmissão |
| `burst 10kb` | 10 KB | Tamanho máximo da rajada |
| `latency 50ms` | 50 ms | Atraso máximo na fila |

### 9. Remover a limitação (repetir passo 4)
```bash
tc qdisc del dev eth0 root
```

Fechar o Terminal.

---

## Comparativo das Disciplinas de Fila

| qdisc | Uso típico | Complexidade |
|-------|-----------|--------------|
| `prio` | Priorizar tipos de tráfego | Simples |
| `htb` | Limitar taxa por classe/IP | Média |
| `tbf` | Limitar taxa global da interface | Simples |

---

## Aprendizado
- O `tc` é a ferramenta nativa do Linux para controle de tráfego — sem necessidade de software adicional
- Apenas uma qdisc raiz pode existir por interface — é necessário remover a atual antes de adicionar outra
- O TBF é ideal para simular conexões lentas em testes de aplicação
- O HTB permite criar políticas complexas de QoS com diferentes taxas para diferentes tipos de tráfego
