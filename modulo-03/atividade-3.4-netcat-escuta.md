# Atividade 3.4 – Escutando requisições com o Netcat no Kali Linux

## Objetivo

Utilizar a ferramenta **Netcat (nc)** para colocar uma porta em modo de escuta e capturar requisições HTTP enviadas pelo navegador, visualizando os cabeçalhos da conexão em tempo real.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**. O uso indevido de ferramentas de captura de tráfego pode configurar crime conforme a legislação vigente.

## Conceito

O **Netcat** (`nc`) é uma ferramenta de linha de comando para comunicação de dados via TCP ou UDP. Permite criar conexões de rede, transferir dados e configurar servidores simples de escuta. É frequentemente referido como o "canivete suíço" das redes por sua versatilidade em testes de segurança e diagnóstico.

### Principais parâmetros do Netcat

| Parâmetro | Descrição                                                       |
|-----------|-----------------------------------------------------------------|
| `-l`      | Modo de escuta (servidor)                                       |
| `-p`      | Define a porta de escuta                                        |
| `-v`      | Modo verboso — exibe detalhes da conexão                        |
| `-n`      | Suprime resolução de DNS                                        |
| `-u`      | Usa UDP em vez de TCP                                           |

### Cabeçalhos HTTP capturados

Quando um navegador faz uma requisição HTTP, o Netcat captura os seguintes cabeçalhos:

| Cabeçalho                  | Descrição                                               |
|----------------------------|---------------------------------------------------------|
| `GET / HTTP/1.1`           | Método, recurso e versão HTTP da requisição             |
| `Host`                     | Endereço e porta do servidor destino                    |
| `User-Agent`               | Identificação do navegador e sistema operacional        |
| `Accept`                   | Tipos de conteúdo aceitos pelo cliente                  |
| `Accept-Language`          | Idiomas preferidos pelo cliente                         |
| `Accept-Encoding`          | Métodos de compressão suportados                        |
| `Connection`               | Política de manutenção da conexão TCP                   |
| `Upgrade-Insecure-Requests`| Indicação de preferência por HTTPS                      |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Visualizar os modos de funcionamento do Netcat:
```bash
nc -help
```

**4.** Verificar o IP da máquina:
```bash
ifconfig
```

**5.** Colocar o Netcat em modo de escuta na porta 5555:
```bash
nc -l -p 5555 -v
```
O terminal ficará aguardando conexões na porta 5555 em todas as interfaces disponíveis.

**6.** Abrir o Mozilla Firefox (Aplicativos → Navegador web) e acessar o IP da máquina na porta 5555:
```
http://[IP_DA_MAQUINA]:5555
```

**7.** *(Print solicitado)* Voltar ao Terminal e observar que o Netcat capturou a tentativa de conexão, exibindo todos os cabeçalhos HTTP enviados pelo Firefox — incluindo método, Host, User-Agent, Accept e demais cabeçalhos.

**8.** Fechar o Firefox e o Terminal.

## Aprendizado

- O Netcat em modo de escuta (`-l`) transforma qualquer porta em um receptor de conexões, permitindo visualizar o tráfego bruto que chega naquela porta.
- Os cabeçalhos HTTP capturados revelam informações detalhadas sobre o cliente: navegador, sistema operacional, idioma, tipos de conteúdo aceitos — dados utilizados tanto em análise de segurança quanto em fingerprinting de clientes.
- Esta técnica demonstra por que é importante proteger serviços expostos: qualquer ferramenta simples consegue interceptar e exibir o conteúdo de requisições não cifradas.
- O mesmo princípio é aplicado em ambientes reais para captura de credenciais, tokens e outros dados sensíveis transmitidos sem criptografia.
