# Atividade 8.7 – Computando Hash SHA-1, SHA-2 e SHA-3 no Kali Linux

## Objetivo
Calcular os valores de hash da família SHA (SHA-1, SHA-2 de 512 bits e SHA-3 de 512 bits) de um arquivo de texto no Kali Linux, compreendendo a evolução da família de algoritmos.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

A família **SHA** (Secure Hash Algorithm) é desenvolvida pelo NIST (Instituto Nacional de Padrões e Tecnologia dos EUA). Cada versão representa uma evolução em segurança e tamanho de saída.

| Algoritmo | Tamanho do Hash | Status |
|-----------|----------------|--------|
| **SHA-1** | 160 bits / 40 hex | ⚠️ Legado (colisões conhecidas) |
| **SHA-2 (512 bits)** | 512 bits / 128 hex | ✅ Padrão atual |
| **SHA-3 (512 bits)** | 512 bits / 128 hex | ✅ Moderno (estrutura diferente do SHA-2) |

> SHA-3 usa uma estrutura interna completamente diferente do SHA-2 (construção Keccak/esponja), tornando-os complementares em segurança.

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Utilitários `sha1sum`, `sha512sum`, `sha3sum` disponíveis
- Arquivo `mensagem.txt` presente da Atividade 8.6

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

### 3. Verificar a presença do arquivo
```bash
ls
```
Resultado esperado:
```
mensagem.txt
```

### 4. Calcular o hash SHA-1
```bash
sha1sum mensagem.txt
```
Resultado esperado:
```
287f168af5acf44b3b06a07fb6f3a4e882a9baab  mensagem.txt
```
> Sequência hexadecimal de 40 dígitos (160 bits).

### 5. Calcular o hash SHA-2 de 512 bits
```bash
sha512sum mensagem.txt
```
Resultado esperado:
```
d0774282912bf4b5479165a9b316f23fea1f9ae3f620f400618813347a13c8d1e297fb51196def065dafe0aa21239d719f51a8cf58410bac22fc4b40b29efeab  mensagem.txt
```
> Sequência hexadecimal de 128 dígitos (512 bits).

### 6. Calcular o hash SHA-3 de 512 bits
```bash
sha3sum -b -a 512 mensagem.txt
```
Resultado esperado:
```
07ad8830012228703f01d1961a759c907214a617c2c42fade253af6bf42bf4500a54038f1fb960165e6cb4985fed141ed90b949f7915d2379c7a67d5484930f1 *mensagem.txt
```
> Sequência hexadecimal de 128 dígitos (512 bits).

### Parâmetros do sha3sum

| Parâmetro | Descrição |
|-----------|-----------|
| `-b` | Exibe resultado em formato binário |
| `-a 512` | Usa a variante de 512 bits do SHA-3 |

> ⚠️ Não apagar o `mensagem.txt` — será utilizado nas Atividades 8.8, 8.9 e 8.10.

---

## Explicação dos Comandos Principais

| Comando | Algoritmo | Saída |
|---------|-----------|-------|
| `sha1sum <arquivo>` | SHA-1 | 40 hex |
| `sha512sum <arquivo>` | SHA-2 512 bits | 128 hex |
| `sha3sum -b -a 512 <arquivo>` | SHA-3 512 bits | 128 hex |

---

## Comparativo completo de algoritmos de hash

| Algoritmo | Bits | Hex | Uso recomendado |
|-----------|------|-----|-----------------|
| MD5 | 128 | 32 | ❌ Não usar para segurança |
| SHA-1 | 160 | 40 | ❌ Não usar para segurança |
| RIPEMD-160 | 160 | 40 | ✅ Uso específico (ex: Bitcoin) |
| Blake2 | 512 | 128 | ✅ Uso geral moderno |
| SHA-2 256 | 256 | 64 | ✅ Padrão amplamente adotado |
| SHA-2 512 | 512 | 128 | ✅ Alta segurança |
| SHA-3 512 | 512 | 128 | ✅ Estrutura alternativa ao SHA-2 |

---

## Aprendizado
- SHA-1 é considerado inseguro desde 2017 quando colisões foram demonstradas publicamente
- SHA-2 e SHA-3 produzem hashes do mesmo tamanho (512 bits), mas com estruturas internas completamente diferentes
- SHA-3 foi desenvolvido como alternativa ao SHA-2 para o caso de vulnerabilidades futuras serem descobertas
- Para verificação de integridade de arquivos, SHA-256 ou SHA-512 são os padrões mais utilizados atualmente
