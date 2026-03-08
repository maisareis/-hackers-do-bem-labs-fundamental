# Atividade 1.5 – Gerenciamento de Tarefas com Taskwarrior

## Objetivo
Explorar o Taskwarrior, uma ferramenta de gerenciamento de tarefas via linha de comando no Kali Linux.

## Conceito
O **Taskwarrior** é uma ferramenta de gerenciamento de tarefas (To-Do) que funciona inteiramente pela linha de comando. Permite criar, organizar, priorizar e concluir tarefas com suporte a tags, prazos e níveis de urgência.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Configurar o Taskwarrior
```bash
mkdir -p ~/.task
echo 'data.location=~/.task' > ~/.taskrc
```

### 3. Adicionar tarefas
```bash
task add Comprar leite
task add Comprar cafe
task add Comprar salgados
```

### 4. Listar tarefas
```bash
task list
```

### 5. Marcar tarefa como urgente
```bash
task 2 modify +urgent
task list
```

### 6. Definir prazo para uma tarefa
```bash
task 3 modify due:2036-12-31
task list
```

### 7. Marcar tarefa como concluída
```bash
task 1 done
task list
```

---

## Principais Comandos

| Comando | Descrição |
|---------|-----------|
| `task add <descrição>` | Cria nova tarefa |
| `task list` | Lista tarefas pendentes |
| `task <id> modify +urgent` | Marca tarefa como urgente |
| `task <id> modify due:AAAA-MM-DD` | Define prazo |
| `task <id> done` | Marca tarefa como concluída |
| `task <id> delete` | Remove tarefa |

---

## Sistema de Urgência
O Taskwarrior calcula automaticamente a urgência de cada tarefa com base em:
- Tags (`+urgent`)
- Prazo (`due`)
- Idade da tarefa
- Prioridade definida

---

## Aprendizado
- Taskwarrior é uma alternativa poderosa a aplicativos de tarefas gráficos
- O sistema de urgência ajuda a priorizar automaticamente as tarefas
- Tags e prazos permitem organização detalhada das atividades
- Ideal para administradores de sistemas que trabalham majoritariamente no terminal
