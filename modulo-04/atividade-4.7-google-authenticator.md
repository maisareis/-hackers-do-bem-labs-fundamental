# Atividade 4.7 – Explorando o Google Authenticator no Kali Linux

## Objetivo

Configurar o **Google Authenticator** no Kali Linux para geração de tokens TOTP (Time-based One-Time Password), integrando autenticação de dois fatores (2FA) via QR Code com um smartphone.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **TOTP** (Time-based One-Time Password) é um algoritmo definido na RFC 6238 que gera senhas de uso único baseadas no tempo atual e em uma chave secreta compartilhada. O Google Authenticator implementa esse padrão, gerando códigos de 6 dígitos que se renovam a cada 30 segundos.

### Como funciona o TOTP

```
Chave Secreta + Tempo Atual → HMAC-SHA1 → Código de 6 dígitos (válido por 30s)
```

### Configurações definidas durante o setup

| Configuração                         | Escolha | Efeito                                                  |
|--------------------------------------|---------|---------------------------------------------------------|
| Tokens baseados em tempo             | Sim     | Usa TOTP (RFC 6238) em vez de HOTP (baseado em contador)|
| Atualizar arquivo de configuração    | Sim     | Salva configuração em `~/.google_authenticator`         |
| Desabilitar reuso do mesmo token     | Sim     | Previne ataques de replay — limita a 1 login por 30s   |
| Aumentar janela de tempo             | Sim     | Permite skew de até 4 minutos entre cliente e servidor  |
| Habilitar rate-limiting              | Sim     | Máximo de 3 tentativas a cada 30 segundos               |

### Scratch codes

Durante o setup, são gerados 5 **emergency scratch codes** — códigos de uso único para recuperação de acesso caso o dispositivo com o Authenticator seja perdido.

## Passo a Passo

**1.** Instalar o **Google Authenticator** no smartphone antes de iniciar.

**2.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**3.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**4.** Configurar o fuso horário para garantir sincronização correta dos tokens:
```bash
timedatectl set-timezone America/Sao_Paulo
timedatectl
```
Verificar que o NTP está ativo (`NTP service: active`) e o horário está correto.

**5.** Iniciar a configuração do Google Authenticator:
```bash
google-authenticator
```

**6.** Maximizar a janela do Terminal. Quando perguntado sobre tokens baseados em tempo, responder `y`:
```
Do you want authentication tokens to be time-based (y/n) y
```
Um QR Code será exibido no terminal, junto com a chave secreta.

**7.** No smartphone, abrir o Google Authenticator → botão **"+"** → **"Faça a leitura de um QR code"** → apontar a câmera para o QR Code exibido no terminal.

**8.** *(Print solicitado)* No terminal, digitar o código de 6 dígitos exibido no Google Authenticator do celular quando solicitado:
```
Enter code from app (-1 to skip): [CÓDIGO DO CELULAR]
Code confirmed
```
A confirmação `Code confirmed` e a exibição dos **emergency scratch codes** indicam que o setup foi concluído com sucesso.

**9.** Responder `y` para todas as perguntas de configuração subsequentes (atualizar arquivo, desabilitar reuso, aumentar janela, habilitar rate-limiting).

**10.** Fechar o Terminal.

## Aprendizado

- O 2FA com TOTP adiciona uma segunda camada de autenticação: mesmo que a senha seja comprometida, o atacante precisaria também do dispositivo físico do usuário para obter o código temporário.
- A chave secreta gerada é o elemento crítico — quem a possui pode gerar tokens válidos. Por isso, o QR Code e a chave jamais devem ser compartilhados ou exibidos em capturas de tela públicas.
- Os emergency scratch codes devem ser armazenados de forma segura (impressos e guardados fisicamente, ou em um gerenciador de senhas) — são o único meio de recuperação se o smartphone for perdido.
- O Google Authenticator é um padrão aberto (RFC 6238) — qualquer aplicativo compatível com TOTP (Authy, Microsoft Authenticator, OTPClient) pode ser usado no lugar.
- A sincronização de tempo entre cliente e servidor é fundamental: o parâmetro de janela de tempo estendida (4 minutos) compensa pequenas diferenças de relógio.
