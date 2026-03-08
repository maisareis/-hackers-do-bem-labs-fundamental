# Atividade 7.2 – Aplicando o Software RAID 1 no Kali Linux

## Objetivo
Implementar o RAID 1 (mirroring) no Kali Linux utilizando 2 partições de 100MB cada, criando um array com espelhamento de dados para redundância.

## Conceito
O **RAID 1** (mirroring) espelha os dados entre dois ou mais discos simultaneamente. Cada dado gravado é duplicado automaticamente, garantindo que se um disco falhar, os dados permanecem intactos no outro.

⚠️ **Atenção:** O RAID 1 utiliza apenas **50% do espaço total** disponível, pois os dados são duplicados.

---

## Pré-requisitos
- Kali Linux com Terminal
- Disco secundário disponível com partições p1 e p2 já criadas (Atividade 7.1)
- Espaço disponível para criar mais 2 partições de 100MB

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Verificar configuração atual dos discos
```bash
fdisk -l
```

### 3. Criar as novas partições para o RAID 1
```bash
fdisk /dev/nvme1n1
```
Dentro do fdisk:
- `n` → nova partição
- Enter → tipo primário (p)
- Enter → número 3
- Enter → setor inicial padrão
- `+100M` → tamanho de 100MB

Repetir para criar a quarta partição (`nvme1n1p4`).

Confirmar e salvar:
- `p` → visualizar partições
- `w` → salvar e sair

### 4. Atualizar a tabela de partições
```bash
partprobe /dev/nvme1n1
```

### 5. Criar o array RAID 1
```bash
mdadm --create /dev/md1 --level=1 --raid-devices=2 /dev/nvme1n1p3 /dev/nvme1n1p4
```
> Responder `y` para as confirmações solicitadas.
> O parâmetro `--level=1` define o tipo RAID 1.

### 6. Verificar o dispositivo criado
```bash
ls -l /dev/md1
```

### 7. Verificar o status dos arrays RAID
```bash
cat /proc/mdstat
```
> Mostra todos os arrays ativos, incluindo o RAID 0 (md0) e RAID 1 (md1).

### 8. Formatar o array com ext4
```bash
mkfs.ext4 /dev/md1
```

### 9. Criar ponto de montagem
```bash
mkdir /mnt/raid1
```

### 10. Montar o array e verificar
```bash
mount /dev/md1 /mnt/raid1
df -h
```
> O array `/dev/md1` aparecerá montado em `/mnt/raid1` com ~88MB disponíveis.

---

## Resultado Esperado
```
/dev/md0   179M   63K  165M   1% /mnt/raid0
/dev/md1    88M   46K   81M   1% /mnt/raid1
```
> O RAID 1 tem apenas ~88MB de espaço útil pois os outros 100MB são usados como espelho.

---

## Comparativo RAID 0 vs RAID 1

| | RAID 0 | RAID 1 |
|---|---|---|
| **Tipo** | Striping | Mirroring |
| **Velocidade** | ✅ Maior | ➖ Normal |
| **Redundância** | ❌ Nenhuma | ✅ Total |
| **Espaço útil** | 100% | 50% |
| **Uso indicado** | Performance | Segurança/Backup |
| **Falha de 1 disco** | ❌ Perde tudo | ✅ Dados preservados |

---

## Aprendizado
- RAID 1 garante redundância espelhando dados entre partições
- O espaço útil é metade do total pois os dados são duplicados
- `cat /proc/mdstat` exibe o status de todos os arrays RAID ativos
- RAID 1 é ideal para dados críticos que não podem ser perdidos
