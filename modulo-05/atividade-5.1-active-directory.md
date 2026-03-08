# Atividade 5.1 – Criando o Active Directory no Windows Server 2022

## Objetivo

Instalar e configurar o **Active Directory Domain Services (AD DS)** e o **DNS Server** no Windows Server 2022, preparando o servidor como controlador de domínio.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **Active Directory (AD)** é o serviço de diretório da Microsoft usado para gerenciamento centralizado de usuários, computadores, grupos e políticas em ambientes Windows. O **AD DS** (Active Directory Domain Services) é a função de servidor que implementa esse serviço, e o **DNS Server** é um requisito obrigatório para o funcionamento do AD, pois os clientes usam DNS para localizar o controlador de domínio.

### Funções instaladas nesta atividade

| Função                                      | Descrição                                                    |
|---------------------------------------------|--------------------------------------------------------------|
| Active Directory Domain Services (AD DS)    | Serviço de diretório para gerenciamento de domínio           |
| DNS Server                                  | Resolução de nomes para localização do controlador de domínio|
| Group Policy Management                     | Gerenciamento de políticas de grupo (instalado automaticamente)|
| Remote Server Administration Tools (RSAT)   | Ferramentas de administração remota (instaladas automaticamente)|

## Passo a Passo

**1.** Conectar ao Windows Server 2022 (servidor) via RDP com as credenciais do ambiente de laboratório (`192.168.98.20`).

**2.** Abrir o **Server Manager**, fechar o aviso inicial e acessar **"Local Server"** para visualizar as configurações atuais.

**3.** Renomear o servidor para `DC01`:
- Clicar no nome atual ao lado de "Computer name" → **Change...** → inserir `DC01` → **OK** → aceitar reinicialização → **Restart Now**.

**4.** Aguardar 3 minutos e reconectar via RDP. Confirmar que o "Computer name" exibe `DC01`.

**5.** No Server Manager, clicar em **"Manage"** → **"Add Roles and Features"**.

**6.** Clicar em **"Next"** 3 vezes até chegar em **"Server Roles"**. Ativar **"Active Directory Domain Services"** → **"Add Features"**.

**7.** Ativar **"DNS Server"** → **"Add Features"** → **"Continue"** → **"Next"**.

**8.** Em **"Features"**, verificar que **"Group Policy Management"** e **"Remote Server Administration Tools"** já estão selecionados → **"Next"** 4 vezes até **"Confirmation"**.

**9.** *(Print solicitado)* No campo **"Confirmation"**, clicar em **"Install"** e aguardar a conclusão da instalação (barra de progresso).

**10.** Clicar em **"Close"** ao concluir. Não fechar o Server Manager.

## Aprendizado

- O AD DS transforma um servidor Windows comum em um **controlador de domínio (DC)**, ponto central de autenticação e autorização de toda a rede.
- O DNS é um pré-requisito técnico do AD: os clientes precisam resolver o nome do domínio (ex.: `aluno.hacker.com`) para localizar o DC. Por isso, o DNS Server é instalado no mesmo servidor.
- O renomeio do servidor para `DC01` é uma boa prática de nomenclatura — indica claramente a função do servidor na infraestrutura.
- As ferramentas RSAT instaladas automaticamente permitem administrar o AD, DNS e políticas de grupo diretamente no servidor ou remotamente.
