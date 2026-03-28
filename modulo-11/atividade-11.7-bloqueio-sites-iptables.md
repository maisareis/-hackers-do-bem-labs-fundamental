# Atividade 11.7 – Bloqueando Sites com iptables no Kali Linux

## Objetivo
Bloquear o acesso a um site específico utilizando o iptables no Kali Linux, verificar o efeito do bloqueio no navegador e remover a regra ao final.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **iptables** é o firewall padrão do kernel Linux, baseado no framework **netfilter**. Permite criar regras para filtrar, redirecionar ou descartar pacotes de rede com base em critérios como IP de origem/destino, porta e protocolo.

As **chains** (cadeias) principais são:
- **INPUT:** pacotes destinados ao sistema local
- **OUTPUT:** pacotes originados pelo sistema local
- **FORWARD:** pacotes que passam pelo sistema (roteamento)

O alvo **DROP** descarta silenciosamente os pacotes — o remetente não recebe nenhuma resposta, diferente do **REJECT** que envia uma mensagem de erro.

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

### 2. Abrir o Firefox em modo anônimo e acessar o site
Abrir o Mozilla Firefox → `Ctrl + Shift + P` → acessar o site alvo sem `https://`. Confirmar que o site carrega normalmente.

### 3. Fechar o Firefox

### 4. Adicionar a regra de bloqueio
```bash
iptables -A INPUT -s globo.com.br -j DROP
```

### Parâmetros do comando

| Parâmetro | Descrição |
|-----------|-----------|
| `-A INPUT` | Adiciona regra à chain de entrada |
| `-s globo.com.br` | Origem: endereço IP ou hostname a bloquear |
| `-j DROP` | Ação: descartar silenciosamente os pacotes |

### 5. Verificar a regra criada
```bash
iptables -L
```
A chain INPUT deve exibir a regra DROP para o domínio bloqueado.

### 6. Confirmar o bloqueio no navegador
Repetir o passo 2. O site não deve carregar.

### 7. Remover a regra de bloqueio
```bash
iptables -D INPUT -s globo.com.br -j DROP
```
O parâmetro `-D` deleta a regra correspondente.

### 8. Confirmar a remoção
```bash
iptables -L
```
A chain INPUT não deve mais exibir a regra DROP.

### 9. Confirmar que o site voltou a funcionar
Acessar novamente o site no Firefox.

Fechar o Firefox e reiniciar o Kali Linux:
```bash
reboot
```

---

## Comparativo -A vs -D no iptables

| Parâmetro | Ação |
|-----------|------|
| `-A` (Append) | Adiciona regra ao final da chain |
| `-D` (Delete) | Remove a regra especificada da chain |
| `-I` (Insert) | Insere regra no início da chain |
| `-L` (List) | Lista todas as regras da chain |

---

## Aprendizado
- O iptables resolve automaticamente nomes de domínio para IPs ao criar regras com `-s`
- O DROP não envia resposta ao cliente — o navegador fica aguardando até o timeout
- As regras do iptables são voláteis — são perdidas após reinicialização, a menos que salvas com `iptables-save`
- Para bloqueios mais robustos em produção, usar ferramentas como `ufw` (Uncomplicated Firewall) ou soluções de proxy/DNS
