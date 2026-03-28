# Atividade 11.8 – Redirecionando Requisições DNS com iptables no Kali Linux

## Objetivo
Demonstrar como redirecionar requisições DNS para o endereço loopback usando NAT com iptables, causando indisponibilidade de acesso a sites, e reverter a configuração ao final.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **DNS** (Domain Name System) é o protocolo responsável por traduzir nomes de domínio em endereços IP. Ele opera na **porta UDP 53** por padrão.

O **DNAT** (Destination NAT) é uma técnica de NAT que altera o endereço IP de destino de um pacote. Ao redirecionar as consultas DNS para o loopback (127.0.0.1), o sistema tenta resolver os nomes de domínio localmente — onde nenhum servidor DNS está rodando — causando falha em todas as resoluções de nomes e impossibilitando o acesso a qualquer site por nome de domínio.

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Mozilla Firefox disponível

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Confirmar acesso normal ao site
Abrir o Firefox em modo anônimo (`Ctrl + Shift + P`) → acessar `https://www.facebook.com/` → confirmar que carrega normalmente.

### 3. Fechar o Firefox

### 4. Adicionar a regra de redirecionamento DNS
```bash
iptables -t nat -A OUTPUT -p udp --dport 53 -j DNAT --to-destination 127.0.0.1:53
```

### Parâmetros do comando

| Parâmetro | Descrição |
|-----------|-----------|
| `-t nat` | Aplica a regra na tabela NAT |
| `-A OUTPUT` | Adiciona à chain de pacotes de saída |
| `-p udp` | Protocolo UDP |
| `--dport 53` | Porta de destino 53 (DNS) |
| `-j DNAT` | Ação: Destination NAT |
| `--to-destination 127.0.0.1:53` | Redireciona para o loopback |

### 5. Confirmar o bloqueio no navegador
Repetir o passo 2. O site não deve carregar, exibindo erro de DNS.

### 6. Verificar as regras NAT
```bash
iptables -t nat -L
```
A chain OUTPUT deve exibir a regra DNAT redirecionando UDP porta 53 para 127.0.0.1:53.

### 7. Remover a regra de redirecionamento
```bash
iptables -t nat -D OUTPUT -p udp --dport 53 -j DNAT --to-destination 127.0.0.1:53
```

### 8. Confirmar a remoção
```bash
iptables -t nat -L
```
A regra DNAT não deve mais aparecer na chain OUTPUT.

### 9. Confirmar que os sites voltaram a funcionar
Atualizar o Firefox — o Facebook deve carregar normalmente.

Fechar o Firefox e o Terminal.

---

## Por que o redirecionamento para loopback bloqueia tudo?

```
Requisição DNS normal:
Navegador → UDP/53 → Servidor DNS → Resposta com IP → Site carrega

Com redirecionamento DNAT:
Navegador → UDP/53 → 127.0.0.1:53 → Sem servidor DNS → Timeout → Site não carrega
```

---

## Aprendizado
- O DNS é a "agenda telefônica" da internet — sem ele, apenas acesso direto por IP funciona
- O DNAT na tabela NAT é mais poderoso que filtros simples — permite redirecionar tráfego para destinos completamente diferentes
- Esta técnica demonstra por que servidores DNS são infraestrutura crítica de segurança
- Em ambientes corporativos, o controle de acesso ao DNS é uma das principais formas de implementar políticas de navegação segura
