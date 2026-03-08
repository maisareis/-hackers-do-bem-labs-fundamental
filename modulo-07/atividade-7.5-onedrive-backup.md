# Atividade 7.5 – Backup via Replicação Síncrona no OneDrive

## Objetivo
Configurar o OneDrive no Windows Server 2022 para realizar backup automático de pastas via replicação síncrona em nuvem.

## Conceito

### Replicação Síncrona em Nuvem
A **replicação síncrona** sincroniza os arquivos em tempo real entre o dispositivo local e a nuvem. Qualquer alteração feita localmente é imediatamente refletida na nuvem.

### OneDrive
O **OneDrive** é o serviço de armazenamento em nuvem da Microsoft, integrado ao Windows e ao ecossistema Microsoft 365.

---

## Pré-requisitos
- Windows Server 2022
- Conta Microsoft (Outlook/Hotmail)
- Arquivo de instalação do OneDrive

---

## Passo a Passo

### 1. Instalar o OneDrive
Executar o arquivo `OneDriveSetup` disponível na pasta do curso.

### 2. Configurar sites confiáveis
- Abrir **Internet Options**
- Aba **Security** → **Trusted Sites**
- Adicionar: `https://odc.officeapps.live.com`

### 3. Autenticar no OneDrive
- Abrir o aplicativo OneDrive
- Fazer login com as credenciais Microsoft

### 4. Configurar pasta de backup
- Na janela "Back up folders on this PC"
- Selecionar a pasta **Desktop**
- Clicar em "Start backup"

### 5. Verificar sincronização
- Clicar em "Open my OneDrive folder"
- O File Explorer exibirá a pasta sincronizada com ícones de status

---

## Status de Sincronização

| Ícone | Significado |
|-------|-------------|
| ✅ Verde | Arquivo sincronizado |
| 🔄 Azul | Sincronização em andamento |
| ❌ Vermelho | Erro na sincronização |
| ☁️ Nuvem | Disponível apenas na nuvem |

---

## Aprendizado
- O OneDrive realiza backup automático e contínuo das pastas selecionadas
- A replicação síncrona garante que os dados estejam sempre atualizados na nuvem
- É uma solução simples e eficaz para backup de dados importantes
- Recomenda-se usar uma conta separada para ambientes de teste
