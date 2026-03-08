# Atividade 1.8 – Configurando o Maltego

## Objetivo
Instalar, configurar e explorar a ferramenta Maltego para reconhecimento OSINT no Kali Linux.

## Conceito
O **Maltego** é uma plataforma de inteligência e análise forense que permite visualizar graficamente relações entre entidades como domínios, IPs, pessoas, e-mails e organizações. É amplamente utilizado em:

- Reconhecimento OSINT (Open Source Intelligence)
- Investigações de segurança cibernética
- Mapeamento de infraestrutura de alvos
- Forense digital

---

## Pré-requisito
Criar conta gratuita em: https://www.maltego.com/maltego-id-registration/

---

## Passo a Passo

### 1. Abrir o Maltego
```bash
maltego
```

### 2. Selecionar o tipo de licença
- Na janela "Welcome to Maltego", selecionar **MALTEGO ID**
- Clicar em **Next**

### 3. Ativar online
- Selecionar **Online Activation (Default)**
- Clicar em **Next**

### 4. Aceitar os termos
- Selecionar **Accept**
- Clicar em **Next**

### 5. Fazer login
- Clicar em **Browser Login**
- Inserir e-mail e senha criados no pré-requisito
- Clicar em **SIGN IN TO MALTEGO**
- Mensagem "Authentication Complete" aparece no Firefox

### 6. Concluir configuração
- Clicar **Next** 4 vezes até o passo "7. Data Sources T&Cs"
- Marcar a caixa **"By checking the box, I declare that..."**
- Clicar **Next** 5 vezes até o passo "12. Ready"
- Clicar **Finish**

### 7. Fechar avisos iniciais
- Clicar em **"Don't show this again"** e **OK** no aviso de memória
- Na mensagem "Maltego Product Tour": **"Don't ask again"** → **"Not now"** → **Close**

### 8. Explorar o Data Hub
- Acessar aba **Transforms → Maltego Data Hub**
- Visualizar as aplicações OSINT disponíveis

---

## Interface do Maltego

| Área | Descrição |
|------|-----------|
| **Entity Palette** | Entidades disponíveis para investigação (domínios, IPs, pessoas, etc.) |
| **Transforms** | Consultas automáticas a fontes de dados externas |
| **Maltego Data Hub** | Repositório de integrações e fontes OSINT |
| **Graph Panel** | Visualização gráfica das relações entre entidades |

---

## Aprendizado
- Maltego centraliza diversas fontes OSINT em uma única interface
- O Data Hub oferece dezenas de integrações com serviços externos
- A versão gratuita (Community) já permite realizar reconhecimento básico
- Ferramenta essencial para investigações de segurança e forense digital
