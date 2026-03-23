# Atividade 10.5 – Conhecendo o Suricata HIDS no Kali Linux

## Objetivo
Configurar e executar o Suricata como HIDS (Host-based Intrusion Detection System) no Kali Linux, utilizando regras da comunidade para monitorar o tráfego de rede em busca de intrusões.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **Suricata** é um motor de detecção de intrusão de alta performance, capaz de operar como IDS (Intrusion Detection System), IPS (Intrusion Prevention System) e monitor de segurança de rede. Utiliza regras para identificar padrões maliciosos no tráfego.

Um **HIDS** (Host-based IDS) monitora atividades no próprio host onde está instalado, diferente do NIDS (Network IDS) que monitora o tráfego de uma rede inteira.

As **regras Emerging Threats** são regras desenvolvidas pela comunidade de segurança para detectar exploits, malware e ataques conhecidos.

Os **logs do Suricata** são armazenados em `/var/log/suricata/` e incluem eventos em formato JSON (`eve.json`), alertas rápidos (`fast.log`) e estatísticas (`stats.log`).

---

## Pré-requisitos
- Kali Linux com Suricata instalado
- Conexão com a internet para baixar as regras da comunidade

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Habilitar o serviço do Suricata
```bash
systemctl enable suricata.service
```

### 3. Parar o Suricata para configuração
```bash
systemctl stop suricata.service
```

### 4. Baixar as regras da comunidade
```bash
cd /var/lib/suricata/rules/
wget https://raw.githubusercontent.com/seanlinmt/suricata/master/files/rules/emerging-exploit.rules
cd ~
```

### 5. Editar o arquivo de configuração
```bash
nano /etc/suricata/suricata.yaml
```

Usar `Ctrl+W` para pesquisar o texto:
```
default-rule
```

Substituir o bloco padrão:
```yaml
default-rule-path: /var/lib/suricata/rules

rule-files:
  - suricata.rules
```
Pelo novo bloco com as regras da comunidade:
```yaml
default-rule-path: /var/lib/suricata/rules

rule-files:
  - emerging-exploit.rules
```
Salvar com `Ctrl+X`, `s`, `Enter`.

### 6. Identificar a interface de rede
```bash
ifconfig
```
Identificar a interface de rede ativa (no lab: `eth0`).

### 7. Iniciar o Suricata na interface identificada
```bash
suricata -c /etc/suricata/suricata.yaml -i eth0
```
Resultado esperado:
```
i: suricata: This is Suricata version 8.0.2 RELEASE running in SYSTEM mode
i: threads: Threads created -> W: 2 FM: 1 FR: 1   Engine started.
```

### 8. Abrir um segundo Terminal e verificar os logs
```bash
cd /var/log/suricata
ls
```
Resultado esperado:
```
eve.json  fast.log  stats.log  suricata.log
```

### 9. Encerrar o Suricata
No primeiro Terminal, pressionar `Ctrl+C` para interromper o Suricata. Fechar ambos os terminais.

---

## Arquivos de log do Suricata

| Arquivo | Formato | Conteúdo |
|---------|---------|----------|
| `eve.json` | JSON | Todos os eventos (alertas, flows, DNS, HTTP, etc.) |
| `fast.log` | Texto | Alertas em formato resumido |
| `stats.log` | Texto | Estatísticas de desempenho do motor |
| `suricata.log` | Texto | Logs do serviço (inicialização, erros) |

---

## Comparativo IDS vs IPS

| | IDS | IPS |
|---|---|---|
| **Ação** | Detecta e alerta | Detecta e bloqueia |
| **Posicionamento** | Passivo (cópia do tráfego) | Inline (no caminho do tráfego) |
| **Risco de falso positivo** | Baixo impacto | Pode bloquear tráfego legítimo |
| **Suricata** | ✅ Suporta | ✅ Suporta (modo IPS) |

---

## Aprendizado
- O Suricata pode operar como IDS, IPS ou monitor de segurança — no lab foi usado como IDS passivo
- As regras Emerging Threats são atualizadas pela comunidade de segurança e cobrem exploits e malware recentes
- O `eve.json` é o log mais completo do Suricata e pode ser integrado com SIEMs como o Elastic Stack
- Em produção, o Suricata é frequentemente combinado com ferramentas de visualização como Kibana para análise de eventos em tempo real
