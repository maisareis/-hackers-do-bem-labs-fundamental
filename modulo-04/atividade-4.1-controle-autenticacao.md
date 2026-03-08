# Atividade 4.1 – Modificando os parâmetros de Controle de Autenticação no Kali Linux

## Objetivo

Explorar os arquivos de controle de autenticação do Linux (`/etc/passwd` e `/etc/group`) e praticar a modificação de senhas de usuários como parte do gerenciamento de controle de acesso.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O controle de autenticação no Linux é gerenciado por um conjunto de arquivos de sistema que armazenam informações sobre usuários, grupos e senhas. O acesso ao sistema depende da correspondência entre as credenciais fornecidas e os dados armazenados nesses arquivos.

### Arquivos de autenticação

| Arquivo        | Conteúdo                                                         |
|----------------|------------------------------------------------------------------|
| `/etc/passwd`  | Informações de usuários: nome, UID, GID, diretório home, shell  |
| `/etc/shadow`  | Hashes de senhas (acesso restrito ao root)                       |
| `/etc/group`   | Definição de grupos e seus membros                               |

### Estrutura do `/etc/passwd`

Cada linha possui 7 campos separados por `:`:

```
nome_usuário : x : UID : GID : comentário : diretório_home : shell
```

| Campo           | Exemplo        | Descrição                                      |
|-----------------|----------------|------------------------------------------------|
| Nome de usuário | `aluno`        | Identificador de login                         |
| Senha           | `x`            | Indica que a senha está em `/etc/shadow`       |
| UID             | `1001`         | ID único do usuário                            |
| GID             | `1002`         | ID do grupo primário                           |
| Comentário      | *(vazio)*      | Campo de informações adicionais                |
| Home            | `/home/aluno`  | Diretório inicial do usuário                   |
| Shell           | `/bin/bash`    | Shell padrão do usuário                        |

### Estrutura do `/etc/group`

Cada linha possui 4 campos separados por `:`:

```
nome_grupo : x : GID : membros
```

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e verificar o usuário e diretório atual:
```bash
whoami
pwd
```

**3.** Visualizar o conteúdo do arquivo de usuários:
```bash
cat /etc/passwd
```
Localizar a linha do usuário `aluno` e identificar seus campos: UID, GID, diretório home e shell.

**4.** Visualizar o conteúdo do arquivo de grupos:
```bash
cat /etc/group
```
Observar que o usuário `aluno` pertence ao grupo `sudo` (GID 27), permitindo execução de comandos privilegiados.

**5.** Elevar privilégios e alterar a senha do usuário `aluno`:
```bash
sudo -i
passwd aluno
```
Definir a nova senha conforme o ambiente de laboratório.

**6.** Encerrar a sessão RDP: botão direito no nome do usuário (canto superior direito) → **Painel** → **Encerrar sessão**.

**7.** Reconectar via RDP com a nova senha para confirmar o acesso bem-sucedido.

**8.** *(Print solicitado)* Repetir o passo 5 para restaurar a senha original do laboratório, confirmando o funcionamento do controle de autenticação.

**9.** Fechar o Terminal.

## Aprendizado

- O `/etc/passwd` é legível por todos os usuários do sistema, mas não contém as senhas — apenas um `x` indicando que o hash está no `/etc/shadow`, que é acessível apenas pelo root.
- O comando `passwd` permite que o root altere a senha de qualquer usuário sem precisar conhecer a senha atual, diferentemente de um usuário comum que precisa informar a senha anterior.
- O campo de shell (`/bin/bash`, `/usr/sbin/nologin`) determina se o usuário pode fazer login interativo — contas de serviço geralmente usam `/usr/sbin/nologin` para impedir acesso direto.
- A pertença ao grupo `sudo` é o que concede ao usuário `aluno` a capacidade de executar comandos como root via `sudo -i`.
