# Atividade 5.9 – Implementando o Acesso Baseado em Função (RBAC) no Kali Linux

## Objetivo

Implementar o **Controle de Acesso Baseado em Função (RBAC)** no Kali Linux criando um grupo com função específica, atribuindo um usuário a ele e definindo permissões de acesso ao arquivo `texto.txt`.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **RBAC (Role-Based Access Control)** é um modelo de controle de acesso no qual as permissões são atribuídas a **funções (roles/grupos)**, e os usuários recebem acesso por pertencerem a essas funções — e não por permissões individuais. No Linux, esse modelo é implementado por meio de **grupos**.

### Comparação DAC vs. RBAC

| Aspecto             | DAC                                      | RBAC                                               |
|---------------------|------------------------------------------|----------------------------------------------------|
| Base de permissão   | Identidade do usuário (proprietário)     | Função/grupo ao qual o usuário pertence            |
| Gerenciamento       | Individual por arquivo                   | Centralizado por grupo/função                      |
| Escalabilidade      | Difícil em ambientes grandes             | Mais escalável — mudar o grupo afeta todos os membros|
| Exemplo Linux       | `chmod u+rwx arquivo`                    | `chown :grupo arquivo` + `chmod 770 arquivo`       |

### Comandos utilizados

| Comando                              | Função                                                         |
|--------------------------------------|----------------------------------------------------------------|
| `getent passwd \| cut -d: -f1`       | Lista todos os usuários do sistema                             |
| `getent group \| cut -d: -f1`        | Lista todos os grupos do sistema                               |
| `groupadd contabilidade`             | Cria o grupo `contabilidade`                                   |
| `usermod -aG contabilidade teste1`   | Adiciona o usuário `teste1` ao grupo `contabilidade`           |
| `chown :contabilidade texto.txt`     | Define `contabilidade` como grupo proprietário do arquivo      |
| `chmod 770 texto.txt`                | Permissão total para proprietário e grupo; nenhuma para outros |

### Permissões octal 770

| Escopo       | Octal | Permissões |
|--------------|-------|------------|
| Proprietário | 7     | rwx        |
| Grupo        | 7     | rwx        |
| Outros       | 0     | ---        |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP ao IP `192.168.98.40` com usuário `aluno` e senha `rnpesr`.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Navegar até a pasta Documentos e listar os arquivos:
```bash
cd /home/aluno/Documentos/
ls
```
Confirmar que o arquivo `texto.txt` (criado na atividade 5.8) está presente.

**4.** Listar todos os usuários do sistema:
```bash
getent passwd | cut -d: -f1
```
Observar que o usuário `teste1` aparece na lista.

**5.** Listar todos os grupos do sistema:
```bash
getent group | cut -d: -f1
```
Observar que o grupo `teste1` aparece no final da lista.

**6.** Criar o grupo `contabilidade` para representar a função:
```bash
groupadd contabilidade
```

**7.** Repetir o comando do passo 5 e confirmar que o grupo `contabilidade` aparece no final da lista.

**8.** Adicionar o usuário `teste1` ao grupo `contabilidade`:
```bash
usermod -aG contabilidade teste1
```

**9.** *(Print solicitado)* Atribuir a propriedade de grupo do arquivo ao `contabilidade` e definir as permissões RBAC:
```bash
chown :contabilidade texto.txt
chmod 770 texto.txt
ls -l
```
Saída esperada:
```
-rwxrwx--- 1 root contabilidade 7 [data] texto.txt
```
O arquivo agora é acessível com permissão total apenas para o proprietário (`root`) e para membros do grupo `contabilidade`. Outros usuários não têm nenhum acesso.

**10.** Apagar o arquivo e fechar o Terminal:
```bash
rm texto.txt
```

## Aprendizado

- O RBAC separa a gestão de permissões da identidade individual: ao adicionar ou remover um usuário do grupo `contabilidade`, suas permissões sobre todos os arquivos do grupo são automaticamente ajustadas.
- O `chmod 770` garante que apenas membros do grupo `contabilidade` (e o proprietário) acessem o arquivo — outros usuários ficam completamente bloqueados (`---`).
- O `chown :grupo arquivo` altera apenas o grupo proprietário sem mudar o usuário proprietário — útil para transferir controle de grupo mantendo o proprietário original.
- Em ambientes corporativos, o RBAC é a base de sistemas como **LDAP**, **Active Directory** e **IAM na nuvem** — onde usuários recebem permissões por pertencerem a grupos que representam funções (ex.: `admins`, `devops`, `financeiro`).
