# Atividade 3.5 – Redirecionando tráfego via portas com o Netcat no Kali Linux

## Objetivo

Utilizar o **Ncat** para redirecionar tráfego recebido na porta 80 para a porta 443, demonstrando comunicação bidirecional entre dois terminais via encaminhamento de portas.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**. O redirecionamento de portas em redes sem autorização pode configurar crime conforme a legislação vigente.

## Conceito

O **Ncat** é uma versão avançada do Netcat desenvolvida pelo projeto Nmap, com suporte a SSL/TLS, IPv6, proxies e execução de comandos. O encaminhamento de portas (*port forwarding*) é uma técnica que redireciona conexões de uma porta para outra, podendo ser usada tanto para fins legítimos (tunelamento, redirecionamento de serviços) quanto em cenários de ataque.

### Topologia da atividade

```
Terminal 2 (porta 80) ←→ Ncat relay (80→443) ←→ Terminal 3 (porta 443)
```

### Parâmetros do Ncat utilizados

| Parâmetro      | Descrição                                                          |
|----------------|--------------------------------------------------------------------|
| `-v`           | Modo verboso                                                       |
| `-l`           | Modo de escuta (servidor)                                          |
| `80`           | Porta de escuta do relay                                           |
| `-c 'comando'` | Executa comando ao receber conexão — usado para criar o relay      |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** No **1º terminal** (root), iniciar o relay que escuta na porta 80 e redireciona para a porta 443:
```bash
ncat -vl 80 -c 'ncat -l 443'
```
O Ncat ficará aguardando conexões nas portas 80 (IPv4 e IPv6).

**4.** Abrir um **2º terminal** (usuário aluno) e conectar na porta 80 do localhost:
```bash
ncat -nv 127.0.0.1 80
```
A saída confirmará a conexão: `Ncat: Connected to 127.0.0.1:80`.

**5.** Verificar no **1º terminal** que a conexão foi registrada:
```
Ncat: Connection from 127.0.0.1:[PORTA]
```

**6.** Abrir um **3º terminal** (usuário aluno) e conectar na porta 443 do localhost:
```bash
ncat -v 127.0.0.1 443
```
A saída confirmará a conexão: `Ncat: Connected to 127.0.0.1:443`.

**7.** No **3º terminal**, digitar `Teste A` e pressionar Enter — a mensagem será enviada pelo canal da porta 443.

**8.** Verificar no **2º terminal** que `Teste A` aparece — o relay encaminhou a mensagem da porta 443 para a porta 80.

**9.** *(Print solicitado)* No **2º terminal**, digitar `Teste B` e pressionar Enter. Verificar no **3º terminal** que `Teste B` é exibido, confirmando a comunicação bidirecional entre as duas portas via relay.

**10.** Fechar os três terminais.

## Aprendizado

- O encaminhamento de portas com Ncat demonstra como o tráfego pode ser redirecionado entre portas diferentes de forma transparente para as partes conectadas.
- A opção `-c` do Ncat permite executar um comando ao receber uma conexão, tornando-o flexível para criar relays, shells reversos e outros cenários de rede.
- A comunicação bidirecional confirmada entre os terminais mostra que o relay não apenas encaminha pacotes, mas mantém um canal full-duplex entre as duas conexões.
- Em contextos de segurança, essa técnica pode ser usada para pivotamento de rede (lateral movement), tunelamento de serviços e bypass de regras de firewall baseadas em porta.
