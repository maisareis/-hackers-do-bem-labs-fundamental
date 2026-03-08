# Atividade 6.3 – Automação de Tarefas com Python

## Objetivo
Utilizar Python para automatizar a coleta de informações de hardware e sistema no Kali Linux, gerando um relatório em arquivo de texto.

## Conceito
A **automação de tarefas** permite executar operações repetitivas de forma programática, economizando tempo e reduzindo erros humanos. Neste exemplo, coletamos informações do sistema automaticamente usando módulos nativos do Python.

---

## Módulos Python Utilizados

| Módulo | Descrição |
|--------|-----------|
| `os` | Interação com o sistema operacional |
| `socket` | Operações de rede e informações do host |

---

## Código Utilizado

```python
import os
import socket

def coletar_informacoes_sistema():
    hostname = socket.gethostname()
    kernel_version = os.uname().release
    cpu_info = os.popen('cat /proc/cpuinfo | grep "model name" | uniq').read().strip()
    mem_info = os.popen('free -h | grep "Mem.:"').read().strip()
    disk_space = os.popen('df -h').read()

    informacoes = {
        "Hostname": hostname,
        "Kernel Version": kernel_version,
        "CPU Info": cpu_info,
        "Memory Info": mem_info,
        "Disk Space": disk_space
    }
    return informacoes

def gerar_relatorio(informacoes):
    relatorio = "Relatório do Sistema Linux\n\n"
    for chave, valor in informacoes.items():
        relatorio += f"{chave}: {valor}\n"

    with open("relatorio_linux.txt", "w") as arquivo:
        arquivo.write(relatorio)

if __name__ == "__main__":
    informacoes_sistema = coletar_informacoes_sistema()
    gerar_relatorio(informacoes_sistema)
    print("Relatório gerado com sucesso em 'relatorio_linux.txt'")
```

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Criar o script Python
Salvar o código acima como `dados_linux.py` na pasta Documentos.

### 3. Navegar até a pasta Documentos
```bash
cd /home/aluno/Documentos/
```

### 4. Executar o script
```bash
python dados_linux.py
# Relatório gerado com sucesso em 'relatorio_linux.txt'
```

### 5. Verificar o arquivo gerado
```bash
ls
# dados_linux.py  relatorio_linux.txt
```

### 6. Visualizar o relatório
```bash
cat relatorio_linux.txt
```

---

## Informações Coletadas pelo Script

- **Hostname** → nome do computador na rede
- **Kernel Version** → versão do núcleo do sistema operacional
- **CPU Info** → modelo do processador (`/proc/cpuinfo`)
- **Memory Info** → uso de memória RAM (`free -h`)
- **Disk Space** → espaço em disco de todas as partições (`df -h`)

---

## Aprendizado
- Python pode automatizar coleta de informações do sistema
- Os módulos `os` e `socket` permitem interagir com o sistema operacional
- O bloco `if __name__ == "__main__"` garante que o código só execute quando rodado diretamente
- Automatizar relatórios de hardware é útil para administração de sistemas
