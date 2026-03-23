# Atividade 10.9 – Full Disk Encryption em uma Partição no Kali Linux

## Objetivo
Implementar Full Disk Encryption (FDE) em uma partição de 200MB no Kali Linux utilizando o LUKS (Linux Unified Key Setup) com o cryptsetup, formatando e montando a partição criptografada.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **FDE** (Full Disk Encryption) criptografa todos os dados de uma partição ou disco, tornando-os inacessíveis sem a senha correta. Mesmo que o disco seja removido fisicamente, os dados permanecem protegidos.

O **LUKS** (Linux Unified Key Setup) é o padrão de criptografia de disco no Linux. Armazena os metadados de criptografia no início da partição, suportando múltiplas senhas (keyslots) para a mesma partição.

O **cryptsetup** é a ferramenta de linha de comando para gerenciar volumes LUKS no Linux.

O fluxo de uso de uma partição LUKS é:
```
luksFormat (criptografar) → luksOpen (desbloquear) → mkfs (formatar) → mount (montar) → uso → umount (desmontar) → luksClose (fechar)
```

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Disco secundário `/dev/nvme1n1` com partições p1 e p2 já criadas

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Explorar o armazenamento disponível
```bash
fdisk -l
```
Identificar o disco de trabalho `/dev/nvme1n1`.

### 3. Verificar as partições existentes no disco
```bash
fdisk /dev/nvme1n1
```
Dentro do fdisk, digitar `p` para listar as partições existentes (p1 e p2, de 100MB cada).

### 4. Criar a nova partição de 200MB
Ainda dentro do fdisk:
```
n       → nova partição
Enter   → tipo primário
Enter   → número 3
Enter   → setor inicial padrão
+200M   → tamanho de 200MB
```

### 5. Confirmar e salvar a nova partição
```
p   → verificar as 3 partições
w   → salvar e sair
```

Em seguida, atualizar a tabela de partições:
```bash
partprobe /dev/nvme1n1
```

### 6. Criptografar a partição com LUKS
```bash
cryptsetup luksFormat /dev/nvme1n1p3
```
Digitar `YES` (em maiúsculas) para confirmar. Informar e confirmar a senha. Neste lab foi usada a senha `abcd1234`.

### 7. Desbloquear (abrir) a partição criptografada
```bash
cryptsetup luksOpen /dev/nvme1n1p3 Teste
```
Informar a senha (`abcd1234`). A partição é mapeada em `/dev/mapper/Teste`.

### 8. Formatar a partição desbloqueada com ext4
```bash
mkfs.ext4 /dev/mapper/Teste
```

### 9. Criar o ponto de montagem
```bash
mkdir -m 777 /mnt/encrypted
```

### 10. Montar a partição criptografada
```bash
mount /dev/mapper/Teste /mnt/encrypted
```

### 11. Desmontar a partição
```bash
umount /mnt/encrypted
```

### 12. Verificar o acesso pelo Desktop
Verificar que um ícone **Volume de 193MB** foi criado no Desktop. Clicar 2 vezes no ícone para visualizar a pasta criptografada no Thunar.

Fechar o Thunar e o Terminal.

---

## Comandos cryptsetup

| Comando | Descrição |
|---------|-----------|
| `cryptsetup luksFormat <partição>` | Criptografa a partição com LUKS |
| `cryptsetup luksOpen <partição> <nome>` | Desbloqueia a partição |
| `cryptsetup luksClose <nome>` | Fecha/bloqueia a partição |
| `cryptsetup luksDump <partição>` | Exibe metadados LUKS da partição |

---

## Por que o volume tem 193MB e não 200MB?

O LUKS reserva espaço no início da partição para armazenar seus metadados (cabeçalho LUKS), que inclui informações sobre o algoritmo de criptografia e os keyslots. Isso reduz o espaço disponível para dados.

---

## Aprendizado
- O LUKS é o padrão de facto para criptografia de disco no Linux — é robusto, bem documentado e suportado nativamente pelo kernel
- A senha `luksFormat` e a senha `luksOpen` devem ser a mesma — sem ela, os dados são irrecuperáveis
- O LUKS suporta múltiplas senhas (até 8 keyslots) — útil para adicionar chaves de backup sem recriar a partição
- `umount` deve ser executado antes de desligar o sistema para garantir que os dados sejam gravados corretamente na partição criptografada
