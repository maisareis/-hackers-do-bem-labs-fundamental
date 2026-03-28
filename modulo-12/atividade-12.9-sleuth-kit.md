# Atividade 12.9 – Explorando The Sleuth Kit (TSK) no Kali Linux

## Objetivo
Utilizar as ferramentas do The Sleuth Kit (fls e ils) para listar diretórios, arquivos e metadados de inodes na partição principal do Kali Linux.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **The Sleuth Kit (TSK)** é uma suíte de ferramentas de código aberto para forense digital. Permite examinar sistemas de arquivos em nível de bloco, recuperar arquivos apagados e analisar evidências digitais sem depender do sistema operacional da máquina analisada.

### Conceito de Inode

Um **inode** (index node) é uma estrutura de dados fundamental nos sistemas de arquivos Linux/Unix. Cada arquivo e diretório possui um inode único que armazena:
- Permissões e tipo do arquivo
- Proprietário (UID/GID)
- Timestamps (acesso, modificação, criação)
- Tamanho
- Localização dos blocos de dados no disco

O inode **não armazena o nome do arquivo** — o nome é armazenado no diretório pai que aponta para o inode.

---

## Pré-requisitos
- Kali Linux com The Sleuth Kit instalado

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Identificar as partições disponíveis
```bash
fdisk -l
```
Identificar a partição principal do sistema (`/dev/nvme0n1p1`).

### 3. Listar arquivos e diretórios com fls
```bash
fls /dev/nvme0n1p1
```

### Interpretação da saída do fls

| Prefixo | Significado |
|---------|-------------|
| `d/d` | Diretório |
| `r/r` | Arquivo regular |
| `l/l` | Link simbólico |
| `V/V` | Entrada virtual (ex: arquivos órfãos) |

Exemplo de saída:
```
d/d 524461:    home      ← Diretório "home" no inode 524461
l/l 13:        bin       ← Link simbólico "bin" no inode 13
V/V 1433601:   $OrphanFiles ← Arquivos sem entrada de diretório
```

### 4. Listar metadados de inode com ils
```bash
ils /dev/nvme0n1p1 2
```

### Campos da saída do ils

| Campo | Descrição |
|-------|-----------|
| `st_ino` | Número do inode |
| `st_alloc` | Alocação de espaço |
| `st_uid` | ID do usuário proprietário |
| `st_gid` | ID do grupo proprietário |
| `st_mtime` | Timestamp de modificação |
| `st_atime` | Timestamp de acesso |
| `st_ctime` | Timestamp de mudança de metadados |
| `st_crtime` | Timestamp de criação (se suportado) |
| `st_mode` | Permissões e tipo do arquivo |
| `st_nlink` | Número de links para o inode |
| `st_size` | Tamanho do arquivo em bytes |

### 5. Converter o timestamp Unix para data legível
```bash
date -d @[TIMESTAMP]
```
Substituir `[TIMESTAMP]` pelo valor `start_time` exibido na saída do `ils`.

---

## Ferramentas do The Sleuth Kit

| Ferramenta | Função |
|-----------|--------|
| `fls` | Lista arquivos e diretórios de uma partição |
| `ils` | Lista metadados de inodes |
| `icat` | Extrai o conteúdo de um arquivo pelo número de inode |
| `fsstat` | Exibe estatísticas do sistema de arquivos |
| `blkstat` | Informações sobre blocos de dados |
| `mmls` | Lista a tabela de partições |

---

## Aprendizado
- O TSK opera diretamente na partição — não depende do sistema de arquivos montado, sendo ideal para análise forense de discos suspeitos
- Arquivos "apagados" continuam presentes no disco até que seus blocos sejam sobrescritos — o TSK pode recuperá-los pelo inode
- Os timestamps do inode são mais confiáveis que os metadados internos do arquivo para análise forense
- `$OrphanFiles` no fls indica arquivos que existem no disco mas não têm entrada de diretório — possível evidência de deleção recente
