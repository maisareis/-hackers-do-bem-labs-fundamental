# Atividade 9.5 – Criando Certificados Digitais Autoassinados no Kali Linux

## Objetivo
Criar um certificado digital autoassinado no Kali Linux utilizando o OpenSSL, passando pelas etapas de geração de chave privada, criação do CSR e emissão do certificado.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

Um **certificado autoassinado** é assinado pela própria chave privada do titular, sem uma CA externa. É útil para ambientes de teste e desenvolvimento interno, mas não é confiável por padrão nos navegadores.

### Fluxo de criação de um certificado

```
1. Chave Privada (private.key)
        ↓
2. CSR - Certificate Signing Request (request.csr)
        ↓
3. Certificado Autoassinado (certificate.crt)
```

| Arquivo | Descrição |
|---------|-----------|
| `private.key` | Chave privada RSA 2048 bits, protegida por AES-256 |
| `request.csr` | Solicitação de certificado com dados do titular |
| `certificate.crt` | Certificado X.509 autoassinado, válido por 365 dias |

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- OpenSSL instalado

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Acessar a pasta de trabalho
```bash
cd /home/aluno/Documentos/Arquivos
```

### 3. Gerar a chave privada RSA-2048 protegida por AES-256
```bash
openssl genpkey -algorithm RSA -out private.key -aes256
ls
```
Informar e confirmar senha quando solicitado. Neste lab foi usada a senha `abcd1234`.

Resultado esperado:
```
private.key
```

### Parâmetros do comando

| Parâmetro | Descrição |
|-----------|-----------|
| `genpkey` | Gera uma chave privada |
| `-algorithm RSA` | Algoritmo RSA |
| `-out private.key` | Arquivo de saída da chave privada |
| `-aes256` | Cifra a chave privada com AES-256 (requer senha) |

### 4. Gerar o CSR (Certificate Signing Request)
```bash
openssl req -new -key private.key -out request.csr
```
Informar a senha da chave privada (`abcd1234`) e preencher os campos solicitados:

| Campo | Valor utilizado no lab |
|-------|----------------------|
| Country Name | BR |
| State or Province Name | DF |
| Locality Name | Brasilia |
| Organization Name | RNP |
| Organizational Unit Name | ESR |
| Common Name | esr.rnp.br |
| Email Address | a@a.com.br |
| Challenge password | rnpesr |
| Optional company name | RNP |

### Parâmetros do comando

| Parâmetro | Descrição |
|-----------|-----------|
| `req -new` | Cria uma nova requisição de certificado |
| `-key private.key` | Chave privada associada ao CSR |
| `-out request.csr` | Arquivo de saída do CSR |

### 5. Gerar o certificado autoassinado e verificar os arquivos
```bash
openssl x509 -req -in request.csr -signkey private.key -out certificate.crt -days 365
ls
```
Informar a senha da chave privada (`abcd1234`).

Resultado esperado:
```
Certificate request self-signature ok
subject=C = BR, ST = DF, L = Brasilia, O = RNP, OU = ESR, CN = esr.rnp.br, emailAddress = a@a.com.br

certificate.crt  private.key  request.csr
```

### Parâmetros do comando

| Parâmetro | Descrição |
|-----------|-----------|
| `x509 -req` | Gera certificado a partir de um CSR |
| `-in request.csr` | CSR de entrada |
| `-signkey private.key` | Chave privada para autoassinatura |
| `-out certificate.crt` | Arquivo de saída do certificado |
| `-days 365` | Validade do certificado em dias |

### 6. Limpeza da pasta de trabalho
```bash
rm -r *
ls
```

---

## Diferença entre certificado autoassinado e emitido por CA

| | Autoassinado | Emitido por CA |
|---|---|---|
| **Confiança** | ❌ Não reconhecido por padrão | ✅ Reconhecido por navegadores |
| **Custo** | ✅ Gratuito | Pode ter custo |
| **Uso** | Testes, desenvolvimento interno | Produção, sites públicos |
| **Revogação** | ❌ Sem suporte | ✅ Suporte a CRL e OCSP |

---

## Aprendizado
- A geração de um certificado passa por três etapas: chave privada → CSR → certificado
- O CSR contém as informações do titular (Distinguished Name) e a chave pública correspondente à chave privada
- O parâmetro `-aes256` protege a chave privada com senha, sendo recomendado em ambientes de produção
- Certificados autoassinados são funcionalmente equivalentes aos emitidos por CA, mas não possuem cadeia de confiança reconhecida publicamente
