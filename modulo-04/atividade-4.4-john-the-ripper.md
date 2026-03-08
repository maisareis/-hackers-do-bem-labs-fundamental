# Atividade 4.4 – Ataque de Dicionário Off-line contra credenciais no Kali Linux

## Objetivo

Utilizar o **John the Ripper** para realizar um ataque de dicionário off-line contra o hash de senha de um usuário Linux, demonstrando a vulnerabilidade de senhas fracas armazenadas no `/etc/shadow`.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**. A realização de ataques de quebra de senha em sistemas sem autorização é crime conforme a legislação vigente.

## Conceito

O **John the Ripper** é uma ferramenta de auditoria de senhas que suporta múltiplos algoritmos de hash e modos de ataque. Em um **ataque de dicionário**, o John testa palavras de uma lista predefinida contra o hash capturado, sendo muito eficaz contra senhas comuns ou simples.

### Arquivo `/etc/shadow`

O `/etc/shadow` armazena os hashes de senhas dos usuários. Seu acesso é restrito ao root. Cada linha segue o formato:

```
usuário : hash_da_senha : último_change : min : max : warn : inactive : expire
```

O hash possui o formato `$id$salt$hash`, onde `$id` identifica o algoritmo:

| ID   | Algoritmo      |
|------|----------------|
| `$1` | MD5            |
| `$5` | SHA-256        |
| `$6` | SHA-512        |
| `$y` | yescrypt       |

### Modos de ataque do John the Ripper

| Modo        | Descrição                                                           |
|-------------|---------------------------------------------------------------------|
| `single`    | Usa variações do nome do usuário como candidatos                    |
| `wordlist`  | Testa palavras de uma lista (dicionário)                            |
| `incremental`| Força bruta — testa todas as combinações possíveis               |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Visualizar os hashes de senha dos usuários:
```bash
cat /etc/shadow
```
Apenas usuários com hash real (começando com `$`) possuem senhas definidas. Contas de serviço exibem `!` ou `*`, indicando login desabilitado.

**4.** Criar um usuário de teste com senha fraca:
```bash
useradd -p $(openssl passwd -1 abcd123) teste1
```
O `openssl passwd -1` gera um hash MD5 (`$1$`) da senha fornecida.

**5.** Verificar que o usuário `teste1` foi criado com o hash MD5 no final do arquivo:
```bash
cat /etc/shadow
```

**6.** Copiar a linha completa do usuário `teste1` do `/etc/shadow`.

**7.** Criar um arquivo com o hash capturado:
```bash
cd /home/aluno/Documentos
nano credencial.txt
```
Colar a linha copiada, salvar com `Ctrl+X` → `S` → `Enter`.

**8.** *(Print solicitado)* Executar o ataque de dicionário com John the Ripper:
```bash
john -format=crypt credencial.txt
```
O John testa primeiro variações do nome do usuário (modo `single`) e depois a wordlist em `/usr/share/john/password.lst`. A senha `abcd123` é encontrada em menos de 1 minuto, confirmando: `abcd123 (teste1)`.

**9.** Remover o arquivo criado e fechar o Terminal:
```bash
rm /home/aluno/Documentos/credencial.txt
```

## Aprendizado

- Senhas simples como `abcd123` são encontradas em segundos por ataques de dicionário, pois constam nas wordlists padrão de ferramentas como o John.
- O hash MD5 (`$1$`) é considerado inseguro para senhas — sistemas modernos usam yescrypt (`$y$`) ou bcrypt (`$2b$`), que são computacionalmente mais custosos de quebrar.
- A flag `--show` pode ser usada após o ataque para exibir todas as senhas quebradas: `john --show credencial.txt`
- Em auditorias de segurança reais, o John é usado para identificar usuários com senhas fracas em hashes obtidos de dumps do `/etc/shadow`, orientando políticas de troca obrigatória de senha.
- A posse do arquivo `/etc/shadow` — mesmo criptografado — já representa um risco, reforçando a importância de proteger o acesso root e monitorar exfiltração de arquivos sensíveis.
