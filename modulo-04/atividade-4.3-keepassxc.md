# Atividade 4.3 – Criando um cofre de senhas com o KeePassXC no Kali Linux

## Objetivo

Utilizar o **KeePassXC** para criar e gerenciar um cofre de senhas criptografado, armazenando credenciais de forma segura com algoritmo AES-256.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **KeePassXC** é um gerenciador de senhas de código aberto, multiplataforma e offline. Ele armazena credenciais em um banco de dados criptografado (arquivo `.kdbx`) protegido por uma senha mestra. Sua principal vantagem em relação a gerenciadores baseados em nuvem é que os dados nunca saem do dispositivo do usuário.

### Características do KeePassXC

| Recurso                  | Descrição                                                          |
|--------------------------|--------------------------------------------------------------------|
| Criptografia             | AES-256 bits (padrão mais seguro disponível atualmente)            |
| Formato do banco         | `.kdbx` — compatível com KeePass em outras plataformas             |
| Geração de senhas        | Cria senhas complexas e únicas para cada conta                     |
| Campos por entrada       | Título, usuário, senha, URL, notas e campos personalizados         |
| Categorização            | Organização por grupos e tags                                      |
| Pesquisa                 | Busca avançada entre todas as entradas                             |

### Por que usar um gerenciador de senhas?

- Permite usar senhas únicas e complexas para cada serviço sem precisar memorizá-las
- Elimina a reutilização de senhas — uma das principais causas de comprometimento de contas
- O banco `.kdbx` é inutilizável sem a senha mestra, mesmo se obtido por um atacante

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal com o usuário `aluno` (**não como root**) e iniciar o KeePassXC:
```bash
keepassxc
```

**3.** Na interface gráfica, clicar em **"+ Criar Banco de Dados"**.

**4.** Inserir o nome `Pessoal` no campo "Nome do banco de dados" → **Continuar**.

**5.** Na aba **"Definições de cifra"**, clicar em **"Avançado"** e verificar que o algoritmo selecionado é **AES 256-bit** → **Continuar**.

**6.** Inserir e confirmar a senha mestra nos campos indicados → **Concluído**. Aceitar o aviso de senha fraca clicando em **"Continuar com senha fraca"**.

**7.** Na janela de salvar arquivo, acessar a pasta **Documentos** e clicar em **Salvar** (arquivo `Senhas.kdbx`).

**8.** Na interface principal do KeePassXC, adicionar uma nova entrada clicando no botão **"+"** e preencher:

| Campo             | Valor                      |
|-------------------|----------------------------|
| Título            | Netflix                    |
| Nome de usuário   | `[CONFIDENCIAL]`           |
| Senha             | `[CONFIDENCIAL]`           |
| URL               | https://www.netflix.com    |

Clicar em **"OK"**.

**9.** *(Print solicitado)* Verificar que a entrada "Netflix" foi armazenada na interface do KeePassXC com título, usuário e URL visíveis.

**10.** Selecionar a entrada "Netflix" e clicar no botão da lixeira para removê-la. Fechar o KeePassXC.

**11.** Remover o arquivo do banco de dados criado:
```bash
cd /home/aluno/Documentos
rm Senhas.kdbx
```

## Aprendizado

- O KeePassXC nunca transmite dados para servidores externos — o banco `.kdbx` permanece completamente local, eliminando riscos de vazamento por brechas em serviços de nuvem.
- O algoritmo AES-256 é considerado praticamente inquebrável com tecnologia atual, tornando a força da senha mestra o único ponto crítico de segurança.
- Documentação oficial: [https://keepassxc.org/docs/KeePassXC_GettingStarted](https://keepassxc.org/docs/KeePassXC_GettingStarted)
- Em ambientes corporativos, gerenciadores de senhas são um controle preventivo essencial para combater o reuso de credenciais e senhas fracas.
- A remoção do arquivo ao final do laboratório é uma boa prática — evita que credenciais de teste permaneçam armazenadas em ambientes compartilhados.
