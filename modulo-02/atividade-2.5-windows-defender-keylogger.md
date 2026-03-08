# Atividade 2.5 – Windows Defender Detectando Keylogger

## Objetivo
Verificar como o Windows Defender SmartScreen detecta e bloqueia o download de um keylogger no Windows Server 2022.

## Conceito

### Windows Defender SmartScreen
O **SmartScreen** é um recurso de segurança da Microsoft que analisa arquivos e URLs em tempo real, comparando com uma base de dados de ameaças conhecidas. Bloqueia downloads e execuções de arquivos considerados inseguros.

### StupidKeylogger
O **StupidKeylogger** é um keylogger open-source para Windows disponível no GitHub, utilizado nesta atividade como exemplo educacional de detecção por antivírus.

---

## Ambiente
- **Sistema:** Windows Server 2022
- **IP:** [CONFIDENCIAL]
- **Usuário:** [CONFIDENCIAL]

---

## Passo a Passo

### 1. Acessar o repositório
```
https://github.com/MinhasKamal/StupidKeylogger
```
Ler a descrição do projeto — contém um keylogger para espionar computadores Windows.

### 2. Tentar baixar o arquivo
```
https://github.com/MinhasKamal/StupidKeylogger/archive/application.zip
```

### 3. Primeira barreira — SmartScreen bloqueia o download
```
StupidKeylogger-application.zip was blocked as unsafe by Microsoft Defender SmartScreen.
```

### 4. Tentar forçar o download
- Passar o mouse na mensagem → clicar nos **3 pontos** → **Keep**

### 5. Segunda barreira — SmartScreen insiste
```
This app is unsafe
```
- Clicar na seta ao lado de **Delete** → **Keep anyway**

### 6. Arquivo baixado com aviso
O arquivo é baixado mas marcado como potencialmente perigoso.

### 7. Explorar os arquivos
Abrir o Explorador de Arquivos → pasta **Downloads** → abrir `StupidKeylogger-application.zip` e explorar os arquivos internos.

---

## Camadas de Proteção do Windows Defender

| Camada | Ação |
|--------|------|
| **SmartScreen (download)** | Bloqueia o download do arquivo |
| **SmartScreen (execução)** | Bloqueia a execução de arquivos não reconhecidos |
| **Windows Defender AV** | Detecta malware em tempo real |
| **Controlled Folder Access** | Protege pastas sensíveis |

---

## Aprendizado
- O Windows Defender SmartScreen cria múltiplas barreiras para downloads maliciosos
- É possível contornar a proteção deliberadamente, mas exige ações conscientes do usuário
- Engenharia social pode convencer usuários a ignorar os avisos de segurança
- Políticas corporativas podem impedir o bypass do SmartScreen por usuários comuns
- Educação dos usuários é essencial para que não ignorem avisos de segurança
