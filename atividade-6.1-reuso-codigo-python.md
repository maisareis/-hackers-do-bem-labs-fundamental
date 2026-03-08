# Atividade 6.1 – Reuso de Código em Python

## Objetivo
Demonstrar o conceito de reuso de código em Python utilizando a biblioteca Pillow para redimensionamento de imagens.

## Conceito
O **reuso de código** consiste em utilizar componentes de software já testados e validados, em vez de criar funcionalidades do zero. Neste exemplo, utilizamos a biblioteca **Pillow** que já implementa todas as funções necessárias para manipulação de imagens.

---

## Pré-requisitos
- Kali Linux com Terminal
- Biblioteca Pillow instalada (`pip install pillow`)

---

## Código Utilizado

```python
from PIL import Image

# Abre uma imagem existente
imagem_original = Image.open("My_shoe.jpg")

# Redimensiona a imagem para 300x300 pixels
imagem_redimensionada = imagem_original.resize((300, 300))

# Salva a imagem redimensionada
imagem_redimensionada.save("My_shoe_redimensionado.jpg")

# Fecha a imagem original
imagem_original.close()
```

## Explicação do Código

| Linha | Descrição |
|-------|-----------|
| `from PIL import Image` | Importa a classe Image da biblioteca Pillow |
| `Image.open()` | Abre a imagem original |
| `imagem_original.resize((300, 300))` | Redimensiona para 300x300 pixels |
| `imagem_redimensionada.save()` | Salva a imagem redimensionada |
| `imagem_original.close()` | Libera recursos após uso |

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Navegar até a pasta Documentos
```bash
cd /home/aluno/Documentos
```

### 3. Baixar a imagem de teste
```bash
wget https://upload.wikimedia.org/wikipedia/commons/0/01/My_shoe.jpg
```

### 4. Criar o script Python
Abrir o editor de texto e salvar o código acima como `redimensionar_imagem.py`.

### 5. Executar o script
```bash
python redimensionar_imagem.py
```

### 6. Verificar o resultado
```bash
ls
# My_shoe.jpg  My_shoe_redimensionado.jpg  redimensionar_imagem.py
```

---

## Resultado Esperado
Dois arquivos de imagem na pasta: o original e o redimensionado para 300x300 pixels.

## Aprendizado
- Reuso de código economiza tempo e evita erros
- Bibliotecas como Pillow abstraem operações complexas de manipulação de imagens
- Sempre fechar recursos após uso (`close()`) é uma boa prática
