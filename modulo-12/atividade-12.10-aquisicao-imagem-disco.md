# Atividade 12.10 – Aquisição de Imagem de Disco com dd no Kali Linux

## Objetivo
Realizar uma cópia bit a bit (imagem forense) de uma partição do Kali Linux para um arquivo utilizando o comando `dd`, e verificar os metadados do arquivo gerado com o comando `stat`.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

A **aquisição de imagem de disco** é o primeiro passo de uma investigação forense digital. Consiste em criar uma cópia exata de uma mídia de armazenamento — bit a bit — preservando todos os dados, inclusive espaço não alocado e arquivos apagados.

O comando `dd` (data duplicator) é a ferramenta clássica do Linux para cópias de baixo nível. Diferente de uma cópia comum de arquivos, o `dd` copia cada byte da origem — incluindo dados "vazios" e deletados.

### Princípios da forense digital na aquisição

1. **Preservação:** nunca trabalhar diretamente na evidência original
2. **Integridade:** verificar hashes (MD5/SHA256) antes e depois da cópia
3. **Documentação:** registrar todos os procedimentos realizados

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Disco secundário com partição disponível

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
Identificar a partição a ser copiada (ex: `/dev/nvme1n1p3`, 200MB).

### 3. Criar a imagem bit a bit com dd
```bash
dd if=/dev/nvme1n1p3 of=/home/aluno/Documentos/Arquivos/Backup_nvme1n1p3
```

### Parâmetros do dd

| Parâmetro | Descrição |
|-----------|-----------|
| `if=` | Input file — origem dos dados (partição) |
| `of=` | Output file — destino dos dados (arquivo de imagem) |

Resultado esperado:
```
409600+0 registos entrados
409600+0 registos saídos
209715200 bytes (210 MB, 200 MiB) copiados
```

### 4. Navegar até a pasta e confirmar o arquivo
```bash
cd /home/aluno/Documentos/Arquivos
ls
```

### 5. Verificar os metadados do arquivo de imagem
```bash
stat Backup_nvme1n1p3
```

### Campos da saída do stat

| Campo | Descrição |
|-------|-----------|
| **Arquivo** | Nome do arquivo |
| **Tamanho** | Tamanho em bytes (200 MiB) |
| **Blocos** | Número de blocos alocados |
| **bloco de E/S** | Tamanho do bloco do sistema de arquivos |
| **Dispositivo** | Número do dispositivo onde está armazenado |
| **Inode** | Número do inode do arquivo |
| **Ligações** | Número de hard links |
| **Acesso (permissões)** | Permissões e UID/GID |
| **Acesso (timestamp)** | Data/hora do último acesso |
| **Modificação** | Data/hora da última modificação do conteúdo |
| **Alteração** | Data/hora da última mudança de metadados |
| **Criação** | Data/hora de criação do arquivo |

### 6. Limpar e fechar
```bash
rm Backup_nvme1n1p3
ls
```
Fechar o Terminal.

---

## dd vs cópia convencional

| | dd (bit a bit) | cp (cópia de arquivos) |
|---|---|---|
| **Copia arquivos apagados** | ✅ Sim | ❌ Não |
| **Copia espaço não alocado** | ✅ Sim | ❌ Não |
| **Preserva estrutura do disco** | ✅ Sim | ❌ Não |
| **Uso forense** | ✅ Aceito | ❌ Não aceito |
| **Velocidade** | Variável | Mais rápida (só arquivos) |

---

## Aprendizado
- A imagem forense deve ser criada antes de qualquer análise — nunca analisar diretamente a mídia original
- Para uso forense profissional, verificar o hash MD5/SHA256 da partição antes e depois: `md5sum /dev/nvme1n1p3` vs `md5sum Backup_nvme1n1p3`
- O `dd` com `bs=4M` ou maior aumenta significativamente a velocidade de cópia em discos grandes
- Ferramentas forenses especializadas como `dcfldd` e `dc3dd` adicionam verificação de hash automática ao processo de aquisição
