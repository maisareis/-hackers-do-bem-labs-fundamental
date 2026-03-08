# Atividade 3.9 – Enumeração DNS com nslookup no Kali Linux

## Objetivo

Utilizar a ferramenta **nslookup** para realizar enumeração de registros DNS de domínios, incluindo registros A, AAAA, NS, MX e TXT, de forma interativa e por linha de comando.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **nslookup** é uma ferramenta de consulta DNS disponível em Linux, Windows e macOS. Pode ser usado de forma direta (passando o domínio como argumento) ou em modo interativo, permitindo alterar o tipo de registro consultado sem sair do programa.

### Modos de uso

```bash
nslookup [domínio]               # Consulta direta (registro A padrão)
nslookup -type=txt [domínio]     # Consulta registros TXT diretamente

# Modo interativo:
nslookup
> set type=ns                    # Define o tipo de registro
> [domínio]                      # Realiza a consulta
```

### Tipos de registros consultados nesta atividade

| Tipo  | Descrição                                                                 |
|-------|---------------------------------------------------------------------------|
| `A`   | Endereços IPv4 do domínio                                                 |
| `AAAA`| Endereços IPv6 do domínio                                                 |
| `NS`  | Servidores de nomes autoritativos                                         |
| `MX`  | Servidores de e-mail com prioridade                                       |
| `TXT` | Registros de texto: verificações, SPF, DKIM e outras configurações        |

### Registros TXT comuns e seus significados

| Prefixo do registro TXT               | Finalidade                                       |
|----------------------------------------|--------------------------------------------------|
| `google-site-verification=`           | Verificação de propriedade no Google Search      |
| `atlassian-domain-verification=`      | Verificação de domínio para produtos Atlassian   |
| `v=spf1 include:...`                  | SPF — define servidores autorizados a enviar e-mail |
| `MS=ms...`                            | Verificação de domínio para Microsoft 365        |
| `miro-verification=`                  | Verificação de domínio para Miro                 |
| `onetrust-domain-verification=`       | Verificação para OneTrust (gestão de privacidade)|

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Consultar os registros A e AAAA de um domínio comercial:
```bash
nslookup grancursosonline.com.br
```
A saída exibe o servidor DNS utilizado para a consulta, dois endereços IPv4 e dois IPv6 — todos pertencentes à Cloudflare, indicando uso de CDN/proxy reverso.

**4.** Consultar os servidores de nomes (NS) em modo interativo:
```bash
nslookup
> set type=ns
> grancursosonline.com.br
```
Os dois servidores NS retornados (`dan.ns.cloudflare.com` e `rita.ns.cloudflare.com`) confirmam que o DNS do domínio é gerenciado pela Cloudflare.

**5.** Consultar os servidores de e-mail (MX) em modo interativo:
```bash
> set type=mx
> grancursosonline.com.br
```
Cinco servidores MX são retornados, todos do Google Workspace, com prioridades 1 (principal), 5 (secundário) e 10 (backup).

**6.** *(Print solicitado)* Sair do modo interativo (`Ctrl+C`) e consultar os registros TXT:
```bash
nslookup -type=txt grancursosonline.com.br
```
A resposta é truncada e retransmitida em modo TCP devido ao volume de registros. São retornados múltiplos registros TXT revelando: verificações de propriedade de vários serviços (Google, Atlassian, Microsoft, Miro, OneTrust), regras SPF autorizando envio de e-mail por Amazon SES, Google, Zendesk, HubSpot e Salesforce, e um registro de verificação da Validity.

**7.** Fechar o Terminal.

## Aprendizado

- O `nslookup` é versátil: funciona tanto em consultas rápidas por linha de comando quanto em modo interativo para exploração sequencial de diferentes tipos de registro.
- Registros TXT são fontes ricas de informação em reconhecimento passivo: revelam quais ferramentas SaaS o domínio utiliza (CRM, helpdesk, automação de marketing), o que pode orientar ataques direcionados.
- O registro SPF (`v=spf1`) lista os servidores autorizados a enviar e-mail pelo domínio — qualquer servidor não listado que envie e-mail em nome do domínio tende a ser marcado como spam ou rejeitado.
- A truncagem da resposta DNS e o fallback para TCP ocorrem quando a resposta excede 512 bytes — limite do protocolo DNS sobre UDP.
- IPs da Cloudflare nos registros A/AAAA indicam que o IP real do servidor de origem está oculto atrás do proxy, dificultando ataques diretos à infraestrutura.
