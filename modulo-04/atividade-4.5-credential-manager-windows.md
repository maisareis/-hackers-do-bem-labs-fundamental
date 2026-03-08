# Atividade 4.5 – Gerenciando credenciais no Windows Server 2022

## Objetivo

Explorar o **Credential Manager** do Windows Server 2022 para visualizar, gerenciar e realizar backup das credenciais armazenadas no sistema.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **Credential Manager** (Gerenciador de Credenciais) é uma funcionalidade nativa do Windows que armazena credenciais de autenticação usadas pelo sistema e aplicações, permitindo login automático em recursos de rede, sites e serviços. As credenciais ficam protegidas pela API DPAPI (Data Protection API), vinculada à conta do usuário Windows.

### Tipos de credenciais no Windows

| Tipo                          | Descrição                                                              |
|-------------------------------|------------------------------------------------------------------------|
| **Web Credentials**           | Senhas de sites salvas pelo Internet Explorer/Edge                     |
| **Windows Credentials**       | Credenciais para recursos de rede, compartilhamentos e serviços       |
| **Certificate-based**         | Credenciais baseadas em certificados digitais                          |
| **Generic Credentials**       | Credenciais genéricas usadas por aplicações                            |

### Formato do arquivo de backup

O backup das credenciais é salvo no formato `.crd` (Credential Backup File), protegido por senha definida durante o processo. Este arquivo pode ser importado em outro sistema Windows para restaurar as credenciais.

## Passo a Passo

**1.** Conectar ao Windows Server 2022 via RDP com as credenciais do ambiente de laboratório.

**2.** Na barra de tarefas, clicar em **"Type here to search"** e digitar:
```
Credential Manager
```

**3.** Abrir a aplicação **"Credential Manager"**.

**4.** Clicar em **"Web Credentials"** e verificar as senhas de sites armazenadas no campo **"Web Passwords"**.

**5.** Clicar em **"Windows Credentials"** e explorar as seções: **Windows Credentials**, **Certificate-based Credentials** e **Generic Credentials**.

**6.** Clicar em **"Back up Credentials"** para iniciar o backup das credenciais.

**7.** Clicar em **"Browse..."** e selecionar a pasta **Desktop**.

**8.** Inserir o nome `backup` no campo **"File Name"** → **Save**.

**9.** Clicar em **"Next"**. Acionar `Ctrl+Alt+Delete` pelo menu de ajustes da VM para prosseguir com o backup.

**10.** Inserir a senha de proteção do backup nos dois campos de **"Password"** → **Next**. A mensagem **"The backup was successful"** confirmará o sucesso.

**11.** *(Print solicitado)* Clicar em **"Finish"** e verificar que o arquivo `backup.crd` foi criado na área de trabalho (Desktop) do Windows Server 2022.

**12.** Selecionar e deletar o arquivo `backup.crd` com `Shift+Delete`. Fechar as janelas abertas e encerrar a conexão RDP.

## Aprendizado

- O Credential Manager é um alvo valioso em testes de penetração: credenciais armazenadas podem incluir senhas de domínio, compartilhamentos de rede e serviços internos.
- O arquivo de backup `.crd` é protegido por senha, mas sua existência em local acessível representa um risco — por isso deve ser armazenado de forma segura ou excluído após uso.
- Em cenários de resposta a incidentes, a análise do Credential Manager pode revelar quais recursos foram acessados pela conta comprometida.
- A DPAPI vincula a proteção das credenciais à conta do usuário Windows — em ataques de Pass-the-Hash ou Golden Ticket, a DPAPI pode ser contornada para extrair credenciais armazenadas.
- Boas práticas recomendam não armazenar credenciais críticas no Credential Manager de estações de trabalho não gerenciadas.
