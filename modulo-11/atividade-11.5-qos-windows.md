# Atividade 11.5 – Configurando QoS no Windows Server 2022

## Objetivo
Configurar uma política de QoS no Windows Server 2022 (cliente) via Group Policy Editor para limitar a taxa de upload a 1 MBps (~8 Mbps), verificar o efeito com teste de velocidade e remover a política ao final.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **QoS baseado em política** (Policy-based QoS) no Windows permite controlar o tráfego de rede de aplicações e serviços através de GPOs (Group Policy Objects). Utiliza dois mecanismos principais:

**DSCP** (Differentiated Services Code Point): campo de 6 bits no cabeçalho IP usado para marcar pacotes com níveis de prioridade (0 a 63). Roteadores compatíveis com QoS usam essa marcação para priorizar o encaminhamento.

**Outbound Throttle Rate**: limita a taxa máxima de transmissão de pacotes de saída, independentemente da prioridade DSCP.

---

## Pré-requisitos
- Windows Server 2022 (cliente) com acesso como Administrator
- Acesso à internet para o teste de velocidade

---

## Passo a Passo

### 1. Medir a taxa de upload antes da limitação
Abrir o Microsoft Edge e acessar:
```
https://www.minhaconexao.com.br/
```
Verificar que o upload está acima de 20 Mbps. Fechar o Edge.

### 2. Abrir o Group Policy Editor
Na barra de pesquisa, escrever `gpedit.msc` e abrir.

### 3. Navegar até Policy-based QoS
Expandir **Computer Configuration** → **Windows Settings** → clicar com o botão direito em **Policy-based QoS** → **Create new policy...**.

### 4. Configurar a política de QoS
Na janela **Policy-based QoS**:
- **Policy name:** `Teste`
- **DSCP value:** `0` (sem marcação de prioridade)
- Ativar **Specify Outbound Throttle Rate** e definir `1 MBps`
- Clicar em **Next**

### 5. Definir o escopo da política
- Selecionar **All applications** → **Next**
- Selecionar **Any source IP address** → **Next**
- Selecionar **From any source port** e **From any destination port** → **Finish**

### 6. Confirmar a criação da regra
Verificar que a regra `Teste` aparece no campo direito do Group Policy Editor.

### 7. Verificar o efeito no Group Policy Editor
A regra limita todas as conexões de saída a 1 MBps (equivalente a ~8 Mbps).

### 8. Repetir o teste de velocidade
Abrir o Microsoft Edge e acessar novamente o site de teste de velocidade. Verificar que o upload ficou entre 7 e 8 Mbps.

### 9. Remover a política de QoS
Voltar ao Group Policy Editor → clicar com o botão direito na regra `Teste` → **Delete policy** → **Yes**.

Fechar todas as janelas.

---

## Comparativo antes e depois do QoS

| | Sem QoS | Com QoS (1 MBps throttle) |
|---|---|---|
| **Upload** | > 20 Mbps | 7–8 Mbps |
| **Download** | Normal | Normal (não afetado) |

---

## Aprendizado
- 1 MBps (megabyte) = 8 Mbps (megabit) — a conversão é importante para entender o impacto real do throttle
- O DSCP sozinho não limita a velocidade — apenas marca os pacotes para que roteadores compatíveis priorizem o encaminhamento
- O Outbound Throttle Rate é o mecanismo que efetivamente restringe a taxa de transmissão
- As políticas de QoS no Windows são aplicadas via GPO — podem ser distribuídas para múltiplas máquinas em um domínio Active Directory
