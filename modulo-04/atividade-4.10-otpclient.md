# Atividade 4.10 – Gerenciando TOTPs com o OTPClient no Kali Linux

## Objetivo

Utilizar o **OTPClient** para criar e gerenciar um banco de dados de tokens TOTP no Kali Linux, integrando com o Google Authenticator para validação de tokens em tempo real.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **OTPClient** é uma aplicação desktop para Linux que gerencia tokens TOTP e HOTP de forma segura, armazenando as chaves secretas em um banco de dados criptografado (`.enc`). É uma alternativa ao uso de smartphone para geração de tokens, projetada especificamente para ambientes desktop Linux.

### Diferença entre OTPClient e Google Authenticator CLI

| Recurso                    | `google-authenticator` (CLI) | OTPClient (GUI)                        |
|----------------------------|------------------------------|----------------------------------------|
| Interface                  | Terminal                     | Gráfica (GTK)                          |
| Banco de dados             | `~/.google_authenticator`    | Arquivo `.enc` criptografado           |
| Múltiplos tokens           | Um por arquivo               | Múltiplos tokens em um banco           |
| Informações exibidas       | Apenas código                | Código + validade + tipo + emissor     |
| Proteção                   | Arquivo de texto              | Criptografia com senha mestra          |

### Campos exibidos por token no OTPClient

| Campo       | Descrição                                           |
|-------------|-----------------------------------------------------|
| `Type`      | Tipo do token (TOTP ou HOTP)                        |
| `Account`   | Nome da conta associada ao token                    |
| `Issuer`    | Emissor/serviço (ex.: Kali Linux, Google, GitHub)   |
| `OTP Value` | Código de 6 dígitos atual                           |
| `Validity`  | Segundos restantes antes da renovação do código     |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal com o usuário `aluno` e iniciar o OTPClient:
```bash
otpclient
```
Aceitar o aviso sobre `memlock` clicando em **OK**.

**3.** Na interface do OTPClient, clicar em **"Create new database"**.

**4.** Na janela de salvar, acessar a pasta **Documentos** e manter o nome `NewDatabase.enc` → **OK**.

**5.** Inserir e confirmar a senha de criptografia do banco de dados → **OK**. Se aparecer a janela "Unlock Login Keyring", inserir as credenciais do Kali Linux e clicar em **Unlock**. Aceitar o aviso inicial de primeiro uso.

**6.** Abrir um **segundo terminal** e iniciar o Google Authenticator:
```bash
google-authenticator
```

**7.** Ativar TOTP respondendo `y` e copiar a chave secreta exibida:
```
Your new secret key is: [CHAVE_SECRETA]
```

**8.** Voltar ao OTPClient e clicar no botão **"+"** (superior esquerdo) → **"Manually"**.

**9.** Preencher os campos:
- **Account**: qualquer nome descritivo
- **Issuer**: qualquer nome descritivo
- **Secret**: colar a chave secreta do passo 7

**10.** *(Print solicitado)* Clicar na linha do token criado e verificar que os campos **Type**, **Account**, **Issuer**, **OTP Value** e **Validity** estão sendo exibidos com valores válidos — o contador de validade decrementando em tempo real.

**11.** Fechar o OTPClient. No primeiro terminal, pressionar `Ctrl+C` e limpar os arquivos criados:
```bash
sudo -i
cd /home/aluno/Documentos
rm *
```
Confirmar a exclusão com `y`. Fechar os terminais.

## Aprendizado

- O OTPClient armazena múltiplos tokens em um único banco criptografado, sendo mais organizado que arquivos individuais do `google-authenticator` para gerenciar vários serviços com 2FA.
- O campo **Validity** exibe em tempo real quantos segundos restam para o token atual expirar — fundamental para saber quando usar o código antes que ele seja renovado.
- A senha do banco `.enc` é a única proteção das chaves secretas: se esquecida, não há recuperação possível (conforme aviso exibido durante a criação).
- O aviso sobre `memlock` indica que o sistema pode não ter memória suficiente bloqueada para proteger as chaves em RAM contra swap — em sistemas de produção, esse limite deve ser ajustado.
- O OTPClient implementa o mesmo padrão RFC 6238 do Google Authenticator: tokens gerados por ambos são intercambiáveis para a mesma chave secreta.
