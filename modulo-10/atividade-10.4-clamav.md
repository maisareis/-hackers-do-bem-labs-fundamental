# Atividade 10.4 – Escaneando Vírus com ClamAV no Kali Linux

## Objetivo
Utilizar o antivírus ClamAV no Kali Linux para atualizar a base de assinaturas de vírus, escanear o diretório home em busca de malware e explorar os parâmetros de configuração da ferramenta.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **ClamAV** é um antivírus de código aberto projetado para sistemas Linux, capaz de detectar vírus, malware e outras ameaças. Sua detecção é baseada em assinaturas — padrões de código malicioso conhecidos armazenados em um banco de dados atualizado pela comunidade.

O **freshclam** é o utilitário responsável por atualizar o banco de dados de assinaturas do ClamAV, garantindo que novas ameaças sejam detectadas.

O **clamscan** é a ferramenta de linha de comando para escanear arquivos e diretórios em busca de malware.

> Vírus e malware podem transitar por máquinas Linux mesmo sem infectá-las — por exemplo, arquivos recebidos por e-mail destinados a sistemas Windows. Por isso, antivírus em Linux é importante especialmente em servidores de arquivo e e-mail.

---

## Pré-requisitos
- Kali Linux com ClamAV instalado
- Conexão com a internet para atualização das assinaturas

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Verificar a versão do ClamAV instalada
```bash
clamscan --version
```
Resultado esperado:
```
ClamAV 1.4.3/27833/Thu Nov 27 06:13:22 2025
```

### 3. Parar o serviço de atualização automática
```bash
systemctl stop clamav-freshclam.service
```

### 4. Atualizar o banco de dados de assinaturas
```bash
freshclam
```
O comando baixa os patches mais recentes do banco de dados. Resultado esperado ao finalizar:
```
daily.cld updated (version: 27841, sigs: 2077269, ...)
main.cvd database is up-to-date
bytecode.cld database is up-to-date
```

### 5. Escanear o diretório home em busca de malware
```bash
clamscan -r /home
```
> Este processo pode demorar até 15 minutos. O parâmetro `-r` indica escaneamento recursivo.

Resultado esperado ao finalizar (SCAN SUMMARY):
```
----------- SCAN SUMMARY -----------
Known viruses: 8708962
Engine version: 1.4.3
Scanned directories: 1015
Scanned files: 6128
Infected files: 0
Data scanned: 1319.81 MB
Time: 583.507 sec (9 m 43 s)
```

### Parâmetros do clamscan

| Parâmetro | Descrição |
|-----------|-----------|
| `-r` | Escaneamento recursivo em subdiretórios |
| `--remove` | Remove arquivos infectados automaticamente |
| `--log=<arquivo>` | Salva o resultado em um arquivo de log |
| `--infected` | Exibe apenas arquivos infectados |

### Campos do SCAN SUMMARY

| Campo | Descrição |
|-------|-----------|
| **Known viruses** | Total de assinaturas carregadas |
| **Scanned directories** | Quantidade de diretórios verificados |
| **Scanned files** | Quantidade de arquivos verificados |
| **Infected files** | Arquivos com malware detectado |
| **Data scanned** | Volume de dados verificado |
| **Time** | Tempo total do escaneamento |

### 6. Verificar a configuração do ClamAV
```bash
clamconf
```
O comando exibe os parâmetros de configuração, incluindo tamanho máximo de arquivo, configurações de log, diretório do banco de dados e informações da plataforma.

Fechar o Terminal.

---

## Aprendizado
- O ClamAV usa detecção baseada em assinaturas — manter o banco de dados atualizado com `freshclam` é fundamental para detectar ameaças recentes
- O `clamconf` permite auditar a configuração atual do antivírus, verificando parâmetros como `MaxFileSize` e localização dos logs
- O escaneamento recursivo com `-r` verifica todos os subdiretórios — essencial para varreduras completas
- Linux não é imune a malware — servidores de arquivo que armazenam documentos para usuários Windows devem ter antivírus para evitar propagação de ameaças
