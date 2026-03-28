# Atividade 11.3 – Implementando MAC Cloning no Kali Linux

## Objetivo
Demonstrar a clonagem de endereço MAC na interface docker0 do Kali Linux, substituindo o MAC original por um MAC fictício e revertendo ao MAC original ao final.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais. O MAC Cloning pode ter implicações legais dependendo do contexto de uso. Utilize apenas em ambientes autorizados e com propósitos legítimos e éticos.

## Conceito

O **endereço MAC** (Media Access Control) é um identificador único de 48 bits atribuído à placa de rede de cada dispositivo. O **MAC Cloning** consiste em alterar temporariamente esse endereço para um valor diferente.

Usos legítimos do MAC Cloning incluem testes de segurança, compatibilidade com sistemas que autenticam por MAC e privacidade em redes públicas. Usos maliciosos incluem se passar por dispositivos autorizados para burlar controles de acesso.

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Interface docker0 disponível

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Identificar os endereços MAC das interfaces
```bash
ip link show
```
Anotar o MAC original da interface `docker0`.

### 3. Confirmar o MAC com ifconfig
```bash
ifconfig
```
> ⚠️ O endereço MAC do docker0 varia em cada VM. Usar sempre o valor exibido na própria máquina.

### 4. Desativar a interface docker0
```bash
ip link set docker0 down
```

### 5. Aplicar o MAC clonado
```bash
ip link set dev docker0 address 08:00:27:8b:c0:05
```

### 6. Reativar a interface
```bash
ip link set docker0 up
```

### 7. Confirmar a clonagem
```bash
ip link show
```
A interface docker0 deve exibir o novo MAC `08:00:27:8b:c0:05`.

### 8. Reverter para o MAC original
```bash
ip link set docker0 down
ip link set dev docker0 address [MAC_ORIGINAL]
ip link set docker0 up
```
> Substituir `[MAC_ORIGINAL]` pelo MAC anotado no passo 2.

### 9. Confirmar a restauração
```bash
ip link show
```
A interface docker0 deve exibir novamente o MAC original.

Fechar o Terminal.

---

## Explicação dos Comandos

| Comando | Descrição |
|---------|-----------|
| `ip link show` | Lista as interfaces e seus endereços MAC |
| `ip link set <iface> down` | Desativa a interface |
| `ip link set dev <iface> address <MAC>` | Altera o endereço MAC da interface |
| `ip link set <iface> up` | Reativa a interface |

---

## Aprendizado
- O endereço MAC pode ser alterado via software — não é um identificador imutável em nível de hardware
- É necessário desativar a interface antes de alterar o MAC e reativá-la após
- O MAC Cloning é reversível — o MAC original pode ser restaurado a qualquer momento
- Switches que implementam MAC Limiting ou Port Security podem detectar e bloquear mudanças de MAC suspeitas
