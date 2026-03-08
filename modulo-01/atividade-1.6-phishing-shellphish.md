# Atividade 1.6 – Phishing com ShellPhish

## Objetivo
Conhecer e simular um ataque de phishing utilizando a ferramenta ShellPhish no Kali Linux.

## ⚠️ Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente de laboratório controlado, com fins educacionais. O uso de ferramentas de phishing contra pessoas sem autorização é **crime**.

## Conceito

### Phishing
O **phishing** é um ataque de engenharia social onde o atacante cria páginas falsas de sites legítimos para capturar credenciais de usuários desavisados:

1. Atacante cria página falsa idêntica ao site legítimo
2. Vítima acessa o link e insere suas credenciais
3. Credenciais são capturadas pelo atacante
4. Vítima é redirecionada para o site real, sem perceber o ataque

### ShellPhish
O **ShellPhish** é uma ferramenta educacional que cria páginas falsas de login de redes sociais e serviços populares. Suporta mais de 29 plataformas, incluindo Facebook, Instagram, Google, Netflix, entre outras.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Acessar a pasta da ferramenta
```bash
cd /curso/ShellPhish
ls
# capture.png  README.md  screenshot.png  sites  LICENSE  shellphish.sh  update.sh
```

### 3. Executar o ShellPhish
```bash
bash shellphish.sh
```

### 4. Configurar a página falsa
```
[~] Select an option: 1              ← Facebook
[~] Select an option: 1              ← Traditional Login Page
[~] Select a Port Forwarding: 1      ← LocalHost
[~] Select a Port (Default: 5555): 5050
```

**Saída:**
```
[~] Successfully Hosted at: http://localhost:5050
[~] Waiting for Login Info, Press Ctrl + C to exit ...
```

### 5. Acessar a página falsa no Firefox
```
http://localhost:5050
```

### 6. Inserir credenciais de teste
- **Usuário:** `teste_usuario`
- **Senha:** `teste_senha`
- Clicar em **Log In** → redireciona automaticamente para o Facebook real

### 7. Credenciais capturadas no terminal
```
[*] Victim IP Found!
[~] Victim IP: 127.0.0.1
[~] Saved: sites/facebook/victim_ip.txt

[*] Login info Found!
[~] Account: teste_usuario
[~] Password: teste_senha
[~] Saved: sites/facebook/login_info.txt
```

---

## Plataformas Suportadas

| # | Plataforma | # | Plataforma |
|---|-----------|---|-----------|
| 01 | Facebook | 11 | Twitch |
| 02 | Instagram | 12 | Pinterest |
| 03 | Google | 13 | Snapchat |
| 04 | Microsoft | 14 | LinkedIn |
| 05 | Netflix | 15 | eBay |
| 06 | PayPal | 16 | Dropbox |
| 07 | Steam | 17 | ProtonMail |
| 08 | Twitter | 18 | Spotify |
| 09 | PlayStation | 19 | Reddit |
| 10 | GitHub | 20 | Adobe |

---

## Como se Proteger

- Verificar sempre a URL antes de inserir credenciais
- Usar autenticação de dois fatores (2FA)
- Desconfiar de links recebidos por e-mail ou mensagens
- Verificar o certificado SSL do site (cadeado)
- Usar gerenciadores de senha que validam o domínio automaticamente

---

## Aprendizado
- Páginas de phishing são visualmente idênticas às originais
- As credenciais são capturadas antes do redirecionamento para o site real
- O IP da vítima também é registrado junto com as credenciais
- 2FA protege a conta mesmo que a senha seja comprometida
