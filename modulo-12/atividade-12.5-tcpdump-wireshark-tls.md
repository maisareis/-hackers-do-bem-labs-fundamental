# Atividade 12.5 – Analisando Pacotes TLS com tcpdump e Wireshark

## Objetivo
Capturar tráfego TLS de um site específico com tcpdump, salvar em arquivo .pcap e analisar os pacotes no Wireshark para identificar a versão do protocolo TLS utilizada.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **tcpdump** é uma ferramenta de captura de pacotes via linha de comando, ideal para capturas automatizadas e análises em servidores sem interface gráfica. Salva as capturas no formato **PCAP** (Packet Capture), compatível com o Wireshark.

O **TLS** (Transport Layer Security) é o protocolo de segurança que protege as conexões HTTPS. As versões mais relevantes são:

| Versão | Status |
|--------|--------|
| TLS 1.0 / 1.1 | ❌ Obsoletas — vulnerabilidades conhecidas |
| **TLS 1.2** | ✅ Amplamente suportado |
| **TLS 1.3** | ✅ Mais seguro e eficiente — padrão atual |

---

## Pré-requisitos
- Kali Linux com tcpdump e Wireshark instalados
- Mozilla Firefox disponível

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Descobrir o IP do site alvo
```bash
nslookup gov.br
```
Anotar o endereço IPv4 retornado.

### 3. Preparar o comando tcpdump (não executar ainda)
```bash
tcpdump -i eth0 -s 0 host [IP_DO_SITE] and port 443 -w capture.pcap
```

### Parâmetros do tcpdump

| Parâmetro | Descrição |
|-----------|-----------|
| `-i eth0` | Interface de captura |
| `-s 0` | Captura o pacote completo (sem truncamento) |
| `host [IP]` | Filtra por IP de origem/destino |
| `and port 443` | Filtra pela porta 443 (HTTPS) |
| `-w capture.pcap` | Salva em arquivo PCAP |

### 4. Preparar o Firefox
Abrir o Mozilla Firefox → digitar `gov.br` na barra de endereços, mas **não pressionar Enter ainda**.

### 5. Iniciar a captura
Voltar ao Terminal → pressionar Enter para iniciar o tcpdump.

### 6. Acessar o site
Voltar ao Firefox → pressionar Enter → aguardar o carregamento completo → fechar o Firefox.

### 7. Parar a captura
No Terminal, pressionar `Ctrl+C`.

Resultado esperado:
```
X packets captured
X packets received by filter
0 packets dropped by kernel
```

### 8. Confirmar o arquivo salvo
```bash
ls
```
O arquivo `capture.pcap` deve aparecer na listagem.

### 9. Abrir o Wireshark
**Aplicativos** → **09 – Descoberta** → **wireshark**.

### 10. Abrir o arquivo de captura
No Wireshark: **Arquivo** → **Abrir** → selecionar `capture.pcap` → **Abrir**.

### 11. Aplicar filtro TLS
No campo de filtro, digitar:
```
tls
```

### 12. Verificar a versão do TLS
Analisar a coluna **Protocol** e os detalhes dos pacotes — confirmar que a versão é **TLSv1.2** ou **TLSv1.3**.

### 13. Fechar e limpar
Fechar o Wireshark → no Terminal:
```bash
rm capture.pcap
```

---

## Fluxo da atividade

```
nslookup → IP do site
    ↓
tcpdump → captura pacotes do IP na porta 443 → capture.pcap
    ↓
Wireshark → abre capture.pcap → filtro "tls" → versão TLS visível
```

---

## Aprendizado
- O tcpdump é ideal para capturas em servidores — não requer interface gráfica e pode ser automatizado
- O formato PCAP é o padrão da indústria para capturas de rede — compatível com Wireshark, Zeek, Suricata e outros
- Identificar a versão TLS de um site é importante para auditoria de segurança — sites que usam TLS 1.0/1.1 precisam ser atualizados
- Combinar tcpdump (captura) com Wireshark (análise) é um workflow clássico de análise de tráfego de rede
