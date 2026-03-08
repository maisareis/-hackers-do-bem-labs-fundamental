# Atividade 3.2 – Explorando varreduras com o Kali Linux

## Objetivo

Utilizar a ferramenta **Nmap** para realizar varreduras de rede, descobrir hosts ativos, identificar portas abertas, detectar proteções de firewall e analisar pacotes em trânsito.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**. A realização de varreduras em redes sem autorização prévia pode configurar crime conforme a legislação vigente.

## Conceito

O **Nmap** (Network Mapper) é uma ferramenta de código aberto amplamente usada em segurança ofensiva e defensiva para mapear redes, descobrir hosts, identificar serviços em execução e detectar sistemas operacionais. Sua flexibilidade o torna essencial em testes de penetração e auditorias de segurança.

### Principais técnicas de varredura

| Técnica         | Parâmetro       | Descrição                                                              |
|-----------------|-----------------|------------------------------------------------------------------------|
| Ping Scan       | `-sn`           | Descobre hosts ativos sem varrer portas                                |
| TCP ACK Scan    | `-sA`           | Detecta firewalls verificando respostas ACK                            |
| Open Port Scan  | `--open`        | Lista apenas portas com estado aberto                                  |
| Packet Trace    | `--packet-trace`| Exibe detalhes de cada pacote enviado e recebido                       |

### Estados de porta no Nmap

| Estado       | Significado                                                         |
|--------------|---------------------------------------------------------------------|
| `open`       | Porta aceitando conexões — serviço ativo                            |
| `closed`     | Porta acessível, mas sem serviço ativo (responde com RST)           |
| `filtered`   | Firewall bloqueia a resposta — estado inconclusivo                  |
| `unfiltered` | Porta acessível, mas Nmap não consegue determinar se está aberta    |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Verificar as interfaces de rede disponíveis:
```bash
ifconfig
```
Identificar a interface principal (eth0) e sua faixa de rede para uso nas varreduras.

**4.** Varrer a rede para listar hosts ativos (ping scan):
```bash
nmap -sn [REDE]/24
```
O parâmetro `-sn` realiza apenas descoberta de hosts sem varredura de portas.

**5.** Salvar os IPs descobertos em arquivo:
```bash
cd /home/aluno/Documentos/
nmap -sn [REDE]/24 | grep [PREFIXO_IP] | cut -d ' ' -f 5 > ips.txt
cat ips.txt
```
O pipeline combina `grep` para filtrar linhas com IPs e `cut` para extrair o campo do hostname/IP, redirecionando para arquivo.

**6.** *(Print solicitado)* Varrer portas abertas em toda a rede:
```bash
nmap --open [REDE]/24
```
O parâmetro `--open` exibe apenas hosts com portas no estado aberto ou não filtrado. Hosts com todas as portas filtradas (firewall) ou fechadas (RST) aparecem sem portas listadas.

**7.** Detectar hosts com proteção de firewall (TCP ACK Scan):
```bash
nmap -sA [REDE]/24
```

**8.** Analisar pacotes enviados durante a varredura:
```bash
nmap --packet-trace [REDE]/24
```
Exibe cada pacote ARP, TCP ou UDP enviado e recebido, útil para depuração e análise de comportamento da rede.

**9.** Remover o arquivo de IPs gerado e fechar o Terminal:
```bash
rm /home/aluno/Documentos/ips.txt
```

## Aprendizado

- O Nmap oferece diversas técnicas de varredura adaptáveis ao objetivo da análise — desde descoberta passiva de hosts até varreduras detalhadas com rastreamento de pacotes.
- Hosts que respondem com pacotes RST (estado `unfiltered`) estão ativos mas sem serviços nas portas testadas, enquanto hosts com estado `filtered` possuem firewall ativo descartando pacotes.
- O parâmetro `--packet-trace` é valioso para entender o tráfego gerado e validar o comportamento do scanner.
- Salvar resultados em arquivo (`> ips.txt`) permite automatizar etapas subsequentes em scripts de reconhecimento de rede.
- Referência oficial: [https://nmap.org/](https://nmap.org/)
