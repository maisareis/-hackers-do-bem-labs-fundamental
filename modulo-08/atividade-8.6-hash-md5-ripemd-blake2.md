# Atividade 8.6 – Computando Hash MD5, RIPEMD-160 e Blake2 no Kali Linux

## Objetivo
Calcular os valores de hash dos algoritmos MD5, RIPEMD-160 e Blake2 de um arquivo de texto no Kali Linux, compreendendo as diferenças entre cada algoritmo.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

Uma **função hash criptográfica** transforma dados de qualquer tamanho em uma sequência de tamanho fixo (o "resumo" ou "digest"). Propriedades essenciais incluem: determinismo (mesma entrada = mesma saída), irreversibilidade e avalanche (qualquer alteração mínima na entrada muda completamente o hash).

| Algoritmo | Tamanho do Hash | Status |
|-----------|----------------|--------|
| **MD5** | 128 bits / 32 hex | ⚠️ Legado (colisões conhecidas) |
| **RIPEMD-160** | 160 bits / 40 hex | ✅ Seguro |
| **Blake2** | 512 bits / 128 hex | ✅ Moderno e rápido |

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- OpenSSL instalado
- Utilitários `md5sum`, `b2sum` disponíveis

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

### 3. Criar o arquivo de teste
```bash
nano mensagem.txt
```
Conteúdo inserido:
```
Hackers do bem!
```
Salvar com `Ctrl+X`, `s`, `Enter`.

### 4. Verificar a presença do arquivo
```bash
ls
```
Resultado esperado:
```
mensagem.txt
```

### 5. Calcular o hash MD5
```bash
md5sum mensagem.txt
```
Resultado esperado:
```
ac640793b0fe42e14c071f53fe8f8486  mensagem.txt
```
> Sequência hexadecimal de 32 dígitos (128 bits).

### 6. Calcular o hash RIPEMD-160
```bash
openssl dgst -ripemd160 mensagem.txt
```
Resultado esperado:
```
RIPEMD-160(mensagem.txt)= 1ec06a94e04fc961bad8957e1fd1f27af9dd421d
```
> Sequência hexadecimal de 40 dígitos (160 bits).

### 7. Calcular o hash Blake2
```bash
b2sum mensagem.txt
```
Resultado esperado:
```
a7158864d19a20e26c1bf13b1a802b73614932d52cea5282e8b0bbc6b6520f322510b1e747a1514429b42a405dc8403c845382859a2b59e34784c736effc3c98  mensagem.txt
```
> Sequência hexadecimal de 128 dígitos (512 bits).

> ⚠️ Não apagar o `mensagem.txt` — será utilizado na Atividade 8.7.

---

## Explicação dos Comandos Principais

| Comando | Algoritmo | Saída |
|---------|-----------|-------|
| `md5sum <arquivo>` | MD5 | 32 hex |
| `openssl dgst -ripemd160 <arquivo>` | RIPEMD-160 | 40 hex |
| `b2sum <arquivo>` | Blake2b | 128 hex |

---

## Aprendizado
- O MD5 é considerado inseguro para uso criptográfico por ter colisões conhecidas, mas ainda é usado para verificação de integridade em contextos não críticos
- O RIPEMD-160 foi desenvolvido como alternativa ao MD5/SHA-1 e é usado em endereços Bitcoin
- O Blake2 é mais rápido que MD5 e SHA-3, sendo moderno e seguro para uso geral
- Qualquer alteração mínima no arquivo resulta em um hash completamente diferente (efeito avalanche)
