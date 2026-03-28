# Atividade 11.2 – Implementando Alias de Interface de Rede no Kali Linux

## Objetivo
Criar aliases de interface de rede (IPs secundários) na interface eth0 do Kali Linux, verificar sua funcionalidade via ping e conexão SSH, e remover as configurações ao final.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

Um **alias de interface de rede** permite associar múltiplos endereços IP a uma única interface física. No Linux, aliases são criados com a notação `eth0:1`, `eth0:2`, etc. São úteis para:

- Hospedar múltiplos serviços com IPs distintos em um único servidor
- Testar configurações de rede sem hardware adicional
- Simular múltiplos dispositivos em um único host

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Serviço SSH ativo no Kali Linux

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Editar o arquivo de interfaces de rede
```bash
nano /etc/network/interfaces
```

### 3. Adicionar os aliases no final do arquivo
```
auto eth0:1
iface eth0:1 inet static
    address [IP_ALIAS_1]
    netmask 255.255.255.0

auto eth0:2
iface eth0:2 inet static
    address [IP_ALIAS_2]
    netmask 255.255.255.0
```
Salvar com `Ctrl+X`, `s`, `Enter`.

### 4. Reiniciar para aplicar as configurações
```bash
reboot
```
Aguardar e reconectar via RDP.

### 5. Acessar como superusuário novamente
```bash
sudo -i
```

### 6. Verificar os aliases configurados
```bash
ip addr show eth0
```
A saída deve exibir o IP principal e os dois IPs secundários (`eth0:1` e `eth0:2`) com `scope global secondary`.

### Campos relevantes da saída

| Campo | Descrição |
|-------|-----------|
| `mtu 9001` | Jumbo Frames — comum em ambientes virtualizados como AWS |
| `scope global dynamic eth0` | IP principal atribuído via DHCP |
| `scope global secondary eth0:1` | Alias 1 configurado estaticamente |
| `scope global secondary eth0:2` | Alias 2 configurado estaticamente |

### 7. Testar conectividade com os aliases via ping
```bash
ping [IP_ALIAS_1]
ping [IP_ALIAS_2]
```
Ambos devem responder com 0% de perda.

### 8. Conectar via SSH ao alias de rede
```bash
ssh aluno@[IP_ALIAS_2]
```
A conexão SSH ao IP alias demonstra que o endereço secundário está ativo e funcional. Após confirmar, sair com `exit`.

### 9. Remover os aliases
```bash
nano /etc/network/interfaces
```
Apagar as linhas adicionadas no passo 3. Salvar com `Ctrl+X`, `s`, `Enter`.

### 10. Reiniciar para remover as configurações
```bash
reboot
```

---

## Aprendizado
- Aliases de interface permitem múltiplos IPs em uma única placa de rede sem hardware adicional
- As configurações em `/etc/network/interfaces` são persistentes — sobrevivem a reinicializações
- O MTU 9001 (Jumbo Frames) é comum em instâncias AWS para melhorar o desempenho em redes de alta velocidade
- IPs alias funcionam como IPs normais — aceitam conexões SSH, HTTP e qualquer outro protocolo
