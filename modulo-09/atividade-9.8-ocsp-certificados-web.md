# Atividade 9.8 – Verificando a Validade de Certificados Web via OCSP no Kali Linux

## Objetivo
Avaliar certificados de sites comerciais utilizando o protocolo OCSP no Kali Linux com o OpenSSL, verificando o status de validade e as características da conexão TLS.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **OCSP** (Online Certificate Status Protocol) permite verificar em tempo real se um certificado está válido ou foi revogado, sem precisar baixar a CRL completa.

O **OCSP Stapling** é uma otimização onde o próprio servidor inclui a resposta OCSP na negociação TLS, evitando que o cliente precise consultar o servidor OCSP separadamente.

O comando `openssl s_client` estabelece uma conexão TLS com um servidor remoto, exibindo detalhes do certificado, da cadeia de confiança e do protocolo negociado.

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- OpenSSL instalado
- Conexão com a internet

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Verificar certificado de site com resposta OCSP
```bash
openssl s_client -connect www.grancursos.com.br:443 -status
```

### Parâmetros do comando

| Parâmetro | Descrição |
|-----------|-----------|
| `s_client` | Subcomando para conexão SSL/TLS como cliente |
| `-connect` | Host e porta de destino |
| `-status` | Solicita resposta OCSP durante a conexão |

### Campos da resposta OCSP a serem analisados

| Campo | Descrição |
|-------|-----------|
| **OCSP Response Status** | Status da resposta (`successful` = sucesso) |
| **Response Type** | Tipo da resposta (Basic OCSP Response) |
| **Version** | Versão do protocolo OCSP |
| **Responder Id** | Hash identificador do servidor OCSP |
| **Produced At** | Momento em que a resposta foi gerada |
| **Cert Status** | Status do certificado (`good` = válido) |
| **This Update** | Momento desta atualização |
| **Next Update** | Próxima atualização prevista |
| **Signature Algorithm** | Algoritmo usado para assinar a resposta OCSP |

### 3. Analisar a cadeia de certificação retornada

```
Certificate chain:
 0 - Certificado do site (grancursos.com.br)
 1 - CA intermediária (Google Trust Services WE1)
 2 - Root CA (GTS Root R4 / GlobalSign)
```

### 4. Analisar os detalhes TLS da conexão

| Campo | Valor do lab |
|-------|-------------|
| **Protocolo** | TLSv1.3 |
| **Cipher** | TLS_AES_256_GCM_SHA384 |
| **Verificação** | OK (certificado válido) |

### 5. Verificar site sem suporte a OCSP Stapling
```bash
openssl s_client -connect www.google.com:443 -status
```
Resultado esperado:
```
OCSP response: no response sent
```
> A mensagem `no response sent` indica que o servidor não fornece OCSP Stapling. Isso não significa que o certificado é inválido — apenas que o servidor optou por não incluir a resposta OCSP na negociação TLS.

---

## Comparativo OCSP vs CRL

| | OCSP | CRL |
|---|---|---|
| **Mecanismo** | Consulta em tempo real | Lista baixada periodicamente |
| **Tamanho** | ✅ Resposta pequena | ❌ Arquivo pode ser grande |
| **Privacidade** | ❌ Servidor sabe quais certificados você consulta | ✅ Lista baixada localmente |
| **Disponibilidade** | Depende do servidor OCSP | ✅ Funciona offline após download |
| **OCSP Stapling** | ✅ Elimina consulta separada | N/A |

---

## Aprendizado
- O `openssl s_client -status` verifica o status OCSP do certificado durante a conexão TLS
- `Cert Status: good` confirma que o certificado não foi revogado
- Nem todos os servidores suportam OCSP Stapling — a ausência de resposta OCSP não indica problema de segurança
- O TLS 1.3 com AES-256-GCM é o padrão atual de segurança para conexões web
