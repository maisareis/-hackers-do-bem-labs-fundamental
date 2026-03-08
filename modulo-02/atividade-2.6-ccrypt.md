# Atividade 2.6 – Criptografia de Dados com Ccrypt

## Objetivo
Utilizar a ferramenta Ccrypt para cifrar e decifrar arquivos no Kali Linux como exemplo de controle técnico de proteção de dados.

## Conceito

### Controle Técnico de Criptografia
A **criptografia de dados** é um controle técnico que protege a **confidencialidade** das informações. Mesmo que um arquivo seja acessado sem autorização, seu conteúdo permanece ilegível sem a chave correta.

### Ccrypt
O **Ccrypt** é uma ferramenta de linha de comando para criptografia e descriptografia de arquivos. Utiliza o algoritmo **Rijndael** (base do AES) com chave de 256 bits para proteger os dados.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Criar arquivo de texto
```bash
cd /home/aluno/Documentos/
nano mensagem.txt
# Inserir: Hackers do bem!
# Salvar: Ctrl+X → s → Enter
```

### 3. Ver opções do ccrypt
```bash
ccrypt -h
```

**Modos principais:**
```
-e, --encrypt    criptografar
-d, --decrypt    descriptografar
-c, --cat        descriptografar para stdout
-x, --keychange  trocar a chave
```

### 4. Criptografar o arquivo
```bash
ccrypt -e mensagem.txt
# Enter encryption key: ****
# Enter encryption key: (repeat) ****
```

### 5. Verificar o resultado
```bash
ls
# mensagem.txt.cpt   ← arquivo original foi substituído

cat mensagem.txt.cpt
# Z▯w▯.▯▯▯▯K▯▯^▯▯▯▯▯▯▯>▯I6E9Y▯i▯▯8Z;mU\▯;X▯
```
O conteúdo está completamente ilegível.

### 6. Descriptografar o arquivo
```bash
ccrypt -d mensagem.txt.cpt
# Enter decryption key: ****
```

### 7. Verificar recuperação
```bash
ls
# mensagem.txt   ← arquivo original recuperado

cat mensagem.txt
# Hackers do bem!
```

---

## Parâmetros do Ccrypt

| Parâmetro | Descrição |
|-----------|-----------|
| `-e` | Criptografar arquivo |
| `-d` | Descriptografar arquivo |
| `-r` | Recursivo (para pastas) |
| `-f` | Forçar sobrescrita |
| `-K key` | Fornecer senha pela linha de comando |
| `-k file` | Ler senha de um arquivo |

---

## Extensões de Arquivo

| Estado | Extensão |
|--------|----------|
| Original | `.txt`, `.pdf`, `.docx`, etc. |
| Criptografado | `.txt.cpt`, `.pdf.cpt`, etc. |

---

## Algoritmo Utilizado

O Ccrypt utiliza o **Rijndael** (base do AES - Advanced Encryption Standard):
- Chave de **256 bits**
- Modo de operação: **CFB (Cipher Feedback)**
- Considerado seguro para uso atual

---

## Aprendizado
- Criptografia protege dados mesmo que o arquivo seja acessado sem autorização
- O Ccrypt substitui o arquivo original pelo arquivo criptografado (`.cpt`)
- Sem a senha correta, o conteúdo é completamente ilegível
- A senha deve ser forte e armazenada com segurança — sem ela, os dados são irrecuperáveis
