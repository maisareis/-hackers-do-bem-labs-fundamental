# Atividade 7.6 – Backup Completo Local com Duplicity

## Objetivo
Implementar backup completo local de uma pasta usando o Duplicity no Kali Linux e restaurar os arquivos em outra pasta.

## Conceito

### Duplicity
O **Duplicity** é uma ferramenta de backup que cria arquivos criptografados usando GnuPG. Suporta backups locais e remotos (FTP, SSH, S3, etc.).

### Backup Completo
O **backup completo** copia todos os arquivos da origem, independentemente de alterações anteriores. É a base para backups futuros.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Criar pasta e arquivo de teste
```bash
cd /home/aluno/Documentos
mkdir Conteudo
cd Conteudo
nano teste.txt
# Conteúdo: "Teste"
```

### 3. Realizar o backup completo
```bash
duplicity /home/aluno/Documentos/Conteudo file:///home/aluno/Documentos/ArmazenaBackup
```
> Será solicitada uma senha para criptografia. Anote essa senha pois será necessária na restauração!

### 4. Verificar os arquivos de backup gerados
```bash
ls /home/aluno/Documentos/ArmazenaBackup
```
> Arquivos `.gpg` gerados pelo Duplicity.

### 5. Restaurar o backup
```bash
duplicity file:///home/aluno/Documentos/ArmazenaBackup /home/aluno/Documentos/BackupRestaurado
```
> Inserir a mesma senha utilizada no backup.

### 6. Verificar a restauração
```bash
ls /home/aluno/Documentos/BackupRestaurado
cat /home/aluno/Documentos/BackupRestaurado/teste.txt
```

---

## Estatísticas do Backup

| Campo | Descrição |
|-------|-----------|
| `SourceFiles` | Total de arquivos na origem |
| `NewFiles` | Arquivos novos em relação ao backup anterior |
| `ChangedFiles` | Arquivos alterados |
| `DeletedFiles` | Arquivos deletados |
| `Errors` | Erros durante o backup |

---

## Aprendizado
- Duplicity criptografa os backups com GnuPG, garantindo segurança
- A senha de criptografia é obrigatória para restauração
- Backup completo copia todos os arquivos independentemente de alterações
