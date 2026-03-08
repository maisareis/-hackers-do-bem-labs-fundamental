# Atividade 7.7 – Backup Diferencial Local com Duplicity

## Objetivo
Implementar backup diferencial usando o Duplicity, copiando apenas os arquivos alterados desde o último backup completo.

## Conceito

### Backup Diferencial
O **backup diferencial** copia apenas os arquivos que foram **alterados ou criados** desde o último backup completo. É mais rápido e ocupa menos espaço que um novo backup completo.

### Diferença entre Backup Completo e Diferencial

| | Backup Completo | Backup Diferencial |
|---|---|---|
| **O que copia** | Todos os arquivos | Apenas alterações |
| **Velocidade** | Mais lento | Mais rápido |
| **Espaço** | Maior | Menor |
| **Dependência** | Independente | Depende do backup completo |

---

## Pré-requisitos
- Atividade 7.6 concluída (backup completo já realizado)
- Pasta `ArmazenaBackup` com o backup completo

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Modificar arquivo existente e criar novo arquivo
```bash
cd /home/aluno/Documentos/Conteudo
nano teste.txt
# Alterar conteúdo para: "Teste Diferencial"

nano teste2.txt
# Conteúdo: "Teste2"
```

### 3. Realizar o backup diferencial
```bash
duplicity /home/aluno/Documentos/Conteudo file:///home/aluno/Documentos/ArmazenaBackup
```
> O Duplicity detecta automaticamente o backup completo anterior e realiza apenas o backup diferencial.
> Inserir a mesma senha do backup completo.

### 4. Restaurar o backup diferencial
```bash
duplicity file:///home/aluno/Documentos/ArmazenaBackup /home/aluno/Documentos/BackupRestaurado2
```

### 5. Verificar a restauração
```bash
ls /home/aluno/Documentos/BackupRestaurado2
# teste.txt  teste2.txt
```

---

## Como o Duplicity Detecta o Backup Diferencial
Quando executado em uma pasta que já possui um backup completo, o Duplicity automaticamente:
1. Verifica a data do último backup completo
2. Identifica quais arquivos foram alterados desde então
3. Copia apenas as diferenças

---

## Aprendizado
- Backup diferencial economiza tempo e espaço armazenando apenas alterações
- O Duplicity gerencia automaticamente o tipo de backup (completo ou diferencial)
- Para restauração completa, o Duplicity combina o backup completo com os diferenciais
- A mesma senha deve ser usada em todos os backups da mesma série
