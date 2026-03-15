# Atividade 8.9 – Criptografia Assimétrica ECC-256 para Confidencialidade no Kali Linux

## Objetivo
Cifrar e decifrar um arquivo utilizando criptografia assimétrica ECC-256 em conjunto com AES-256-CBC no Kali Linux, compreendendo o esquema híbrido necessário para uso prático do ECC.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **ECC** (Elliptic Curve Cryptography) é baseado na matemática de curvas elípticas sobre corpos finitos. Com chaves bem menores que o RSA, oferece segurança equivalente ou superior com menor custo computacional.

O **ECC-256** usa a curva `prime256v1` (também conhecida como P-256 ou secp256r1), com chave de 256 bits equivalente em segurança a um RSA de ~3072 bits.

> ⚠️ **Limitação importante:** O ECC não cifra dados diretamente. Ele é usado para derivar um segredo compartilhado (via ECDH), que serve como chave para um algoritmo simétrico — neste caso, AES-256-CBC.

### Esquema Híbrido utilizado

```
ECC (assimétrico) → gera chave de sessão → AES-256-CBC (simétrico) → cifra o arquivo
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

### 3. Gerar o par de chaves ECC-256
```bash
openssl ecparam -name prime256v1 -genkey -noout -out chave_privada.pem
openssl ec -in chave_privada.pem -pubout -out chave_publica.pem
ls
```
Resultado esperado:
```
chave_privada.pem  chave_publica.pem  mensagem.txt
```

### Parâmetros da geração de chaves ECC

| Parâmetro | Descrição |
|-----------|-----------|
| `ecparam -name prime256v1` | Seleciona a curva elíptica P-256 |
| `-genkey` | Gera o par de chaves |
| `-noout` | Não exibe os parâmetros da curva |
| `ec -pubout` | Extrai a chave pública da chave privada |

### 4. Gerar chave de sessão e derivar chave compartilhada
```bash
openssl rand -out chave_sessao.bin 32
openssl pkeyutl -derive -inkey chave_privada.pem -peerkey chave_publica.pem -out chave_compartilhada.bin
ls
```

### 5. Cifrar o arquivo com AES-256-CBC usando a chave de sessão
```bash
openssl enc -aes-256-cbc -salt -in mensagem.txt -out arquivo_criptografado -pass file:chave_sessao.bin
ls
```
Resultado esperado:
```
arquivo_criptografado  chave_compartilhada.bin  chave_privada.pem  chave_publica.pem  chave_sessao.bin  mensagem.txt
```

### Descrição dos arquivos gerados

| Arquivo | Descrição |
|---------|-----------|
| `chave_sessao.bin` | Chave aleatória de 32 bytes usada para cifrar o arquivo |
| `chave_compartilhada.bin` | Segredo ECDH derivado do par de chaves |
| `arquivo_criptografado` | Arquivo cifrado com AES-256-CBC |

### 6. Decifrar o arquivo e verificar o resultado
```bash
openssl enc -d -aes-256-cbc -in arquivo_criptografado -out mensagem_recuperada.txt -pass file:chave_sessao.bin
ls
```
Resultado esperado:
```
arquivo_criptografado    chave_privada.pem  chave_sessao.bin         mensagem.txt
chave_compartilhada.bin  chave_publica.pem  mensagem_recuperada.txt
```

### 7. Confirmar a recuperação da mensagem
```bash
cat mensagem_recuperada.txt
```
Resultado esperado:
```
Hackers do bem!
```

### 8. Limpeza (manter apenas o mensagem.txt)
```bash
rm arquivo_criptografado chave_privada.pem chave_sessao.bin chave_compartilhada.bin chave_publica.pem mensagem_recuperada.txt
ls
```

---

## Comparativo ECC vs RSA

| | ECC-256 | RSA-2048 |
|---|---|---|
| **Segurança equivalente** | ~3072 bits RSA | 2048 bits |
| **Tamanho da chave** | ✅ Menor | ❌ Maior |
| **Velocidade** | ✅ Mais rápido | ❌ Mais lento |
| **Cifra direta** | ❌ Não | ✅ Sim (arquivos pequenos) |
| **Uso moderno** | ✅ TLS, SSH, blockchain | ✅ SSL/TLS legado |

---

## Aprendizado
- O ECC não cifra dados diretamente; é necessário o esquema híbrido com AES
- A curva `prime256v1` (P-256) é a mais utilizada em implementações modernas como TLS 1.3
- A `chave_sessao.bin` deve ser protegida com o mesmo rigor da chave privada
- ECC oferece segurança equivalente ao RSA com chaves significativamente menores, sendo mais eficiente em dispositivos com recursos limitados
