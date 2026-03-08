# Atividade 3.8 – Enumeração DNS com host no Kali Linux

## Objetivo

Utilizar a ferramenta **host** para realizar enumeração de registros DNS de domínios, obtendo informações sobre endereços IP (A/AAAA), servidores de nomes (NS) e servidores de e-mail (MX).

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

A ferramenta **host** é um utilitário de linha de comando para consultas DNS simples e diretas. É usada em reconhecimento passivo (*OSINT*) para mapear a infraestrutura de um domínio sem interagir diretamente com os servidores alvo — apenas consultando registros públicos do DNS.

### Tipos de registros DNS consultados

| Registro | Descrição                                                               |
|----------|-------------------------------------------------------------------------|
| `A`      | Endereço IPv4 associado ao domínio                                      |
| `AAAA`   | Endereço IPv6 associado ao domínio                                      |
| `MX`     | Servidores de e-mail e suas prioridades                                 |
| `NS`     | Servidores de nomes autoritativos do domínio                            |
| `HTTPS`  | Registro SVCB/HTTPS com dicas de conexão, protocolos suportados e ECH   |

### Sintaxe do comando host

```bash
host [domínio]               # Consulta padrão (A + AAAA + MX)
host -t ns [domínio]         # Consulta registros NS
host -t mx [domínio]         # Consulta registros MX
host -t txt [domínio]        # Consulta registros TXT
```

### Prioridade nos registros MX

Quanto **menor** o número de prioridade, **maior** a preferência no roteamento de e-mails:

| Prioridade | Papel                          |
|------------|--------------------------------|
| 1          | Servidor principal             |
| 5          | Servidor secundário            |
| 10         | Servidor de backup             |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Consultar todos os registros DNS padrão de um domínio:
```bash
host grancursos.com.br
```
A saída exibe registros A (IPv4), AAAA (IPv6), MX (servidores de e-mail) e o registro HTTPS/SVCB com informações sobre protocolos suportados (h3, h2) e suporte a ECH (*Encrypted Client Hello*).

**4.** Consultar os servidores de nomes (NS) de um subdomínio:
```bash
host -t ns esr.rnp.br
```
A ausência de registro NS indica que o subdomínio não possui servidores de nomes autoritativos configurados diretamente.

**5.** Consultar os servidores de nomes (NS) de um domínio comercial:
```bash
host -t ns grancursosonline.com.br
```
Os servidores NS retornados pertencem à infraestrutura da Cloudflare, indicando uso de CDN/proxy de segurança.

**6.** *(Print solicitado)* Consultar os servidores de e-mail (MX) de um domínio comercial:
```bash
host -t mx grancursosonline.com.br
```
A saída lista cinco servidores MX do Google Workspace com diferentes prioridades: o servidor `aspmx.l.google.com` (prioridade 1) é o principal, seguido pelos servidores `alt1` e `alt2` (prioridade 5) e `alt3` e `alt4` (prioridade 10) como backups.

**7.** Fechar o Terminal.

## Aprendizado

- A ferramenta `host` permite um reconhecimento passivo eficiente, coletando informações de infraestrutura de um domínio sem contato direto com os servidores alvo.
- O registro MX revela o provedor de e-mail utilizado (neste caso, Google Workspace), informação útil para entender a superfície de ataque e possíveis vetores de phishing.
- O registro NS aponta para o provedor de DNS (Cloudflare), o que indica que o domínio pode estar protegido por WAF e proxy reverso, dificultando a identificação do IP real do servidor.
- O suporte a ECH (*Encrypted Client Hello*) no registro HTTPS indica que o domínio utiliza tecnologias modernas de privacidade, protegendo o SNI durante o handshake TLS.
- A ausência de registro NS em subdomínios (como `esr.rnp.br`) é comum quando o subdomínio herda a configuração de DNS da zona pai.
