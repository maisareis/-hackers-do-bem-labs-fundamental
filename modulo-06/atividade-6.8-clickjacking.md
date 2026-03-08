# Atividade 6.8 – Ataque Clickjacking

## Objetivo
Entender e demonstrar o funcionamento do ataque Clickjacking utilizando um arquivo HTML com iframe.

## Conceito
O **Clickjacking** (ou UI Redressing) é um ataque onde uma página maliciosa engana o usuário para clicar em elementos de outro site, sem que ele perceba. Isso é feito sobrepondo um iframe transparente sobre um elemento visível da página.

---

## Como Funciona o Ataque

1. O usuário acessa uma página maliciosa
2. A página exibe um botão ou link aparentemente inofensivo
3. Sobre esse elemento, é posicionado um `<iframe>` transparente de outro site
4. O usuário clica no que parece ser o botão, mas na verdade clica no iframe

---

## Código HTML de Demonstração

```html
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
</head>
<body>
  <style>
    iframe {
      width: 400px;
      height: 100px;
      position: absolute;
      top: 5px;
      left: -14px;
      opacity: 0.03;  /* quase invisível */
      z-index: 1;
    }
  </style>

  <div>Click to get rich now:</div>
  <iframe src="https://www.facebook.com"></iframe>
  <button>Click here!</button>
  <div>...And you're cool!</div>
</body>
</html>
```

---

## Propriedades CSS Chave do iframe

| Propriedade | Valor | Efeito |
|-------------|-------|--------|
| `position: absolute` | - | Sobrepõe o iframe sobre outros elementos |
| `opacity: 1` | 100% visível | Ataque visível, não funciona |
| `opacity: 0` | Invisível | Ataque oculto |
| `opacity: 0.03` | Quase invisível | Simula ataque real |
| `z-index: 1` | Camada superior | Garante que o iframe fique na frente |

---

## Testando os Níveis de Opacidade

| opacity | Resultado |
|---------|-----------|
| `1` | iframe totalmente visível, sem ataque |
| `0.5` | iframe parcialmente visível |
| `0.03` | iframe quase invisível, simula ataque real |
| `0` | iframe invisível |

---

## Proteção dos Navegadores Modernos
Navegadores modernos como o Firefox possuem proteções contra Clickjacking, impedindo que iframes de outros domínios capturem cliques diretamente. Porém, o ataque ainda pode ser explorado por usuários menos atentos.

---

## Como se Proteger (lado do servidor)

### Cabeçalho X-Frame-Options
```
X-Frame-Options: DENY
X-Frame-Options: SAMEORIGIN
```

### Content Security Policy (CSP)
```
Content-Security-Policy: frame-ancestors 'none'
Content-Security-Policy: frame-ancestors 'self'
```

---

## Aprendizado
- Clickjacking explora a confiança do usuário em interfaces visuais
- Iframes transparentes são usados para sobrepor conteúdo malicioso
- Navegadores modernos possuem proteções nativas contra esse ataque
- Cabeçalhos HTTP como `X-Frame-Options` e `CSP` protegem sites contra Clickjacking
