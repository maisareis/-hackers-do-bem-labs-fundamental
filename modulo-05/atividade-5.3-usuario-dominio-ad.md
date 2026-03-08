# Atividade 5.3 – Inserindo um usuário no Domínio do Active Directory no Windows Server 2022

## Objetivo

Criar um usuário no Active Directory, configurar sua associação ao grupo **Remote Desktop Users**, criar uma **Organizational Unit (OU)** com GPO e aplicar política de acesso via Remote Desktop Services.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

### Elementos do Active Directory

| Elemento               | Descrição                                                              |
|------------------------|------------------------------------------------------------------------|
| User (Usuário)         | Conta de autenticação associada a uma pessoa ou serviço               |
| Organizational Unit    | Contêiner lógico para organizar objetos do AD (usuários, computadores) |
| Group Policy Object (GPO) | Conjunto de configurações aplicadas a objetos dentro de uma OU    |
| Domain Users           | Grupo padrão que contém todos os usuários do domínio                  |
| Remote Desktop Users   | Grupo que permite acesso remoto via RDP                               |

### Comando `gpupdate /force`

Força a aplicação imediata de todas as políticas de grupo pendentes no computador e no usuário logado, sem aguardar o intervalo de atualização automático (padrão: 90 minutos).

## Passo a Passo

**1.** No Server Manager, clicar em **"Tools"** → **"Active Directory Users and Computers"**. Expandir `aluno.hacker.com` → **Users** → botão direito → **New** → **User**.

**2.** Preencher os dados do usuário:
- First name: `Nome1`
- Last name: `Sobrenome1`
- User logon name: `nome1`

Clicar em **Next** → inserir senha conforme o ambiente → desmarcar "User must change password at next logon" → marcar "Password never expires" → **Next** → **Finish**.

**3.** Clicar com o botão direito no usuário `Nome1 Sobrenome1` → **Properties** → aba **"Member of"** → **Add...** → digitar `remote` → **Check Names** → selecionar **"Remote Desktop Users"** → **OK** 2 vezes → **Apply** → **OK**.

**4.** Acessar **"Tools"** → **"Group Policy Management"**. Navegar em: `Forest: aluno.hacker.com` → `Domains` → `aluno.hacker.com`.

**5.** Botão direito em `aluno.hacker.com` → **"New Organizational Unit"** → nome `Rede1` → **OK**.

**6.** Botão direito na pasta `Rede1` → **"Create a GPO in this domain, and Link it here..."** → nome `Alunos` → **OK**.

**7.** Botão direito no GPO `Alunos` → **"Edit..."**.

**8.** No **Group Policy Management Editor**, expandir: `Computer Configuration` → `Policies` → `Windows Settings` → `Security Settings` → `Local Policies` → `User Rights Assignment`. Abrir **"Allow log on through Remote Desktop Services"**.

**9.** Marcar **"Define these policy settings"** → **"Add User or Group"** → **Browse...** → **Advanced** → **Find Now** → selecionar **"Domain Users"** com 2 cliques.

**10.** Repetir: **Advanced** → **Find Now** → selecionar **"Authenticated Users"** → **OK** 2 vezes → **Apply** → **OK**.

**11.** *(Print solicitado)* Abrir o **Command Prompt** e executar:
```cmd
gpupdate /force
```
Confirmar as mensagens `Computer Policy update has completed successfully` e `User Policy update has completed successfully`.

**12.** Fechar o Command Prompt. Não fechar o Windows Server.

## Aprendizado

- Organizational Units permitem delegar administração e aplicar GPOs a subconjuntos específicos de objetos do AD, tornando o gerenciamento mais granular que a aplicação no domínio inteiro.
- A política "Allow log on through Remote Desktop Services" define quais usuários ou grupos podem estabelecer sessões RDP — controle essencial para limitar a superfície de ataque de acesso remoto.
- O `gpupdate /force` é indispensável após modificações em GPOs quando se quer validar imediatamente as mudanças, sem aguardar o ciclo de atualização automática.
- Adicionar o usuário ao grupo "Remote Desktop Users" no AD não é suficiente sozinho — a política de grupo precisa permitir explicitamente o logon via RDP para que o acesso funcione.
