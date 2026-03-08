# Atividade 5.5 – Configurando política de senhas no GPO do Windows Server 2022

## Objetivo

Criar e aplicar uma **Group Policy Object (GPO)** de política de senhas no domínio `aluno.hacker.com`, definindo requisitos de tamanho, complexidade, histórico e validade.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

As **políticas de senha** no Active Directory definem os requisitos mínimos de segurança para as senhas de todos os usuários do domínio. Quando vinculadas ao domínio via GPO, aplicam-se a todos os computadores membros.

### Parâmetros de política de senha configurados

| Parâmetro                            | Valor definido | Significado                                                    |
|--------------------------------------|----------------|----------------------------------------------------------------|
| Minimum password age                 | 120 dias       | Senha deve ser usada por no mínimo 120 dias antes de ser trocada|
| Minimum password length              | 9 caracteres   | Tamanho mínimo obrigatório da senha                            |
| Password must meet complexity requirements | Enabled  | Exige letras maiúsculas, minúsculas, números e símbolos        |
| Enforce password history             | 3 senhas       | Impede reutilização das últimas 3 senhas                       |

### Requisitos de complexidade do Windows

Quando "Password must meet complexity requirements" está habilitado, a senha deve:
- Ter no mínimo 6 caracteres
- Não conter o nome do usuário ou partes do nome completo
- Conter caracteres de pelo menos 3 das 4 categorias: maiúsculas, minúsculas, dígitos, símbolos

## Passo a Passo

**1.** Conectar ao Windows Server 2022 (servidor) via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o **Server Manager** → **"Tools"** → **"Group Policy Management"**.

**3.** Expandir `Forest: aluno.hacker.com` → `Domains` → `aluno.hacker.com`. Botão direito em **"Group Policy Objects"** → **"New"**.

**4.** Inserir o nome `Politica de senhas` → **OK**.

**5.** Expandir **"Group Policy Objects"** → botão direito em `Politica de senhas` → **"Edit"**.

**6.** No **Group Policy Management Editor**, expandir: `Computer Configuration` → `Policies` → `Windows Settings` → `Security Settings` → `Account Policies` → clicar em **"Password Policy"**.

**7.** Configurar **"Minimum password age"**: 2 cliques → ativar "Define this policy" → inserir `120` → **Apply** → **OK** 2 vezes.

**8.** Configurar **"Minimum password length"**: 2 cliques → ativar "Define this policy" → inserir `9` → **Apply** → **OK**.

**9.** Configurar **"Password must meet complexity requirements"**: 2 cliques → ativar "Define this policy" → selecionar **"Enabled"** → **Apply** → **OK**.

**10.** Configurar **"Enforce password history"**: 2 cliques → ativar "Define this policy" → inserir `3` → **Apply** → **OK**.

**11.** Voltar ao **Group Policy Management** → botão direito em `aluno.hacker.com` → **"Link an Existing GPO"** → selecionar `Politica de senhas` → **OK**.

**12.** *(Print solicitado)* Abrir o **Command Prompt** e executar:
```cmd
gpupdate /force
```
Confirmar `Computer Policy update has completed successfully` e `User Policy update has completed successfully`.

**13.** Fechar as conexões com o Windows Server 2022.

## Aprendizado

- Políticas de senha definidas via GPO e vinculadas ao domínio têm precedência sobre configurações locais dos computadores membros, garantindo padronização em toda a organização.
- O "Minimum password age" impede que usuários burlem o histórico de senhas trocando rapidamente várias vezes até poder reutilizar a senha original.
- O histórico de senhas (`Enforce password history`) trabalha em conjunto com o tempo mínimo: sem o tempo mínimo, o usuário poderia trocar a senha N vezes em sequência para "zerar" o histórico.
- Em ambientes de alta segurança, recomenda-se complementar as políticas de senha com **Fine-Grained Password Policies (FGPP)** para aplicar requisitos diferentes a grupos específicos (ex.: administradores com política mais restritiva).
- O `gpupdate /force` deve ser executado em todos os computadores do domínio para garantir a aplicação imediata — em ambientes grandes, isso é feito via script ou reinicialização programada.
