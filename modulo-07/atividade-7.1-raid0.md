# Atividade 7.1 – Aplicando o Software RAID 0 no Kali Linux

## Objetivo
Implementar o RAID 0 (striping) no Kali Linux utilizando 2 partições de 100MB cada, criando um array de disco virtual com maior velocidade de leitura e escrita.

## Conceito
O **RAID 0** (striping) divide os dados entre dois ou mais discos simultaneamente, aumentando a performance de leitura e escrita.

⚠️ **Atenção:** O RAID 0 **não oferece redundância**. Se uma partição falhar, todos os dados são perdidos.

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Disco secundário disponível: `/dev/nvme1n1`

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Verificar os discos disponíveis
```bash
fdisk -l
```
> Identifica os discos presentes. O disco principal contém o SO e o secundário `/dev/nvme1n1` é usado no lab.

### 3. Criar as partições no disco secundário
```bash
fdisk /dev/nvme1n1
```
Dentro do fdisk:
- `n` → nova partição
- Enter → tipo primário (p)
- Enter → número 1
- Enter → setor inicial padrão
- `+100M` → tamanho de 100MB

Repetir para criar a segunda partição (`nvme1n1p2`).

Confirmar e salvar:
- `p` → visualizar partições
- `w` → salvar e sair

### 4. Atualizar a tabela de partições
```bash
partprobe /dev/nvme1n1
```

### 5. Criar o array RAID 0
```bash
mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/nvme1n1p1 /dev/nvme1n1p2
```
> O parâmetro `--level=0` define o tipo RAID 0.

### 6. Verificar o dispositivo criado
```bash
ls -l /dev/md0
```

### 7. Formatar o array com ext4
```bash
mkfs.ext4 /dev/md0
```

### 8. Criar ponto de montagem
```bash
mkdir /mnt/raid0
```

### 9. Montar o array
```bash
mount /dev/md0 /mnt/raid0
```

### 10. Verificar o resultado
```bash
df -h
```
> O array `/dev/md0` aparecerá montado em `/mnt/raid0` com ~179MB disponíveis.

```bash
fdisk -l
```
> Confirma a criação do disco RAID `/dev/md0` com 196 MiB.

---

## Resultado Esperado
```
Disk /dev/md0: 196 MiB, 205520896 bytes, 401408 sectors
```
> O espaço total não é exatamente 200MB (100+100) pois alguns megabytes são reservados para uso interno do sistema RAID.

---

## Explicação dos Comandos Principais

| Comando | Descrição |
|--------|-----------|
| `fdisk /dev/nvme1n1` | Abre o particionador de disco |
| `partprobe` | Atualiza a tabela de partições no kernel |
| `mdadm --create` | Cria o array RAID |
| `mkfs.ext4` | Formata o array com sistema de arquivos ext4 |
| `mount` | Monta o dispositivo em uma pasta |
| `df -h` | Exibe o uso de disco de forma legível |

---

## Diferença entre RAID 0 e RAID 1

| | RAID 0 | RAID 1 |
|---|---|---|
| **Tipo** | Striping | Mirroring |
| **Velocidade** | ✅ Maior | ➖ Normal |
| **Redundância** | ❌ Nenhuma | ✅ Total |
| **Espaço** | 100% aproveitado | 50% aproveitado |
| **Uso indicado** | Performance | Segurança |
