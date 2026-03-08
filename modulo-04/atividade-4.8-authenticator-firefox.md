# Atividade 4.8 – Incorporando o Google Authenticator ao Mozilla Firefox

## Objetivo

Integrar o Google Authenticator ao Mozilla Firefox por meio do plugin **Authenticator by mymindstorm**, permitindo geração de tokens TOTP diretamente no navegador sem necessidade de smartphone.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O plugin **Authenticator** para Firefox implementa o mesmo algoritmo TOTP (RFC 6238) utilizado pelo Google Authenticator para smartphones. Isso permite usar o navegador como segundo fator de autenticação, útil em ambientes onde o uso de celular é restrito ou impraticável.

### Fluxo da atividade

Esta atividade usa dois ambientes em paralelo:

| Ambiente              | Função                                                    |
|-----------------------|-----------------------------------------------------------|
| Computador pessoal    | Firefox com plugin Authenticator instalado                |
| VM Kali Linux (AWS)   | Geração da chave secreta via `google-authenticator`       |

A chave secreta gerada no Kali é transferida para o plugin do Firefox via Clipboard do Remote Connection.

### Diferença entre smartphone e plugin de navegador

| Recurso               | Smartphone             | Plugin Firefox            |
|-----------------------|------------------------|---------------------------|
| Requer celular        | Sim                    | Não                       |
| Portabilidade         | Alta                   | Vinculado ao computador   |
| Risco se comprometido | Phishing de tela       | Acesso ao perfil do browser|
| Padrão TOTP           | RFC 6238               | RFC 6238                  |

## Passo a Passo

**1.** No **computador pessoal** (não na VM), abrir o Mozilla Firefox e acessar:
```
https://addons.mozilla.org/en-US/firefox/addon/auth-helper/
```

**2.** Clicar em **"Add to Firefox"** ao lado de "Authenticator by mymindstorm" → **"Adicionar/Add"** → **"OK"**. O ícone do plugin aparecerá na barra de navegação.

**3.** Conectar à VM Landing e iniciar o Kali Linux via RDP com as credenciais do laboratório.

**4.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**5.** Iniciar o Google Authenticator no Kali Linux:
```bash
google-authenticator
```

**6.** Ativar TOTP respondendo `y`. Anotar a chave secreta exibida:
```
Your new secret key is: [CHAVE_SECRETA]
```

**7.** Copiar a chave secreta para o computador pessoal usando a ferramenta **Clipboard** do Remote Connection.

**8.** No computador pessoal, clicar no ícone do plugin Authenticator no Firefox → ícone do lápis → ícone **"+"** → **"Inserir Manualmente"**.

**9.** Preencher os campos:
- **Emissor**: `Kali Linux` (ou nome descritivo)
- **Segredo**: colar a chave secreta copiada no passo 7

Clicar em **OK**.

**10.** *(Print solicitado)* Clicar novamente no ícone do Authenticator e verificar que o código TOTP está sendo gerado no navegador — o mesmo código que seria gerado por um smartphone com o Google Authenticator configurado com a mesma chave.

**11.** Fechar o Firefox no computador pessoal. Voltar ao Kali Linux e fechar o Terminal.

## Aprendizado

- O plugin do Firefox e o app de smartphone geram exatamente o mesmo código para a mesma chave secreta e mesmo instante de tempo — porque ambos implementam o mesmo padrão aberto RFC 6238.
- A chave secreta é o elemento que precisa ser protegido: qualquer pessoa ou sistema que a possua pode gerar tokens válidos indefinidamente.
- Em ambientes corporativos, o uso de plugin de navegador como segundo fator é menos seguro que um dispositivo físico dedicado (hardware token), pois o navegador pode ser comprometido por extensões maliciosas ou malware.
- A transferência da chave via Clipboard de Remote Connection demonstra um vetor de vazamento em ambientes de área de trabalho remota — dados copiados entre ambientes podem ser interceptados.
- Para cenários de alta segurança, recomenda-se o uso de tokens FIDO2/WebAuthn (chaves físicas como YubiKey) em vez de TOTP.
