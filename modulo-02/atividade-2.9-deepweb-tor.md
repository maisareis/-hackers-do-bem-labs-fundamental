# Atividade 2.9 – Navegando na DeepWeb com Tor

## Objetivo
Navegar na DeepWeb utilizando o Tor Browser, acessando sites com domínio `.onion`.

## ⚠️ Aviso Legal
> O uso da rede Tor para atividades criminosas é **estritamente proibido** e sujeito a penalidades legais severas. Navegue com responsabilidade. Atividades como acesso ilegal a sistemas, distribuição de material ilegal e fraudes são monitoradas por autoridades internacionais.

## Conceito

### Surface Web vs Deep Web vs Dark Web

| Camada | Descrição | Acesso |
|--------|-----------|--------|
| **Surface Web** | Sites indexados pelo Google, Bing, etc. | Navegador comum |
| **Deep Web** | Conteúdo não indexado (e-mails, bancos, intranets) | Login/autenticação |
| **Dark Web** | Parte da Deep Web acessível via Tor | Tor Browser |

### Domínios .onion
Os sites da Dark Web utilizam domínios `.onion`, que são endereços criptográficos gerados automaticamente. Só são acessíveis através da rede Tor.

---

## Passo a Passo

### 1. Acessar o Hidden Wiki (Surface Web)
No Firefox ESR, acessar:
```
https://thehiddenwiki.org
```
Este site lista endereços `.onion` da Dark Web.

### 2. Acessar o Hidden Wiki via Tor
No Tor Browser, acessar:
```
http://6nhmgdpnyoljh5uzr5kwlatx2u3diou4ldeommfxjz3wkhalzgjqxzqd.onion
```

**Resultado:**
```
The Hidden Wiki
The Original Hidden Wiki - Only @ 6nhmgdpnyoljh5uzr5kwlatx2u3diou4ldeommfxjz3wkhalzgjqxzqd.onion
```

### 3. Explorar outros sites .onion
```
http://n6qisfgjauj365pxccpr5vizmtb5iavqaug7m7e4ewkxuygk5iim6yyd.onion
http://prjd5pmbug2cnfs67s3y65ods27vamswdaw2lnwf45ys3pjl55h2gwqd.onion
http://xf2gry25d3tyxkiu2xlvczd3q7jl6yyhtpodevjugnxia2u665asozad.onion
http://stormwayszuh4juycoy4kwoww5gvcu2c4tdtpkup667pdwe4qenzwayd.onion
```

> **Nota:** Alguns links podem estar fora do ar por ordens judiciais ou ataques cibernéticos.

---

## Características dos Sites .onion

- Endereços são sequências longas de caracteres aleatórios
- Não possuem certificado SSL tradicional (mas têm criptografia própria)
- Podem sair do ar a qualquer momento
- Carregam mais lentamente que sites comuns

---

## Usos Legítimos da Dark Web

- **SecureDrop** — plataforma para denúncias de jornalistas
- **The New York Times .onion** — acesso em países com censura
- **Facebook .onion** — acesso em países que bloqueiam o Facebook
- **Fóruns de privacidade** — discussões sobre segurança digital

---

## Aprendizado
- A Dark Web é uma pequena parte da Deep Web acessível via Tor
- Sites `.onion` só funcionam dentro da rede Tor
- Links da Dark Web mudam frequentemente e podem sair do ar
- A navegação anônima não torna o usuário imune à lei — autoridades monitoram a rede Tor
