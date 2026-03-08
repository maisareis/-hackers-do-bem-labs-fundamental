# Atividade 5.4 – Configurando um cliente com Windows Server 2022 para integrar o Domínio do Active Directory

## Objetivo

Ingressar o Windows Server 2022 (cliente) no domínio `aluno.hacker.com` e autenticar com um usuário de domínio via RDP.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

Para que um computador cliente faça parte de um domínio AD, dois requisitos são fundamentais:

1. **Conectividade de rede** com o controlador de domínio
2. **DNS apontando para o DC** — o cliente precisa resolver o nome do domínio para localizar o DC

Após o ingresso no domínio, o cliente passa a autenticar usuários via Kerberos contra o AD, permitindo login com credenciais de domínio em vez de contas locais.

### Diferença entre conta local e conta de domínio

| Aspecto             | Conta Local                          | Conta de Domínio                         |
|---------------------|--------------------------------------|------------------------------------------|
| Armazenamento       | SAM local do computador              | Active Directory (no DC)                 |
| Escopo              | Apenas no computador local           | Qualquer computador do domínio           |
| Formato de login    | `NomeUsuario`                        | `DOMINIO\NomeUsuario`                    |
| Gerenciamento       | Administrador local                  | Administrador do domínio                 |

## Passo a Passo

**1.** No Windows Server 2022 (servidor), abrir o **Command Prompt** e verificar o IP:
```cmd
ipconfig
```
Anotar o IP `192.168.98.20` — será o DNS do cliente.

**2.** Minimizar a sessão RDP do servidor. Conectar via RDP ao Windows Server 2022 (cliente) no IP `192.168.98.30`.

**3.** Abrir o **Command Prompt** e testar conectividade com o servidor:
```cmd
ping 192.168.98.20
```
Confirmar que todos os 4 pacotes são respondidos (0% loss).

**4.** Acessar **Control Panel** → **Network and Internet** → **Network and Sharing Center** → **Ethernet X** → **Properties** → 2 cliques em **"Internet Protocol Version 4 (TCP/IPv4)"** → selecionar **"Use the following DNS server addresses"** → inserir `192.168.98.20` → **OK** 2 vezes → **Close**.

**5.** No Server Manager, instalar a função **Remote Assistance**: **Manage** → **Add Roles and Features** → **Next** 4 vezes → ativar **"Remote Assistance"** → **Next** → **Install** → **Close**.

**6.** Botão direito no ícone Windows → **System** → **"Advanced system settings"** → aba **"Computer Name"** → **Change** → selecionar **"Domain"** → inserir `aluno.hacker.com` → **OK** → inserir usuário `nome1` e senha conforme o ambiente → **OK**. Aceitar reinicialização.

**7.** Aguardar 3 minutos e reconectar via RDP ao cliente (`192.168.98.30`). Acessar **System** → **"Advanced system settings"** → aba **"Remote"** → marcar **"Allow connections only from..."** → **Select Users...** → **Add...** → **Advanced...** → inserir credenciais `nome1` → **Find Now** → selecionar **"Nome1 Sobrenome1"** com 2 cliques → **OK**. Confirmar que `ALUNO\nome1` aparece na lista → **OK** 3 vezes. Fazer **Sign out**.

**8.** *(Print solicitado)* Criar nova conexão RDP ao IP `192.168.98.30` com usuário `nome1` e senha conforme o ambiente. Confirmar autenticação bem-sucedida no domínio `aluno.hacker.com`. Fechar a sessão RDP.

## Aprendizado

- Apontar o DNS do cliente para o IP do DC é o passo mais crítico no ingresso ao domínio: sem resolução DNS do nome do domínio, a operação de junção falha mesmo com conectividade de rede.
- O ingresso ao domínio registra o computador como objeto no AD, permitindo aplicação de GPOs e gerenciamento centralizado.
- Após o ingresso, o login local passa a ser `ALUNO\nome1` (domínio\usuário), diferente de uma conta local que seria apenas `nome1`.
- A instalação do **Remote Assistance** no cliente é necessária para suporte a conexões remotas no contexto do ambiente de laboratório.
