# Atividade 7.8 – Formatação Completa no Kali Linux

## Objetivo
Executar a formatação completa de uma partição no Kali Linux, removendo todos os metadados e sistemas de arquivos.

## Conceito

### Formatação Completa
A **formatação completa** remove todas as estruturas de sistema de arquivos de uma partição, tornando-a disponível para novo uso. O comando `wipefs` apaga as assinaturas de metadados do sistema de arquivos.

---

## ⚠️ Observação Importante
Antes de executar os comandos de zeragem de superblocks, é necessário **parar os arrays RAID ativos**. Caso contrário, os comandos `mdadm --zero-superblock` retornarão erro.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Verificar discos e partições
```bash
fdisk -l
```

### 3. Verificar e parar os arrays RAID ativos
```bash
# Verificar arrays ativos
cat /proc/mdstat

# Parar os arrays
mdadm --stop /dev/md127
mdadm --stop /dev/md126
```
> Os números dos arrays (md126, md127) podem variar. Verificar com `cat /proc/mdstat`.

### 4. Zerar os superblocks RAID
```bash
mdadm --zero-superblock /dev/nvme1n1p1
mdadm --zero-superblock /dev/nvme1n1p2
mdadm --zero-superblock /dev/nvme1n1p3
mdadm --zero-superblock /dev/nvme1n1p4
```

### 5. Deletar partições desnecessárias
```bash
fdisk /dev/nvme1n1
```
Dentro do fdisk:
- `d` → Enter → deleta partição 4
- `d` → Enter → deleta partição 3
- `d` → Enter → deleta partição 2
- `p` → confirma que só a p1 permanece
- `w` → salva e sai

### 6. Atualizar tabela de partições e formatar
```bash
partprobe /dev/nvme1n1
mkfs.ext4 /dev/nvme1n1p1
```

### 7. Aplicar formatação completa com wipefs
```bash
wipefs -a /dev/nvme1n1p1
```

**Saída esperada:**
```
/dev/nvme1n1p1: 2 bytes were erased at offset 0x00000438 (ext4): 53 ef
```

---

## O que o wipefs faz?
- Remove todas as assinaturas de metadados do sistema de arquivos
- Os bytes `53 ef` são o identificador do sistema de arquivos ext4
- O offset `0x00000438` corresponde ao superbloco do ext4

---

## Aprendizado
- Arrays RAID ativos devem ser parados antes de manipular as partições
- `mdadm --zero-superblock` remove metadados RAID das partições
- `wipefs -a` remove assinaturas de sistemas de arquivos
- Nunca executar esses comandos no disco que contém o sistema operacional
