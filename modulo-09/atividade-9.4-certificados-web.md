# Atividade 9.4 – Avaliando Certificados Web no Windows Server 2022

## Objetivo
Avaliar certificados digitais de sites reais utilizando o Microsoft Edge no Windows Server 2022 (cliente), exportar e visualizar a estrutura de um certificado no formato PEM.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

Os **certificados web (TLS/SSL)** são usados pelos sites para estabelecer conexões seguras HTTPS. Eles são emitidos por CAs públicas e contêm informações sobre o domínio, emissor e validade.

O **formato PEM** (Privacy Enhanced Mail) é a representação em Base64 de um certificado, delimitado por `-----BEGIN CERTIFICATE-----` e `-----END CERTIFICATE-----`. É o formato mais comum para armazenar e transportar certificados.

O **cadeado** no navegador indica que a conexão é protegida por TLS e que o certificado do site é válido e confiável.

---

## Pré-requisitos
- Windows Server 2022 (cliente) com acesso à internet
- Microsoft Edge disponível na barra de tarefas

---

## Passo a Passo

### 1. Abrir o Microsoft Edge
Clicar no ícone do Microsoft Edge na barra de tarefas.

### 2. Acessar o site do Google
Na barra de endereços, navegar para:
```
https://www.google.com/
```

### 3. Acessar as informações de conexão
Clicar no **símbolo do cadeado** à esquerda da barra de endereços.

### 4. Ver detalhes da conexão segura
Clicar em **Connection is secure**.

### 5. Abrir o certificado
Clicar no **ícone de certificado** à esquerda do botão X.

### 6. Analisar a aba General
Ler os campos:

| Campo | Descrição |
|-------|-----------|
| **Issued To** | Domínio para o qual o certificado foi emitido |
| **Issued By** | CA que emitiu o certificado |
| **Validity Period** | Período de validade do certificado |
| **Fingerprints** | Hash único que identifica o certificado |

### 7. Analisar a aba Detail
Ler os campos:

| Campo | Descrição |
|-------|-----------|
| **Certificate Hierarchy** | Cadeia de confiança até a Root CA |
| **Certificate Fields** | Todos os campos técnicos do certificado X.509 |

### 8. Exportar o certificado
Ainda na aba **Detail**, clicar em **Export...** e salvar na pasta **Downloads**.

### 9. Abrir o Notepad
Na barra de tarefas, pesquisar por `notepad` e abrir.

### 10. Abrir o certificado exportado
No Notepad, clicar em **File** → **Open** → navegar até a pasta **Downloads**.

### 11. Mudar o filtro de arquivos
Na janela de abertura, mudar o filtro de `Text documents (*.txt)` para `All files`.

### 12. Selecionar o certificado
Clicar 2 vezes no arquivo do certificado exportado.

### 13. Visualizar o certificado
Verificar que o certificado exportado está no formato PEM, similar a:
```
-----BEGIN CERTIFICATE-----
MIIOPDCCDSSgAwIBAgIRAIL1YOXCpi0SCR8u2Lz1CqMwDQYJKoZIhvcNAQELBQAw
[... conteúdo Base64 ...]
-----END CERTIFICATE-----
```

### 14. Fechar e encerrar
Fechar o Notepad e o Microsoft Edge. Encerrar a conexão RDP.

---

## Estrutura do Certificado PEM

| Elemento | Descrição |
|----------|-----------|
| `-----BEGIN CERTIFICATE-----` | Marcador de início do certificado |
| Conteúdo Base64 | Certificado X.509 codificado em Base64 |
| `-----END CERTIFICATE-----` | Marcador de fim do certificado |

> O conteúdo em Base64 é a representação binária do certificado DER codificada em texto. Pode ser decodificado com `openssl x509 -in certificado.crt -text`.

---

## Aprendizado
- O cadeado no navegador indica que a conexão usa TLS com certificado válido emitido por uma CA confiável
- O formato PEM é Base64 do certificado binário DER, facilitando o transporte em texto
- A cadeia de certificação mostra os intermediários entre o site e a Root CA pública
- Exportar e visualizar certificados é uma habilidade fundamental para análise de segurança web
