# Atividade 12.4 – Capturando Pacotes com Wireshark no Kali Linux

## Objetivo
Capturar tráfego HTTPS na interface eth0 com o Wireshark, aplicar filtros de exibição e analisar o fluxo TCP criptografado de uma conexão real.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **Wireshark** é o analisador de protocolos de rede mais utilizado no mundo. Permite capturar e inspecionar pacotes em tempo real em uma interface de rede.

O **HTTPS** (HTTP sobre TLS) criptografa o conteúdo das comunicações web. Mesmo capturando os pacotes, o conteúdo permanece ilegível sem a chave privada do servidor — apenas os metadados (IPs, portas, tamanhos) são visíveis.

---

## Pré-requisitos
- Kali Linux com Wireshark instalado
- Mozilla Firefox disponível

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Identificar a interface de rede
```bash
ifconfig
```
Verificar que `eth0` é a interface de saída para a internet.

### 3. Abrir o Wireshark
**Aplicativos** → **09 – Descoberta** → **wireshark** (inserir senha se solicitado).

### 4. Iniciar a captura na interface eth0
Clicar 2 vezes em `eth0` para iniciar o monitoramento.

### 5. Gerar tráfego HTTPS
Abrir o Mozilla Firefox → acessar `https://esr.rnp.br/` → aguardar o carregamento completo → fechar o Firefox.

### 6. Parar a captura
No Wireshark, clicar no quadrado vermelho (**Stop**).

### 7. Conhecer as colunas do Wireshark

| Coluna | Descrição |
|--------|-----------|
| **Time** | Momento da captura em relação ao início |
| **Source** | Endereço IP de origem do pacote |
| **Destination** | Endereço IP de destino do pacote |
| **Protocol** | Protocolo identificado (TLS, TCP, DNS, etc.) |
| **Length** | Tamanho do pacote em bytes |
| **Info** | Resumo do conteúdo do pacote |

### 8. Aplicar filtro para HTTPS
Na barra de filtro (onde está escrito `Apply a display filter`), digitar:
```
tcp.port == 443
```
A coluna Protocol deve exibir predominantemente `TLSv1.2` ou `TLSv1.3`.

### 9. Seguir o fluxo TCP
Clicar com o botão direito em qualquer linha → **Seguir** → **Fluxo TCP**.

### 10. Observar o conteúdo ilegível
A janela exibida mostrará dados incompreensíveis — confirmando que a criptografia HTTPS está funcionando corretamente.

### 11. Fechar o Wireshark
Fechar sem salvar (**Sair sem Salvar**) e fechar o Terminal.

---

## Por que o conteúdo HTTPS é ilegível?

```
HTTP (sem criptografia):
GET /login HTTP/1.1
User: admin
Password: 123456
← Legível no Wireshark!

HTTPS (com TLS):
[Dados criptografados: 8f3a2c...]
← Ilegível sem a chave privada do servidor
```

---

## Aprendizado
- O Wireshark captura pacotes no nível da interface de rede — vê tudo que trafega, independente do protocolo
- HTTPS garante que mesmo com captura de pacotes, o conteúdo permanece protegido
- O filtro `tcp.port == 443` isola todo o tráfego HTTPS na captura
- O fluxo TCP mostra a conversa completa entre cliente e servidor — essencial para análise de protocolos e debugging de aplicações
