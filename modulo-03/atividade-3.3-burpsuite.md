# Atividade 3.3 – Interceptando tráfego de navegador com o Burp Suite no Kali Linux

## Objetivo

Utilizar o **Burp Suite** como proxy interceptador para capturar e analisar o tráfego HTTP/HTTPS gerado pelo navegador Mozilla Firefox no Kali Linux.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**. A interceptação de tráfego de terceiros sem autorização é ilegal e pode configurar crime conforme a legislação vigente.

## Conceito

O **Burp Suite** é uma plataforma de teste de segurança para aplicações web desenvolvida pela PortSwigger. Na sua edição Community, oferece um proxy interceptador que se posiciona entre o navegador e o servidor web, permitindo visualizar, modificar e redirecionar requisições e respostas HTTP/HTTPS.

### Fluxo de interceptação

```
Firefox → Burp Suite (proxy 127.0.0.1:8080) → Servidor Web
```

Para interceptar tráfego HTTPS, é necessário instalar o certificado CA do Burp Suite no navegador, permitindo que ele atue como intermediário no handshake TLS.

### Abas principais do Burp Suite

| Aba              | Função                                                         |
|------------------|----------------------------------------------------------------|
| `Proxy`          | Controla a interceptação de requisições                        |
| `HTTP history`   | Histórico de todas as requisições interceptadas                |
| `Intercept`      | Visualiza e libera/modifica requisições em tempo real          |
| `Proxy Settings` | Configura a interface e porta do proxy listener                |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal com usuário padrão e iniciar o Burp Suite:
```bash
burpsuite
```
Aceitar o aviso de versão JRE marcando "Don't show again for the JRE" e clicar em "OK". Aceitar os Termos e Condições.

**3.** Na tela de projeto, selecionar **"Temporary Project in memory"** → **"Next"**.

**4.** Selecionar **"Use Burp defaults"** → **"Start Burp"**.

**5.** Na aba **"Proxy"**, verificar que o botão exibe **"Intercept is off"**.

**6.** Acessar **"Proxy Settings"** e confirmar que há um listener ativo em `127.0.0.1:8080`.

**7.** Abrir o Mozilla Firefox e tentar acessar `http://burp` — a página não abrirá ainda.

**8.** Configurar o proxy no Firefox:
- Menu → **Settings** → buscar "Proxy" → **Network Settings**
- Selecionar **"Manual proxy configuration"**
- HTTP Proxy: `127.0.0.1` | Port: `8080`
- Ativar **"Also use this proxy for HTTPS"** → **OK**

**9.** Acessar novamente `http://burp` no Firefox — desta vez a página do Burp Suite será exibida. Clicar em **"CA Certificate"** para baixar o arquivo `cacert.der`.

**10.** Instalar o certificado no Firefox:
- Ir em **Settings** → **Privacy & Security** → **View Certificates** → **Import**
- Selecionar `cacert.der` → **Abrir**
- Ativar as duas opções "Trust this CA to..." → **OK**

**11.** Fechar a janela de Settings do Burp Suite e retornar à janela principal.

**12.** Na aba **"Proxy"**, clicar em **"Intercept is off"** para ativar — o botão passará a exibir **"Intercept is on"**.

**13.** No Firefox, abrir nova aba e acessar `https://casasbahia.com.br/`. O navegador ficará travado — o Burp Suite interceptou a requisição.

**14.** No Burp Suite, observar os detalhes capturados: Host, Cookie, User-Agent e demais cabeçalhos HTTP.

**15.** *(Print solicitado)* Clicar na aba **"HTTP history"** e observar o registro da requisição interceptada do Firefox.

**16.** Voltar à aba **"Intercept"** e clicar em **"Forward"** para liberar o acesso — o site abrirá no Firefox.

**17.** Desfazer a configuração de proxy no Firefox: **Network Settings** → **"No Proxy"** → **OK**.

**18.** Fechar o Burp Suite, o Firefox e o Terminal.

## Aprendizado

- O Burp Suite atua como um Man-in-the-Middle legítimo no ambiente de testes, permitindo inspecionar cada requisição antes que ela alcance o servidor.
- A instalação do certificado CA é necessária para que o navegador confie no certificado gerado dinamicamente pelo Burp para cada site HTTPS.
- A aba "HTTP history" registra todas as requisições passadas pelo proxy, mesmo sem interceptação ativa — útil para análise posterior.
- O botão "Forward" libera a requisição retida, enquanto "Drop" a descarta — funcionalidades centrais em testes de segurança de aplicações web.
