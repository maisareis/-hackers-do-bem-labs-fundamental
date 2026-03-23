# Atividade 10.10 – Usando Ubuntu em Container Docker no Kali Linux

## Objetivo
Executar um container Ubuntu no Kali Linux utilizando Docker, explorar comandos básicos de gerenciamento de containers e imagens, e monitorar recursos em tempo real.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **Docker** é uma plataforma de containerização que permite empacotar aplicações e suas dependências em containers isolados, garantindo que a aplicação funcione da mesma forma em qualquer ambiente.

Diferente de uma VM, um **container** compartilha o kernel do sistema operacional hospedeiro, sendo mais leve e iniciando em segundos. Cada container tem seu próprio sistema de arquivos, rede e processos isolados.

| | Container | VM |
|---|---|---|
| **Isolamento** | Processo/namespace | Hardware virtualizado |
| **Overhead** | ✅ Muito baixo | ❌ Alto |
| **Tempo de boot** | ✅ Segundos | ❌ Minutos |
| **Kernel** | Compartilhado com host | Próprio |
| **Uso típico** | Microserviços, CI/CD | Sistemas completos |

---

## Pré-requisitos
- Kali Linux com Docker instalado
- Conexão com a internet para baixar a imagem Ubuntu

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Verificar o funcionamento do Docker
```bash
docker
```
O comando exibe o menu de ajuda com todos os subcomandos disponíveis, confirmando que o Docker está instalado e funcionando.

### 3. Listar as imagens disponíveis
```bash
docker images
```
Resultado esperado (sem imagens):
```
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

### 4. Baixar a imagem do Ubuntu
```bash
docker pull ubuntu
```
Resultado esperado:
```
latest: Pulling from library/ubuntu
Status: Downloaded newer image for ubuntu:latest
```

### 5. Confirmar que a imagem foi baixada
```bash
docker images
```
Resultado esperado:
```
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    ca2b0f26964c   10 days ago   77.9MB
```

### 6. Criar o container sem iniciá-lo
```bash
docker run ubuntu
```

### 7. Iniciar o container interativo com bash
```bash
docker run --name MeuContainer -it ubuntu bash
```
Resultado esperado (prompt do Ubuntu dentro do container):
```
root@2c6580444ba4:/#
```

### Parâmetros do docker run

| Parâmetro | Descrição |
|-----------|-----------|
| `--name MeuContainer` | Nome do container |
| `-it` | Modo interativo com terminal TTY |
| `ubuntu` | Imagem a ser usada |
| `bash` | Comando a executar dentro do container |

### 8. Executar comando dentro do container
```bash
ls
```
Resultado esperado (estrutura de diretórios do Ubuntu):
```
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

### 9. Abrir segundo Terminal e verificar o processo do container
Em um segundo Terminal:
```bash
sudo -i
docker top MeuContainer
```
Resultado esperado:
```
UID    PID    PPID   C    STIME   TTY   TIME     CMD
root   40370  40342  0    16:09   pts/0 00:00:00  bash
```

### 10. Monitorar estatísticas em tempo real
```bash
docker stats
```
Resultado esperado:
```
CONTAINER ID   NAME           CPU %   MEM USAGE / LIMIT   MEM %   NET I/O       BLOCK I/O   PIDS
2c6580444ba4   MeuContainer   0.01%   916KiB / 1.926GiB   0.05%   1.16kB / 0B   0B / 0B     1
```
Pressionar `Ctrl+C` para sair. Fechar o segundo Terminal.

### 11. Sair do container Ubuntu
No primeiro Terminal:
```bash
exit
```

### 12. Parar o container
```bash
docker stop MeuContainer
```
Resultado esperado:
```
MeuContainer
```

### 13. Remover containers parados
```bash
docker container prune
```
Digitar `y` para confirmar. Fechar o Terminal.

---

## Principais comandos Docker

| Comando | Descrição |
|---------|-----------|
| `docker pull <imagem>` | Baixa uma imagem do Docker Hub |
| `docker images` | Lista imagens disponíveis localmente |
| `docker run <imagem>` | Cria e inicia um container |
| `docker ps` | Lista containers em execução |
| `docker top <container>` | Exibe processos do container |
| `docker stats` | Estatísticas de recursos em tempo real |
| `docker stop <container>` | Para um container |
| `docker container prune` | Remove containers parados |
| `docker rmi <imagem>` | Remove uma imagem |

---

## Aprendizado
- O Docker compartilha o kernel do host — um container Ubuntu no Kali Linux usa o kernel do Kali, não o kernel do Ubuntu
- `docker run -it ubuntu bash` é o comando mais comum para explorar containers interativamente
- `docker stats` é essencial para monitorar o consumo de recursos de containers em produção
- `docker container prune` e `docker image prune` são boas práticas para liberar espaço em disco após testes
