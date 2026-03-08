# Atividade 1.1 – Comparando Integridade de Arquivos de Texto

## Objetivo
Utilizar o comando `diff` no Kali Linux para comparar arquivos de texto e detectar modificações.

## Conceito
A **verificação de integridade** garante que um arquivo não foi modificado desde sua criação ou última verificação. O comando `diff` compara dois arquivos linha por linha e exibe as diferenças encontradas.

---

## Passo a Passo

### 1. Criar o arquivo de texto original
Abrir o editor de texto Mousepad e salvar o conteúdo como `Texto.txt` na pasta Documentos.

### 2. Acessar como superusuário
```bash
sudo -i
```

### 3. Navegar até a pasta Documentos
```bash
cd /home/aluno/Documentos/
ls
# Texto.txt
```

### 4. Copiar o arquivo original
```bash
cp Texto.txt Texto_copia.txt
ls
# Texto_copia.txt  Texto.txt
```

### 5. Verificar que os arquivos são idênticos
```bash
diff Texto.txt Texto_copia.txt
# Sem retorno = arquivos idênticos
```

### 6. Modificar o arquivo copiado
```bash
nano Texto_copia.txt
# Alterar a primeira letra "L" por "P"
```

### 7. Verificar a diferença
```bash
diff Texto.txt Texto_copia.txt
```

**Saída esperada:**
```
1c1
< Lorem ipsum dolor sit amet...
---
> Porem ipsum dolor sit amet...
```

---

## Interpretando a saída do diff

| Símbolo | Significado |
|---------|-------------|
| `1c1` | Linha 1 foi alterada (c = changed) |
| `<` | Conteúdo do arquivo original |
| `---` | Separador entre original e modificado |
| `>` | Conteúdo do arquivo modificado |

---

## Aprendizado
- `diff` detecta qualquer alteração em arquivos de texto
- Ausência de saída indica que os arquivos são idênticos
- Útil para verificar se arquivos foram manipulados
