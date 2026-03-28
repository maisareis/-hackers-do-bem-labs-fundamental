# Atividade 11.9 – Usando OpenVPN no Kali Linux

## Objetivo
Conectar o Kali Linux a um servidor VPN gratuito usando OpenVPN, verificar a alteração do IP de saída e localização geográfica, e encerrar a conexão ao final.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **OpenVPN** é uma solução de código aberto para implementação de VPNs (Virtual Private Networks). Utiliza o protocolo **SSL/TLS** para estabelecer um túnel criptografado entre o cliente e o servidor VPN, garantindo confidencialidade e integridade do tráfego.

Ao se conectar a uma VPN, todo o tráfego de saída é roteado pelo servidor VPN — o endereço IP público visível para os sites é o do servidor VPN, não o real do cliente.

O **VPNBook** é um serviço de VPN gratuito que disponibiliza servidores em diferentes países com arquivos de configuração OpenVPN.

---

## Pré-requisitos
- Kali Linux com OpenVPN instalado
- Mozilla Firefox disponível
- Acesso à internet

---

## Passo a Passo

### 1. Acessar o site do VPNBook e baixar o pacote
Abrir o Firefox → acessar `https://www.vpnbook.com/` → localizar a seção **Free OpenVPN and PPTP VPN** → aba **OpenVPN**.

### 2. Escolher um servidor fora dos EUA
Selecionar um servidor que não comece com "Download US" (ex: CA, DE, FR). Anotar o usuário e senha exibidos abaixo dos links de download.

### 3. Extrair o arquivo baixado
Abrir o Thunar → navegar até `/home/aluno/Downloads/` → clicar com o botão direito no arquivo baixado (começa com `vpnbook`) → **Extrair aqui**.

### 4. Acessar como superusuário
```bash
sudo -i
```

### 5. Navegar até a pasta extraída
```bash
cd /home/aluno/Downloads/vpnbook-openvpn-[servidor]/
```

### 6. Conectar ao servidor VPN
```bash
openvpn --config vpnbook-[servidor]-tcp80.ovpn
```
Inserir o usuário e senha do site quando solicitado.

### 7. Confirmar a conexão estabelecida
Verificar a última linha da saída:
```
Initialization Sequence Completed
```

### 8. Verificar o novo IP de saída
No Firefox, acessar:
```
https://whatismyipaddress.com/
```
O IP e a localização exibidos devem corresponder ao servidor VPN escolhido, não ao IP real da máquina.

### 9. Encerrar a conexão VPN
No Terminal, pressionar `Ctrl+C`.

### 10. Verificar que o IP voltou ao original
Atualizar o site no Firefox — o IP deve ser novamente o da AWS (EUA).

### 11. Limpar os arquivos baixados
```bash
cd /home/aluno/Downloads
rm -r *
```

Fechar o Firefox e o Terminal.

---

## Fluxo de tráfego com e sem VPN

```
Sem VPN:
Kali Linux → Internet → IP AWS (EUA) visível para os sites

Com VPN:
Kali Linux → Túnel SSL/TLS → Servidor VPN (Canadá) → Internet → IP do servidor VPN visível
```

---

## Aprendizado
- A VPN mascara o IP real do cliente — os sites enxergam apenas o IP do servidor VPN
- O OpenVPN usa SSL/TLS — o mesmo protocolo que protege conexões HTTPS
- VPNs gratuitas como o VPNBook têm limitações de velocidade e privacidade — para uso profissional, soluções pagas e auditadas são recomendadas
- A conexão VPN roteia TODO o tráfego pelo servidor — incluindo DNS, evitando vazamentos de DNS
