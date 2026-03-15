# Atividade 8.10 – Assinatura Digital com RSA-2048 no Kali Linux

## Objetivo
Assinar digitalmente um arquivo utilizando RSA-2048 com SHA-256, verificar a assinatura de um arquivo legítimo e demonstrar a falha de verificação em um arquivo adulterado.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

A **assinatura digital** garante **autenticidade** (quem assinou) e **integridade** (o conteúdo não foi alterado). Funciona de forma inversa à cifragem para confidencialidade:

- A **chave privada** é usada para **assinar** (apenas o titular pode assinar)
- A **chave pública** é usada para **verificar** (qualquer um pode verificar)

### Processo de assinatura digital

```
Arquivo original → SHA-256 → hash → cifrado com chave privada → assinatura.bin
```

### Processo de verificação

```
Arquivo recebido → SHA-256 → hash A
assinatura.bin → decifrado com chave pública → hash B
Se hash A == hash B → "Verified OK"
Se hash A != hash B → "Verification failure"
```

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- OpenSSL instalado
- Arquivo `mensagem.txt` presente da Atividade 8.6

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Acessar a pasta de trabalho e verificar o arquivo
```bash
cd /home/aluno/Documentos/Arquivos
ls
```
Resultado esperado:
```
mensagem.txt
```

### 3. Gerar a chave privada RSA-2048
```bash
openssl genpkey -algorithm RSA -out chave_privada.pem -pkeyopt rsa_keygen_bits:2048
```

### 4. Extrair a chave pública e verificar os arquivos criados
```bash
openssl rsa -pubout -in chave_privada.pem -out chave_publica.pem
ls
```
Resultado esperado:
```
chave_privada.pem  chave_publica.pem  mensagem.txt
```

### 5. Assinar o arquivo com a chave privada
```bash
openssl dgst -sha256 -sign chave_privada.pem -out assinatura.bin mensagem.txt
ls
```
Resultado esperado:
```
assinatura.bin  chave_privada.pem  chave_publica.pem  mensagem.txt
```

### Parâmetros do comando de assinatura

| Parâmetro | Descrição |
|-----------|-----------|
| `dgst` | Operação de digest/hash |
| `-sha256` | Algoritmo de hash usado na assinatura |
| `-sign chave_privada.pem` | Chave privada usada para assinar |
| `-out assinatura.bin` | Arquivo de saída com a assinatura digital |
| `mensagem.txt` | Arquivo a ser assinado |

### 6. Verificar a assinatura do arquivo legítimo
```bash
openssl dgst -sha256 -verify chave_publica.pem -signature assinatura.bin mensagem.txt
```
Resultado esperado:
```
Verified OK
```

### Parâmetros do comando de verificação

| Parâmetro | Descrição |
|-----------|-----------|
| `-verify chave_publica.pem` | Chave pública usada para verificar |
| `-signature assinatura.bin` | Arquivo com a assinatura digital |
| `mensagem.txt` | Arquivo original a ser verificado |

### 7. Criar arquivo adulterado para teste
```bash
nano mensagem_errada.txt
```
Conteúdo inserido:
```
Hackers do mal!
```
Salvar com `Ctrl+X`, `s`, `Enter`.

### 8. Confirmar a criação do arquivo adulterado
```bash
ls
```
Resultado esperado:
```
assinatura.bin  chave_privada.pem  chave_publica.pem  mensagem_errada.txt  mensagem.txt
```

### 9. Verificar a assinatura com o arquivo adulterado
```bash
openssl dgst -sha256 -verify chave_publica.pem -signature assinatura.bin mensagem_errada.txt
```
Resultado esperado:
```
Verification failure
```
A assinatura gerada para `mensagem.txt` não é válida para `mensagem_errada.txt`, demonstrando que qualquer alteração no conteúdo invalida a assinatura.

### 10. Limpeza da pasta de trabalho
```bash
rm -r *
ls
```

---

## Diferença entre Confidencialidade e Autenticidade com RSA

| Objetivo | Cifra com | Decifra/Verifica com |
|----------|-----------|---------------------|
| **Confidencialidade** | Chave pública do destinatário | Chave privada do destinatário |
| **Autenticidade (assinatura)** | Chave privada do remetente | Chave pública do remetente |

---

## Resultado da Verificação

| Cenário | Resultado |
|---------|-----------|
| Arquivo original + assinatura correta | `Verified OK` |
| Arquivo adulterado + mesma assinatura | `Verification failure` |
| Assinatura de outra chave privada | `Verification failure` |

---

## Aprendizado
- A assinatura digital garante integridade (conteúdo não alterado) e autenticidade (quem assinou)
- Qualquer alteração mínima no arquivo, mesmo de um único byte, invalida completamente a assinatura
- A chave privada assina; a chave pública verifica — o inverso da cifragem para confidencialidade
- O parâmetro `-pkeyopt rsa_keygen_bits:2048` especifica explicitamente o tamanho da chave RSA
- Assinaturas digitais são a base de certificados SSL/TLS, e-mails seguros (S/MIME) e blockchains
