# Atividade 1.3 – Verificando Integridade com Hash MD5

## Objetivo
Utilizar o comando `md5sum` para calcular e comparar hashes MD5 de arquivos, verificando sua integridade.

## Conceito

### Função Hash
Uma **função hash** é um algoritmo matemático que transforma qualquer dado em uma sequência fixa de caracteres hexadecimais. Suas propriedades principais são:

- **Determinística:** o mesmo arquivo sempre gera o mesmo hash
- **Efeito avalanche:** qualquer alteração mínima gera um hash completamente diferente
- **Irreversível:** não é possível recuperar o arquivo original a partir do hash

### MD5 (Message Digest 5)
O **MD5** gera um hash de 128 bits (32 caracteres hexadecimais). Apesar de não ser mais recomendado para criptografia, ainda é amplamente usado para verificação de integridade.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Navegar até a pasta Documentos
```bash
cd /home/aluno/Documentos/
```

### 3. Calcular o hash MD5 dos arquivos
```bash
md5sum Imagem1.png
md5sum Imagem1_copia.png
md5sum Imagem2.png
md5sum Texto.txt
md5sum Texto_copia.txt
```

**Resultado esperado:**
```
ffee4a00274267c6dfd27f0607f6b345  Imagem1.png
ffee4a00274267c6dfd27f0607f6b345  Imagem1_copia.png  ← hashes iguais!
702f2024904fd727d975210b692d9032  Imagem2.png
e21678c114a62e8293e9f87bbd0b9235  Texto.txt
52ffef02ffe8f762147bc72b53e3bb0f  Texto_copia.txt
```

---

## Análise dos Resultados

| Arquivos | Hashes | Conclusão |
|----------|--------|-----------|
| Imagem1.png e Imagem1_copia.png | **Iguais** | Arquivos idênticos |
| Imagem1.png e Imagem2.png | **Diferentes** | Arquivo foi modificado |
| Texto.txt e Texto_copia.txt | **Diferentes** | Arquivo foi modificado |

---

## Outros Algoritmos Hash

| Algoritmo | Tamanho | Comando Linux |
|-----------|---------|---------------|
| MD5 | 128 bits | `md5sum` |
| SHA-1 | 160 bits | `sha1sum` |
| SHA-256 | 256 bits | `sha256sum` |
| SHA-512 | 512 bits | `sha512sum` |

---

## Aprendizado
- Hashes iguais indicam arquivos idênticos
- Qualquer alteração, por menor que seja, gera um hash completamente diferente
- MD5 é útil para verificação de integridade mas não para criptografia segura
- SHA-256 e SHA-512 são mais seguros e recomendados atualmente
