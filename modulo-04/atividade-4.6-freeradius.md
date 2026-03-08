# Atividade 4.6 – Implementando um servidor RADIUS no Kali Linux

## Objetivo

Implementar e testar um servidor de autenticação **FreeRADIUS** no Kali Linux, validando cenários de autenticação bem-sucedida e rejeitada via protocolo RADIUS.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **RADIUS** (Remote Authentication Dial-In User Service) é um protocolo de rede que fornece autenticação, autorização e contabilidade (AAA) de forma centralizada. Amplamente utilizado por provedores de internet, redes Wi-Fi corporativas e VPNs, o RADIUS permite que dispositivos de acesso (NAS — Network Access Servers) deleguem a autenticação de usuários a um servidor central.

O **FreeRADIUS** é a implementação open source mais utilizada do protocolo RADIUS.

### Estrutura de autenticação RADIUS

```
Cliente (radtest) → NAS → Servidor FreeRADIUS → Access-Accept / Access-Reject
```

### Arquivos de configuração do FreeRADIUS

| Arquivo/Diretório         | Função                                                      |
|---------------------------|-------------------------------------------------------------|
| `clients.conf`            | Define clientes RADIUS e seus segredos compartilhados       |
| `users`                   | Define usuários e suas senhas em texto claro ou hash        |
| `sites-enabled/default`   | Política de autorização e autenticação padrão               |
| `mods-enabled/`           | Módulos ativos (pap, eap, ldap, etc.)                       |

### Portas padrão do FreeRADIUS

| Porta | Protocolo | Função                     |
|-------|-----------|----------------------------|
| 1812  | UDP       | Autenticação e autorização |
| 1813  | UDP       | Contabilidade (accounting) |

### Resultado de uma requisição RADIUS

| Resposta         | Significado                                           |
|------------------|-------------------------------------------------------|
| `Access-Accept`  | Autenticação bem-sucedida — acesso concedido          |
| `Access-Reject`  | Autenticação falhou — usuário ou senha incorretos     |
| `Access-Challenge`| Servidor solicita informação adicional (ex.: OTP)   |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Verificar interfaces de rede (atentar para o loopback `127.0.0.1`):
```bash
ifconfig
```

**4.** Visualizar os arquivos de configuração do FreeRADIUS:
```bash
ls /etc/freeradius/3.0
```

**5.** Verificar o cliente pré-configurado e seu segredo compartilhado:
```bash
cat /etc/freeradius/3.0/clients.conf
```
O cliente `localhost` usa o segredo `testing123`.

**6.** Criar o usuário de teste no arquivo de usuários:
```bash
nano /etc/freeradius/3.0/users
```
Abaixo do bloco comentado do `bob`, adicionar:
```
usuario1 Cleartext-Password := "rnpesr"
```
Salvar com `Ctrl+X` → `S` → `Enter`.

**7.** Iniciar o FreeRADIUS em modo debug (X maiúsculo):
```bash
freeradius -X
```
O servidor ficará escutando nas portas 1812 (auth) e 1813 (acct) em todos os endereços. A mensagem `Ready to process requests` confirma que está operacional.

**8.** Abrir um **segundo terminal** e testar autenticação com credenciais válidas:
```bash
sudo -i
radtest usuario1 rnpesr 127.0.0.1 0 testing123
```
A resposta `Received Access-Accept` confirma autenticação bem-sucedida. No **primeiro terminal**, verificar os logs do processo completo de autorização PAP.

**9.** Verificar no primeiro terminal os logs da autenticação aceita, incluindo: recebimento da requisição, verificação do nome de usuário, comparação com senha conhecida (`pap: User authenticated successfully`) e envio do `Access-Accept`.

**10.** No segundo terminal, verificar o log da autenticação rejeitada com usuário inválido:
```bash
radtest usuarioX rnpesr 127.0.0.1 0 testing123
```
A resposta `Received Access-Reject` e a mensagem `Expected Access-Accept got Access-Reject` confirmam a rejeição.

**11.** *(Print solicitado)* O resultado do passo 10 exibe o `Access-Reject`, demonstrando que o servidor recusou corretamente um usuário não cadastrado.

**12.** Fechar os dois terminais.

## Aprendizado

- O FreeRADIUS centraliza a autenticação: qualquer dispositivo de rede configurado como cliente RADIUS pode delegar o controle de acesso ao servidor, eliminando a necessidade de gerenciar credenciais localmente em cada equipamento.
- O segredo compartilhado (`testing123` no laboratório) protege a comunicação entre o cliente RADIUS e o servidor — em produção, deve ser uma string longa e aleatória.
- O modo debug (`-X`) é fundamental para diagnóstico: exibe cada etapa do processo de autorização, identificando exatamente por que um acesso foi aceito ou rejeitado.
- O método PAP (Password Authentication Protocol) transmite a senha em texto claro dentro do pacote RADIUS — por isso o túnel entre cliente e servidor deve ser protegido. Em produção, métodos como EAP-TLS são preferidos.
- Mais informações: [https://freeradius.org/](https://freeradius.org/)
