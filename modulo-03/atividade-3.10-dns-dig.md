# Atividade 3.10 – Enumeração DNS com dig no Kali Linux

## Objetivo

Utilizar a ferramenta **dig** para realizar enumeração detalhada de registros DNS, incluindo A, NS, MX, AAAA e CNAME, analisando as seções completas das respostas DNS.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **dig** (*Domain Information Groper*) é a ferramenta de consulta DNS mais completa disponível no Linux. Ao contrário do `host` e do `nslookup`, o `dig` exibe toda a estrutura da resposta DNS — incluindo cabeçalho, flags, seções de pergunta, resposta, autoridade e adicional — tornando-o ideal para diagnóstico avançado e análise de comportamento do DNS.

### Estrutura da resposta do dig

| Seção              | Conteúdo                                                        |
|--------------------|-----------------------------------------------------------------|
| `HEADER`           | Opcode, status, ID e flags da consulta/resposta                 |
| `OPT PSEUDOSECTION`| Configurações EDNS (versão, flags, tamanho UDP máximo)          |
| `QUESTION SECTION` | Registro consultado (domínio + tipo)                            |
| `ANSWER SECTION`   | Registros que respondem à consulta                              |
| `AUTHORITY SECTION`| Servidores autoritativos para o domínio (quando aplicável)      |
| `ADDITIONAL`       | Informações complementares                                      |

### Flags comuns no cabeçalho

| Flag | Significado                                        |
|------|----------------------------------------------------|
| `qr` | Query Response — é uma resposta                   |
| `rd` | Recursion Desired — cliente pediu recursão         |
| `ra` | Recursion Available — servidor suporta recursão    |

### Tipos de registro consultados com dig

```bash
dig [domínio]               # Registro A (padrão)
dig [domínio] -t ns         # Servidores de nomes (NS)
dig [domínio] -t mx         # Servidores de e-mail (MX)
dig [domínio] AAAA          # Endereços IPv6
dig [domínio] CNAME         # Nome canônico
```

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Verificar a sintaxe e opções disponíveis do dig:
```bash
dig -h
```

**4.** Consultar os registros A (IPv4) de um domínio comercial:
```bash
dig grancursosonline.com.br
```
A resposta inclui: status `NOERROR`, 2 respostas na `ANSWER SECTION` com os IPs IPv4 da Cloudflare (TTL de 300 segundos), o servidor DNS utilizado (`192.168.98.2#53`), tempo de consulta (4ms) e tamanho da mensagem recebida (84 bytes).

**5.** Consultar os servidores de nomes (NS):
```bash
dig grancursosonline.com.br -t ns
```
Retorna dois servidores NS da Cloudflare (`rita.ns.cloudflare.com` e `dan.ns.cloudflare.com`) com TTL de 300 segundos.

**6.** Consultar os servidores de e-mail (MX):
```bash
dig grancursosonline.com.br -t mx
```
Retorna 5 registros MX do Google Workspace com prioridades 1, 5 e 10. O cabeçalho confirma `ANSWER: 5`.

**7.** Consultar os endereços IPv6 (AAAA):
```bash
dig grancursosonline.com.br AAAA
```
Retorna dois endereços IPv6 da Cloudflare com TTL de 300 segundos, confirmando suporte completo a IPv6 pelo domínio.

**8.** *(Print solicitado)* Consultar o registro CNAME do domínio:
```bash
dig grancursosonline.com.br CNAME
```
A `ANSWER SECTION` está vazia (0 respostas), indicando que o domínio raiz não possui registro CNAME — o que é esperado, pois CNAMEs não podem coexistir com registros A na raiz de um domínio. A `AUTHORITY SECTION` retorna o registro SOA (*Start of Authority*) da Cloudflare, com informações como servidor primário (`dan.ns.cloudflare.com`), e-mail do administrador (`dns.cloudflare.com`), serial e intervalos de atualização/expiração.

**9.** Fechar o Terminal.

## Aprendizado

- O `dig` é a ferramenta mais completa para análise de DNS: exibe toda a estrutura da resposta, incluindo flags, TTLs, seções de autoridade e estatísticas da consulta.
- A seção `AUTHORITY SECTION` com registro SOA é retornada quando o tipo consultado não existe para o domínio — nela constam informações sobre o servidor primário e parâmetros de zona.
- A impossibilidade de ter CNAME na raiz de um domínio (zona apex) é uma limitação do protocolo DNS — por isso serviços como Cloudflare utilizam registros ALIAS ou CNAME achatados (*flattening*) como solução.
- O TTL de 300 segundos (5 minutos) indica que os registros do domínio são atualizados frequentemente, comum em ambientes com CDN e alta disponibilidade.
- Comparando as três ferramentas: `host` é mais simples e direta, `nslookup` oferece modo interativo conveniente, e `dig` é a mais completa para diagnóstico e análise técnica aprofundada.
