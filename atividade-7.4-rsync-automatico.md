# Atividade 7.4 – Replicação Automática com Rsync e Crontab

## Objetivo
Automatizar a replicação de arquivos entre pastas usando Rsync agendado pelo Crontab no Kali Linux.

## Conceitos

### Crontab
O **crontab** é o agendador de tarefas do Linux. Permite executar scripts ou comandos automaticamente em horários ou intervalos definidos.

### Formato do Crontab
```
* * * * * comando
│ │ │ │ │
│ │ │ │ └── dia da semana (0-7)
│ │ │ └──── mês (1-12)
│ │ └────── dia do mês (1-31)
│ └──────── hora (0-23)
└────────── minuto (0-59)
```

> `* * * * *` significa "executar a cada minuto"

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Criar o script de sincronização
```bash
cd /home/aluno/Documentos
nano sync_script.sh
```

Conteúdo do script:
```bash
#!/bin/bash
rsync -avz /home/aluno/Documentos/PastaA/ /home/aluno/Documentos/PastaB/
```

### 3. Tornar o script executável
```bash
chmod +x sync_script.sh
```

### 4. Configurar o Crontab
```bash
crontab -e
```
Adicionar a linha:
```
* * * * * /home/aluno/Documentos/sync_script.sh
```

### 5. Aguardar e verificar
Após 1 minuto, verificar que o arquivo foi replicado automaticamente:
```bash
ls /home/aluno/Documentos/PastaB
```

---

## Aprendizado
- Crontab permite automatizar tarefas recorrentes no Linux
- Combinando Rsync + Crontab é possível criar backups automáticos
- O script deve ter permissão de execução (`chmod +x`)
- `* * * * *` agenda a execução a cada minuto
- Para mais opções de agendamento: https://cron.help
