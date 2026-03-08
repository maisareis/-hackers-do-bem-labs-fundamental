# Atividade 2.8 – Navegador Tor no Kali Linux

## Objetivo
Configurar e utilizar o navegador Tor para navegar anonimamente, demonstrando o mascaramento do IP real.

## Conceito

### Rede Tor (The Onion Router)
A **rede Tor** é uma rede de anonimato que roteia o tráfego da internet através de múltiplos servidores voluntários (nós), criptografando cada camada como uma cebola. O destino final vê apenas o IP do **nó de saída (exit node)**, não o IP real do usuário.

```
Usuário → Nó 1 → Nó 2 → Nó 3 (Exit Node) → Internet
   IP real     [criptografado em cada camada]      IP do Exit Node
```

---

## Passo a Passo

### 1. Iniciar o Tor Browser
```bash
torbrowser-launcher
```
O launcher baixa e verifica a assinatura do Tor Browser automaticamente.

### 2. Conectar à rede Tor
- Clicar no botão roxo **"Conectar"**
- Se falhar: clicar em **"Try a bridge"**

### 3. Verificar o funcionamento
Acessar no Tor Browser:
```
https://whatismyipaddress.com/
```

**Resultado no Tor Browser:**
```
IPv4: 185.220.100.240
ISP: Stiftung Erneuerbare Freiheit
Services: Tor Exit Node
City: Frankfurt am Main
Country: Germany
```

### 4. Comparar com o Firefox normal
Acessar o mesmo site no Firefox ESR:
```
IPv4: 107.22.123.86
ISP: Amazon.com Inc.
City: Ashburn
Region: Virginia
Country: United States
```

**Conclusão:** IPs completamente diferentes — o Tor mascarou com sucesso o IP real.

---

## Diferença Tor vs VPN

| Característica | Tor | VPN |
|---------------|-----|-----|
| Anonimato | Alto (múltiplos nós) | Médio (1 servidor) |
| Velocidade | Lenta | Rápida |
| Custo | Gratuito | Geralmente pago |
| Confiança necessária | Nenhuma | No provedor VPN |
| Acesso a .onion | ✅ Sim | ❌ Não |

---

## Casos de Uso Legítimos do Tor

- Jornalistas em países com censura
- Denunciantes (whistleblowers)
- Pesquisadores de segurança
- Usuários em países com internet censurada
- Privacidade pessoal legítima

---

## Aprendizado
- O Tor substitui o IP real pelo IP do nó de saída
- O nó de saída pode estar em qualquer país do mundo
- O Tor é mais lento que a navegação normal devido aos múltiplos saltos
- Bridges são usadas quando o Tor está bloqueado pelo ISP
- O uso do Tor para atividades ilegais é monitorado por autoridades internacionais
