# Atividade 6.6 – Solicitações HTTP e Codificação Percentual

## Objetivo
Explorar o comando `curl` para fazer solicitações HTTP e entender a codificação percentual (percent encoding) utilizada em URLs.

## Conceitos

### curl
O **curl** é uma ferramenta de linha de comando para transferência de dados com servidores usando diversos protocolos (HTTP, HTTPS, FTP, etc.).

### Codificação Percentual (Percent Encoding)
A **codificação percentual** é um mecanismo para representar caracteres especiais em URLs. Caracteres que não são permitidos diretamente em URLs são substituídos por `%` seguido de dois dígitos hexadecimais.

Exemplos comuns:

| Caractere | Codificação |
|-----------|-------------|
| espaço | `%20` |
| `á` | `%C3%A1` |
| `!` | `%21` |
| `,` | `%2C` |

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Testar o curl com uma URL simples
```bash
curl https://www.example.com
```
> Retorna o HTML da página diretamente no terminal.

### 3. Baixar um arquivo com curl
```bash
cd /home/aluno/Documentos
curl -o output.html https://www.example.com
```

### Parâmetros do curl utilizados

| Parâmetro | Descrição |
|-----------|-----------|
| `-o arquivo` | Salva a saída em um arquivo |
| `-s` | Modo silencioso (sem barra de progresso) |
| `-i` | Inclui cabeçalhos HTTP na saída |

### 4. Testar codificação percentual
```bash
curl "https://www.google.com/search?q=Ol%C3%A1%2C%20mundo%21"
```

A URL acima decodificada representa a busca por: **"Olá, mundo!"**

| Parte codificada | Caractere real |
|-----------------|----------------|
| `%C3%A1` | `á` |
| `%2C` | `,` |
| `%20` | espaço |
| `%21` | `!` |

---

## Aprendizado
- O `curl` é amplamente usado para testes de APIs e download de arquivos
- A codificação percentual permite incluir caracteres especiais em URLs
- Navegadores automaticamente codificam e decodificam URLs para o usuário
