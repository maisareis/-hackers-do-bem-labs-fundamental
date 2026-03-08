# Atividade 2.7 – Análise de Logs com Logcheck

## Objetivo
Utilizar o Logcheck para analisar eventos de sistema no Kali Linux como controle detectivo de segurança.

## Conceito

### Controle Detectivo
Um **controle detectivo** identifica eventos de segurança que já ocorreram ou estão ocorrendo. A análise de logs é um dos principais controles detectivos em segurança da informação.

### Logcheck
O **Logcheck** é uma ferramenta para análise automatizada de logs em sistemas Linux. Identifica e reporta padrões incomuns ou potencialmente preocupantes nas mensagens de log, como tentativas de acesso não autorizado e falhas de autenticação.

---

## Modos de Filtragem

| Modo | Descrição |
|------|-----------|
| **server** | Nível padrão para servidores com diferentes daemons |
| **paranoid** | Máquinas de alta segurança — mais verboso |
| **workstation** | Máquinas com usuários — filtra mensagens rotineiras |

---

## Passo a Passo

### 1. Executar o Logcheck (modo padrão)
```bash
sudo -u logcheck logcheck -o -t
```

**Parâmetros:**
- `-u logcheck` → executar como usuário logcheck
- `-o` → modo online (analisa logs em tempo real)
- `-t` → saída em texto simples

**Saída:**
```
Security Events for sudo
=-=-=-=-=-=-=-=-=-=-=-=-
sudo: pam_unix(sudo-i:session): session closed for user root

System Events
=-=-=-=-=-=-=
dhclient[495]: XMT: Solicit on eth0, interval 108860ms.
xrdp-chansrv[1991]: [ERROR] clipboard_event_selection_request: unknown target text/plain;charset=utf-8
```

### 2. Editar a configuração
```bash
sudo -i
nano /etc/logcheck/logcheck.conf
```

**Alterações:**
```bash
REPORTLEVEL="workstation"
SENDMAILTO="umemail@gmail.com"
MAILASATTACH=1
```

### 3. Executar novamente e comparar
```bash
exit
sudo -u logcheck logcheck -o -t
```

---

## Interpretando os Eventos

### Security Events

| Evento | Significado |
|--------|-------------|
| `pam_unix(sudo-i:session): session opened for user root` | Sessão sudo aberta — alguém virou root |
| `pam_unix(sudo-i:session): session closed for user root` | Sessão sudo encerrada |

### System Events

| Evento | Significado |
|--------|-------------|
| `dhclient: XMT: Solicit on eth0` | Cliente DHCP buscando endereço IP |
| `xrdp-chansrv: clipboard_event_selection_request` | Erro na área de transferência do RDP |

---

## Arquivo de Configuração

```bash
/etc/logcheck/logcheck.conf
```

| Parâmetro | Descrição |
|-----------|-----------|
| `REPORTLEVEL` | Nível de filtragem (server/paranoid/workstation) |
| `SENDMAILTO` | E-mail para receber os relatórios |
| `MAILASATTACH` | Enviar como anexo (0=não, 1=sim, 2=gzip) |

---

## Aprendizado
- Logs registram todas as atividades relevantes do sistema
- O Logcheck filtra eventos relevantes e descarta os rotineiros
- Eventos de `sudo` são especialmente importantes para auditoria de segurança
- O modo `workstation` é menos verboso que o `server` ou `paranoid`
- Análise regular de logs é fundamental para detectar incidentes de segurança
