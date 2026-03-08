# Atividade 1.2 – Comparando Integridade de Arquivos Genéricos

## Objetivo
Utilizar o comando `cmp` no Kali Linux para comparar arquivos binários (imagens, PDFs, etc.) e detectar modificações.

## Conceito
O comando `cmp` compara dois arquivos **byte a byte** e informa a posição exata onde a primeira diferença foi encontrada. Diferente do `diff`, funciona com qualquer tipo de arquivo, não apenas texto.

---

## Diferença entre diff e cmp

| Comando | Tipo de arquivo | O que mostra |
|---------|----------------|--------------|
| `diff` | Texto | Linhas diferentes |
| `cmp` | Qualquer (binário, imagem, etc.) | Byte e linha da primeira diferença |

---

## Passo a Passo

### 1. Criar imagens de teste
Abrir o programa KolourPaint e criar duas imagens:
- `Imagem1.png` → imagem original
- `Imagem1_copia.png` → cópia idêntica da original
- `Imagem2.png` → imagem com um ponto extra

### 2. Verificar arquivos presentes
```bash
cd /home/aluno/Documentos
ls
# Imagem1_copia.png  Imagem1.png  Imagem2.png  Texto_copia.txt  Texto.txt
```

### 3. Comparar arquivos idênticos
```bash
cmp Imagem1.png Imagem1_copia.png
# Sem retorno = arquivos idênticos
```

### 4. Comparar arquivos diferentes
```bash
cmp Imagem1.png Imagem2.png
```

**Saída esperada:**
```
Imagem1.png Imagem2.png são diferentes: byte 58, linha 3
```

---

## Interpretando a saída do cmp

| Campo | Significado |
|-------|-------------|
| `são diferentes` | Os arquivos possuem conteúdo diferente |
| `byte 58` | A primeira diferença está no byte 58 |
| `linha 3` | Posição aproximada no arquivo |

---

## Aprendizado
- `cmp` funciona com qualquer tipo de arquivo, incluindo binários
- Detecta diferenças mesmo em arquivos visualmente idênticos
- Útil para verificar integridade de imagens, executáveis e documentos
