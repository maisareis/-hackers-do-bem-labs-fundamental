# Atividade 8.1 – Esteganografia no Kali Linux

## Objetivo
Ocultar o conteúdo de um arquivo de texto dentro de uma imagem JPG utilizando o Steghide, e posteriormente recuperar o texto oculto a partir da imagem.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais. O uso de técnicas de esteganografia em sistemas sem autorização prévia é ilegal.

## Conceito

A **esteganografia** é a técnica de ocultar informações dentro de outros arquivos (imagens, áudios, vídeos) de forma que a existência da mensagem seja imperceptível. Diferente da criptografia, que torna a mensagem ilegível, a esteganografia esconde a própria existência da mensagem.

O **Steghide** é uma ferramenta de esteganografia que permite embutir e extrair dados ocultos em arquivos de imagem (JPEG, BMP) e áudio (WAV, AU), com suporte a proteção por senha.

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Ferramenta `steghide` instalada
- Conexão com a internet para baixar a imagem de cobertura

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Criar a pasta de trabalho
```bash
mkdir -m 777 /home/aluno/Documentos/Arquivos
```

### 3. Acessar a pasta e criar o arquivo de texto
```bash
cd /home/aluno/Documentos/Arquivos
nano mensagem.txt
```
Conteúdo inserido:
```
Hackers do bem com Esteganografia!
```
Salvar com `Ctrl+X`, `s`, `Enter`.

### 4. Baixar a imagem de cobertura
```bash
wget https://upload.wikimedia.org/wikipedia/commons/1/1b/Marcel_Proust_1895-cropped.jpg
```

### 5. Verificar os arquivos presentes
```bash
ls
```
Resultado esperado:
```
Marcel_Proust_1895-cropped.jpg  mensagem.txt
```

### 6. Ocultar o arquivo de texto na imagem
```bash
steghide embed -cf Marcel_Proust_1895-cropped.jpg -ef mensagem.txt
```
Informar senha quando solicitado. Neste lab foi usada a senha `abcd1234`. A imagem é sobrescrita com os dados ocultos.

### Parâmetros do steghide embed

| Parâmetro | Descrição |
|-----------|-----------|
| `embed` | Operação de incorporação de dados |
| `-cf` | Cover file: arquivo de cobertura (imagem) |
| `-ef` | Embed file: arquivo a ser ocultado |

### 7. Apagar o arquivo original e confirmar
```bash
rm mensagem.txt
ls
```
Resultado esperado:
```
Marcel_Proust_1895-cropped.jpg
```

### 8. Extrair os dados ocultos da imagem
```bash
steghide extract -sf Marcel_Proust_1895-cropped.jpg
```
Informar a senha utilizada na incorporação (`abcd1234`).

### Parâmetros do steghide extract

| Parâmetro | Descrição |
|-----------|-----------|
| `extract` | Operação de extração de dados |
| `-sf` | Stego file: arquivo que contém os dados ocultos |

### 9. Verificar o arquivo recuperado e seu conteúdo
```bash
ls
cat mensagem.txt
```
Resultado esperado:
```
Marcel_Proust_1895-cropped.jpg  mensagem.txt

Hackers do bem com Esteganografia!
```

### 10. Limpeza da pasta de trabalho
```bash
rm -r *
ls
```

---

## Explicação dos Comandos Principais

| Comando | Descrição |
|---------|-----------|
| `steghide embed -cf <imagem> -ef <arquivo>` | Oculta um arquivo dentro de uma imagem |
| `steghide extract -sf <imagem>` | Extrai dados ocultos de uma imagem |
| `wget <url>` | Baixa arquivo da internet |

---

## Aprendizado
- A esteganografia oculta a existência da mensagem, diferente da criptografia que apenas a torna ilegível
- O Steghide suporta proteção por senha, adicionando uma camada extra de segurança
- A imagem de cobertura é sobrescrita com os dados ocultos, mantendo o mesmo nome de arquivo
- Combinar esteganografia com criptografia oferece proteção ainda mais robusta
