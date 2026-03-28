# Atividade 12.3 – Explorando os Logs no Kali Linux

## Objetivo
Explorar os arquivos de log do Kali Linux em `/var/log/`, utilizar os comandos `cat`, `journalctl` e `grep` para análise de logs, e visualizar logs por categoria com a interface gráfica gnome-logs.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

No Linux, os logs são armazenados em `/var/log/` e gerenciados por dois sistemas principais:

- **rsyslog:** sistema tradicional de logs em arquivos de texto plano
- **systemd-journald:** sistema moderno que armazena logs em formato binário, acessados via `journalctl`

Os arquivos de log são rotacionados automaticamente — versões antigas recebem sufixos numéricos (`.1`, `.2`) ou são comprimidas (`.gz`).

---

## Pré-requisitos
- Kali Linux com acesso via Terminal

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Listar os logs disponíveis
```bash
ls -l /var/log
```

### Principais arquivos de log

| Arquivo/Pasta | Conteúdo |
|--------------|---------|
| `syslog` | Log geral do sistema — eventos de boot, serviços, hardware |
| `auth.log` | Autenticações, logins SSH, sudo |
| `kern.log` | Mensagens do kernel |
| `cron.log` | Tarefas agendadas pelo cron |
| `dpkg.log` | Instalação e remoção de pacotes |
| `boot.log` | Processo de inicialização do sistema |
| `suricata/` | Logs do IDS Suricata |
| `clamav/` | Logs do antivírus ClamAV |
| `xrdp.log` | Logs do servidor RDP |

### 3. Visualizar o conteúdo do syslog
```bash
cat /var/log/syslog
```
O syslog é muito extenso — contém todos os eventos do sistema desde a criação da máquina.

### 4. Explorar os comandos do journalctl

| Comando | Descrição |
|---------|-----------|
| `journalctl -f` | Exibe logs em tempo real |
| `journalctl --since "2023-01-01" --until "2023-07-31"` | Logs de um período |
| `journalctl -u <serviço>` | Logs de um serviço específico |
| `journalctl -k` | Logs do kernel |
| `journalctl -b` | Logs do boot atual |
| `journalctl -xe` | Logs detalhados do registro mais recente |

### 5. Ver logs do boot atual
```bash
journalctl -b
```
Pressionar `Ctrl+C` para sair.

### 6. Filtrar logs com grep
```bash
grep "error" /var/log/syslog
```
O `grep` filtra apenas as linhas que contêm a palavra "error", facilitando a identificação de problemas.

### 7. Sair do superusuário e abrir o gnome-logs
```bash
exit
gnome-logs
```
> A mensagem "obex server init falhou" pode aparecer e pode ser ignorada — é apenas um aviso do serviço Bluetooth.

### 8. Explorar as categorias do gnome-logs

| Categoria | Conteúdo |
|-----------|---------|
| **Importantes** | Eventos marcados como críticos ou de alto impacto |
| **Todos** | Todos os eventos registrados |
| **Aplicativos** | Logs de aplicações em execução |
| **Sistema** | Eventos do sistema operacional e kernel |
| **Segurança** | Autenticações, acessos e eventos de segurança |
| **Hardware** | Detecção e eventos de dispositivos físicos |

### 9. Explorar Aplicativos, Sistema e Segurança
Clicar em cada categoria e explorar os eventos registrados.

Fechar a janela do gnome-logs e o Terminal.

---

## Aprendizado
- `/var/log/auth.log` é o arquivo mais relevante para auditoria de segurança no Linux — registra todas as tentativas de login e uso do sudo
- O `journalctl` é mais poderoso que o `cat` — permite filtros por tempo, serviço e prioridade
- O `grep` é essencial para análise de logs — buscar por "error", "fail" ou "denied" rapidamente identifica problemas
- O gnome-logs oferece uma interface visual para navegar pelos logs sem precisar conhecer os comandos de terminal
