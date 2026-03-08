# Atividade 7.3 – Replicação de Pastas com Rsync (Manual)

## Objetivo
Utilizar o comando `rsync` para replicar manualmente o conteúdo de uma pasta para outra no Kali Linux.

## Conceito
O **Rsync** é uma ferramenta de sincronização de arquivos que copia apenas as diferenças entre origem e destino, tornando as transferências eficientes.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Criar as pastas de origem e destino
```bash
cd /home/aluno/Documentos
mkdir -m 777 PastaA PastaB
```

### 3. Criar arquivo de teste na PastaA
```bash
cd PastaA
nano Teste.txt
# Conteúdo: "Teste de Pasta A para Pasta B"
```

### 4. Replicar o arquivo para PastaB
```bash
rsync -avz /home/aluno/Documentos/PastaA/Teste.txt /home/aluno/Documentos/PastaB
```

### Parâmetros do rsync

| Parâmetro | Descrição |
|-----------|-----------|
| `-a` | Modo arquivo: recursivo, preserva permissões e timestamps |
| `-v` | Modo verboso: exibe detalhes da transferência |
| `-z` | Comprime os dados durante a transferência |

### 5. Verificar a replicação
```bash
ls /home/aluno/Documentos/PastaB
cat /home/aluno/Documentos/PastaB/Teste.txt
```

---

## Aprendizado
- `rsync` é mais eficiente que `cp` pois copia apenas as diferenças
- O parâmetro `-a` preserva metadados dos arquivos
- Rsync funciona tanto localmente quanto entre servidores remotos via SSH
