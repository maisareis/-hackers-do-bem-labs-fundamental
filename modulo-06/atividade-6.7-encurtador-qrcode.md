# Atividade 6.7 – Encurtando Links e Gerando QR Code

## Objetivo
Utilizar o Kali Linux para encurtar URLs via API e gerar QR Codes a partir dos links encurtados.

## Conceitos

### Encurtador de URLs
Um **encurtador de URL** é um serviço que converte URLs longas em URLs curtas, facilitando o compartilhamento.

### QR Code
O **QR Code** (Quick Response Code) é um código de barras bidimensional que pode armazenar informações como URLs, textos e dados de contato. Pode ser lido por câmeras de smartphones.

---

## Ferramentas Utilizadas

| Ferramenta | Descrição |
|-----------|-----------|
| `curl` | Requisição HTTP para a API do TinyURL |
| `qrencode` | Geração de QR Codes no Linux |

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Encurtar uma URL via API do TinyURL
```bash
curl -s -i "https://tinyurl.com/api-create.php?url=https://esr.rnp.br/cursos/?_formacao_cursos=seguranca"
```
> O resultado será uma URL encurtada no formato `http://tinyurl.com/xxxxxxx`

### 3. Navegar até a pasta Documentos
```bash
cd /home/aluno/Documentos
```

### 4. Gerar o QR Code
```bash
qrencode -o qr_code.png "https://tinyurl.com/XXXXXXX"
```

### Parâmetros do qrencode

| Parâmetro | Descrição |
|-----------|-----------|
| `-o arquivo.png` | Define o nome do arquivo de saída |
| `"URL"` | URL ou texto a ser codificado no QR Code |

### 5. Visualizar o QR Code
Abrir o arquivo `qr_code.png` no gerenciador de arquivos Thunar.

---

## Como Funciona a API do TinyURL
A API recebe uma URL longa como parâmetro e retorna uma URL curta:
```
https://tinyurl.com/api-create.php?url=URL_LONGA
```

---

## Aprendizado
- APIs podem ser consumidas diretamente pelo `curl` na linha de comando
- O `qrencode` permite gerar QR Codes no Linux sem interface gráfica
- QR Codes são úteis para compartilhar links de forma rápida
- Encurtadores de URL facilitam o compartilhamento de links longos
