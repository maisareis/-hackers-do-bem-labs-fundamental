# Atividade 11.6 – Explorando o ACL no Firewall do Windows Server 2022

## Objetivo
Explorar as principais seções do Windows Defender Firewall with Advanced Security no Windows Server 2022, compreendendo Inbound Rules, Outbound Rules, Connection Security Rules, Monitoring e Security Associations.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **Windows Defender Firewall with Advanced Security** é um firewall stateful que opera nas camadas 3 e 4 do modelo OSI. Permite criar regras granulares de controle de tráfego baseadas em programa, porta, protocolo, endereço IP e usuário.

As **ACLs** (Access Control Lists) de firewall são listas de regras que definem o que é permitido ou bloqueado — equivalente ao `iptables` no Linux.

---

## Pré-requisitos
- Windows Server 2022 (cliente) com acesso como Administrator

---

## Passo a Passo

### 1. Abrir o Windows Defender Firewall with Advanced Security
Na barra de pesquisa, escrever `Windows Defender Firewall with Advanced Security` e abrir.

### 2. Explorar as Inbound Rules
Clicar em **Inbound Rules** na coluna esquerda. Controlam o tráfego que **entra** no sistema — permitem ou bloqueiam conexões de entrada com base em critérios como programa, porta e IP de origem.

### 3. Explorar as Outbound Rules
Clicar em **Outbound Rules**. Controlam o tráfego que **sai** do sistema — permitem ou bloqueiam conexões de saída, como acesso a sites ou serviços específicos.

### 4. Explorar as Connection Security Rules
Clicar em **Connection Security Rules**. Usadas para configurar conexões seguras (VPN, IPSec) entre o sistema local e outros dispositivos, com autenticação e criptografia.

### 5. Explorar o Monitoring
Clicar em **Monitoring**. Exibe informações em tempo real sobre o tráfego e as regras ativas.

### 6. Explorar Monitoring → Firewall
Expandir **Monitoring** → clicar em **Firewall**. Exibe:
- Conexões **permitidas** pelas regras ativas
- Conexões **bloqueadas** pelo firewall
- Regras de firewall em vigor
- Detalhes individuais de cada conexão

### 7. Explorar Monitoring → Connection Security Rules
Expandir **Monitoring** → clicar em **Connection Security Rules**. Exibe conexões seguras estabelecidas e tentativas descartadas.

### 8. Explorar Security Associations → Main Mode
Expandir **Monitoring** → **Security Associations** → **Main Mode**. Exibe as negociações IKE (Internet Key Exchange) no modo principal para estabelecimento de VPNs:
- **Autenticação:** troca de identidades entre os dispositivos
- **Negociação de chave:** acordo sobre algoritmos de criptografia
- **Estabelecimento da conexão:** início da comunicação segura

### 9. Explorar Security Associations → Quick Mode
Expandir **Monitoring** → **Security Associations** → **Quick Mode**. Exibe as negociações após o Main Mode:
- **Renovação de chaves:** atualização periódica das chaves de criptografia
- **Gerenciamento de tráfego:** definição de quais IPs/portas serão protegidos
- **Verificação de integridade:** garantia de que os dados não foram modificados

Fechar todas as janelas.

---

## Resumo das Seções do Firewall

| Seção | Função |
|-------|--------|
| **Inbound Rules** | Controla tráfego de entrada |
| **Outbound Rules** | Controla tráfego de saída |
| **Connection Security Rules** | Gerencia conexões seguras (VPN/IPSec) |
| **Monitoring → Firewall** | Visualiza conexões permitidas e bloqueadas em tempo real |
| **Security Associations → Main Mode** | Negociações IKE iniciais para VPN |
| **Security Associations → Quick Mode** | Renovação de chaves e gerenciamento de tráfego VPN |

---

## Aprendizado
- O Windows Firewall Advanced Security é significativamente mais poderoso que o firewall básico — permite regras por aplicação, usuário e IP
- As Security Associations registram todo o ciclo de vida de conexões VPN IPSec — útil para diagnóstico de problemas de conectividade
- O Main Mode e Quick Mode fazem parte do protocolo IKE usado pelo IPSec para negociar parâmetros de segurança
- Em ambientes sem VPN ativa, as seções Main Mode e Quick Mode estarão vazias
