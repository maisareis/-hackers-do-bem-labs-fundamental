# Atividade 5.2 – Criando o Domain Controller do Active Directory no Windows Server 2022

## Objetivo

Promover o Windows Server 2022 a **Domain Controller (DC)** do domínio `aluno.hacker.com` e configurar as zonas DNS direta e reversa.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **Domain Controller (DC)** é o servidor que hospeda o Active Directory Domain Services e autentica usuários e computadores no domínio. A promoção a DC cria a floresta e o domínio raiz. O **DNS** é configurado em paralelo para suportar a resolução de nomes do domínio, incluindo zonas de pesquisa direta (nome → IP) e reversa (IP → nome).

### Zonas DNS configuradas

| Zona                          | Tipo     | Função                                           |
|-------------------------------|----------|--------------------------------------------------|
| `aluno.hacker.com`            | Direta   | Resolve nomes do domínio para IPs                |
| `98.168.192.in-addr.arpa`     | Reversa  | Resolve IPs para nomes (PTR records)             |

### Conceitos da promoção a DC

| Opção                  | Descrição                                                         |
|------------------------|-------------------------------------------------------------------|
| Add a new forest       | Cria uma nova floresta AD — usado em ambientes novos              |
| Root domain name       | Nome FQDN do domínio raiz da floresta                             |
| NetBIOS domain name    | Nome curto do domínio (máx. 15 caracteres) — usado em compatibilidade|
| DSRM password          | Senha de recuperação do Directory Services Restore Mode           |

## Passo a Passo

**1.** No Server Manager, clicar no ícone de bandeira com exclamação amarela → **"Promote this server to a domain controller"**.

**2.** Selecionar **"Add a new forest"**, inserir `aluno.hacker.com` como "Root domain name" → **Next**.

**3.** Inserir e confirmar a senha DSRM → **Next** 2 vezes.

**4.** Confirmar que o campo "NetBIOS domain name" está preenchido com `ALUNO` → **Next** 3 vezes.

**5.** Em **"Prerequisites Check"**, verificar o ícone verde com a mensagem "All prerequisite checks passed successfully" → **Install**. O servidor será reiniciado automaticamente.

**6.** Aguardar 3 minutos e reconectar via RDP ao IP `192.168.98.20` com usuário `Administrator`. Abrir o **Server Manager** e aguardar as opções **AD DS**, **DNS** e **File and Storage Services** aparecerem na coluna esquerda.

**7.** Clicar em **"Tools"** → **"DNS"**. Expandir `DC01` → **Forward Lookup Zones** → `aluno.hacker.com` para visualizar as configurações DNS.

**8.** Clicar com o botão direito em **"Reverse Lookup Zones"** → **New Zone...** → **Next** 4 vezes → inserir `192.168.98` em "Network ID" → **Next** 2 vezes → **Finish**.

**9.** *(Print solicitado)* Verificar que a pasta "Reverse Lookup Zones" agora contém a zona `98.168.192.in-addr.arpa` recém-criada.

**10.** Acessar **Reverse Lookup Zones** → `98.168.192.in-addr.arpa` → **Start of Authority (SOA)** → aba **Name Servers** → **Edit...** → **Resolve** — verificar o ícone verde confirmando resolução DNS → **OK** 2 vezes.

**11.** Em **Forward Lookup Zones** → `aluno.hacker.com` → `dc01` (coluna direita): marcar **"Update associated pointer (PTR) record"** → **OK**.

**12.** Em **Reverse Lookup Zones** → `98.168.192.in-addr.arpa` → clicar no ícone **Refresh** (verde) e confirmar o registro PTR do servidor. Fechar o DNS Manager.

## Aprendizado

- A promoção a DC cria a floresta AD, o domínio raiz e integra o DNS — todos necessários para o funcionamento do ambiente de diretório.
- A zona de pesquisa reversa (`in-addr.arpa`) é necessária para resolução inversa de IP para nome — usada em logs, autenticação Kerberos e diagnósticos de rede.
- O registro PTR associa o IP do servidor ao seu FQDN, garantindo consistência entre as zonas direta e reversa.
- A senha DSRM é usada para recuperação do AD em modo de restauração — deve ser armazenada com segurança, pois permite acesso administrativo mesmo sem o domínio funcionando.
