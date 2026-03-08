# Atividade 5.7 – Criando uma Organizational Unit (OU) no Windows Server 2022

## Objetivo

Criar uma **Organizational Unit (OU)** chamada `TI` no Active Directory e adicionar um usuário com **Common Name** `usuarioti` dentro dela.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

### Organizational Unit (OU)

Uma **OU** é um contêiner lógico dentro do Active Directory usado para organizar objetos (usuários, computadores, grupos) de forma hierárquica. OUs permitem:

- Delegar administração a diferentes equipes
- Aplicar GPOs a subconjuntos específicos de objetos
- Refletir a estrutura organizacional da empresa (ex.: por departamento, localidade ou função)

### Common Name (CN)

O **Common Name** é o atributo que identifica unicamente um objeto dentro de sua OU no AD. No contexto de usuários, é o **User Logon Name** — o nome usado para autenticação. Em LDAP, o caminho completo de um objeto é representado como:

```
CN=usuarioti,OU=TI,DC=aluno,DC=hacker,DC=com
```

### Diferença entre OU e Container

| Tipo       | GPO aplicável | Delegação | Criado por |
|------------|---------------|-----------|------------|
| OU         | Sim           | Sim       | Administrador |
| Container  | Não diretamente | Limitada | Sistema (padrão) |

## Passo a Passo

**1.** Conectar ao Windows Server 2022 (servidor) via RDP com as credenciais do ambiente de laboratório.

**2.** Na barra de tarefas, clicar em "Type here to search" e inserir `Active Directory Users and Computers`. Clicar em **Active Directory Users and Computers**.

**3.** Clicar com o botão direito no domínio `aluno.hacker.com` (coluna esquerda).

**4.** Selecionar **New** → **Organizational Unit**.

**5.** Inserir o nome `TI` → **OK**. Confirmar que a pasta `TI` foi criada na coluna esquerda.

**6.** Clicar com o botão direito na pasta `TI` → **New** → **User**.

**7.** Preencher os campos:
- First name: `Usuario`
- Last name: `TI`

**9.** *(Print solicitado)* No campo **"User logon name"**, inserir `usuarioti` — este será o Common Name do usuário.

**10.** Clicar em **Next** → inserir a senha `usu@rioti1` → desmarcar "User must change password at next logon" → marcar "Password never expires" → **Next** → **Finish**.

**11.** Confirmar que o novo usuário `Usuario TI` aparece dentro da OU `TI`. Fechar a conexão RDP.

## Aprendizado

- OUs refletem a estrutura organizacional e permitem aplicar políticas diferentes para cada departamento — ex.: a OU `TI` pode ter GPOs mais permissivas para instalação de software que outras OUs.
- O User Logon Name (`usuarioti`) é o **Common Name (CN)** no contexto LDAP — usado em integrações com aplicações que autenticam via LDAP/LDAPS contra o AD.
- Criar usuários diretamente na OU correta (em vez de no contêiner padrão `Users`) é uma boa prática: facilita a aplicação de GPOs e a delegação de administração por departamento.
- A hierarquia de OUs pode ser tão profunda quanto necessário — ex.: `OU=TI,OU=SP,OU=Brasil,DC=empresa,DC=com` para representar localidades.
