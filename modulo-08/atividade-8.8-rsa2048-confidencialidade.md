# Atividade 8.8 – Criptografia Assimétrica RSA-2048 para Confidencialidade no Kali Linux

## Objetivo
Cifrar e decifrar um arquivo utilizando criptografia assimétrica RSA-2048 com o OpenSSL no Kali Linux, compreendendo o papel do par de chaves pública e privada.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

A **criptografia assimétrica** utiliza um par de chaves matematicamente relacionadas: a **chave pública** (compartilhada livremente) e a **chave privada** (mantida em segredo).

No contexto de **confidencialidade**:
- A **chave pública** é usada para **cifrar** os dados
- A **chave privada** é usada para **decifrar** os dados

O **RSA-2048** é o algoritmo assimétrico mais utilizado no mundo, baseado na dificuldade de fatorar números primos grandes. Com 2048 bits, oferece segurança robusta para a maioria das aplicações atuais.

> ⚠️ O RSA não é adequado para cifrar arquivos grandes diretamente. Na prática, é usado para cifrar a chave de sessão de algoritmos simétricos (como AES).

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

### 3. Gerar o par de chaves RSA-2048 e verificar os arquivos criados
```bash
openssl genpkey -algorithm RSA -out chave_privada.pem
openssl rsa -pubout -in chave_privada.pem -out chave_publica.pem
ls
```
Resultado esperado:
```
chave_privada.pem  chave_publica.pem  mensagem.txt
```

### 4. Cifrar o arquivo com a chave pública
```bash
openssl pkeyutl -encrypt -pubin -inkey chave_publica.pem -in mensagem.txt -out arquivo_criptografado
ls
```

### Parâmetros do comando de cifragem

| Parâmetro | Descrição |
|-----------|-----------|
| `pkeyutl` | Operações de chave pública/privada |
| `-encrypt` | Realiza operação de cifragem |
| `-pubin` | Indica que a chave fornecida é pública |
| `-inkey` | Arquivo da chave a ser usada |
| `-in` | Arquivo de entrada |
| `-out` | Arquivo de saída cifrado |

### 5. Decifrar o arquivo com a chave privada e verificar o resultado
```bash
openssl pkeyutl -decrypt -inkey chave_privada.pem -in arquivo_criptografado -out mensagem_recuperada.txt
ls
```
Resultado esperado:
```
arquivo_criptografado  chave_privada.pem  chave_publica.pem  mensagem_recuperada.txt  mensagem.txt
```

### Parâmetros do comando de decifragem

| Parâmetro | Descrição |
|-----------|-----------|
| `-decrypt` | Realiza operação de decifragem |
| `-inkey` | Arquivo da chave privada |
| `-in` | Arquivo cifrado de entrada |
| `-out` | Arquivo decifrado de saída |

### 6. Confirmar a recuperação da mensagem
```bash
cat mensagem_recuperada.txt
```
Resultado esperado:
```
Hackers do bem!
```

### 7. Limpeza (manter apenas o mensagem.txt)
```bash
rm arquivo_criptografado chave_privada.pem chave_publica.pem mensagem_recuperada.txt
ls
```

---

## Fluxo da Criptografia Assimétrica para Confidencialidade

```
Remetente                          Destinatário
    |                                   |
    |  chave_publica.pem (pública)       |
    |<----------------------------------|
    |                                   |
    | cifra com chave_publica.pem       |
    |---------------------------------->|
    |  arquivo_criptografado            |
    |                                   |
    |                    decifra com    |
    |                chave_privada.pem  |
```

---

## Aprendizado
- A chave pública pode ser compartilhada livremente; a chave privada nunca deve sair do poder do destinatário
- O RSA é adequado para arquivos pequenos; arquivos grandes exigem o esquema híbrido (RSA + AES)
- A segurança do RSA-2048 depende da proteção rigorosa da chave privada
- Arquivos `.pem` são o formato padrão para armazenar chaves criptográficas com OpenSSL
