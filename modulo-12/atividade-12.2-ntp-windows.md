# Atividade 12.2 – Habilitando NTP no Windows Server 2022

## Objetivo
Configurar o Windows Server 2022 para sincronização de tempo via NTP usando o servidor do Google, e verificar o status da sincronização via PowerShell.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **NTP** (Network Time Protocol) é o protocolo responsável pela sincronização de relógios em redes de computadores. A precisão do tempo é crítica em segurança da informação por vários motivos:

- **Correlação de logs:** logs de sistemas diferentes devem ter timestamps alinhados para análise de incidentes
- **Certificados digitais:** certificados têm validade baseada em data/hora — relógios errados causam falhas de autenticação
- **Autenticação Kerberos:** o Active Directory rejeita autenticações com diferença de tempo superior a 5 minutos

O NTP usa uma hierarquia de **estratos** (stratum): stratum 0 são as referências atômicas/GPS, stratum 1 são servidores sincronizados diretamente com stratum 0, e assim por diante.

---

## Pré-requisitos
- Windows Server 2022 com acesso como Administrator
- Continuação da Atividade 12.1 (sessão RDP ativa)

---

## Passo a Passo

### 1. Abrir o Group Policy Editor
Na barra de pesquisa, escrever `gpedit.msc` e abrir.

### 2. Navegar até Time Providers
Expandir **Computer Configuration** → **Administrative Templates** → **System** → **Windows Time Service** → clicar em **Time Providers**.

### 3. Habilitar o NTP Server
Clicar 2 vezes em **Enable Windows NTP Server** → selecionar **Enabled** → **OK**.

### 4. Habilitar o NTP Client
Clicar 2 vezes em **Enable Windows NTP Client** → selecionar **Enabled** → **OK**.

### 5. Configurar o servidor NTP
Clicar 2 vezes em **Configure Windows NTP Client** → selecionar **Enabled** → no campo **NtpServer**, inserir:
```
time.google.com,0x9
```
Clicar em **OK**.

> O sufixo `,0x9` indica o modo de polling do NTP (combinação de unicast e especial).

### 6. Abrir o PowerShell como Administrator
Na barra de pesquisa, escrever `Windows PowerShell` → clicar com o botão direito → **Run as administrator**.

### 7. Parar o serviço Windows Time
```powershell
net stop w32time
```

### 8. Iniciar o serviço Windows Time
```powershell
net start w32time
```

### 9. Forçar a sincronização
```powershell
w32tm /resync
```

### 10. Verificar o status do NTP
```powershell
w32tm /query /status
```

### Campos da saída do comando

| Campo | Descrição |
|-------|-----------|
| **Leap Indicator** | Aviso de segundo extra (0 = sem aviso) |
| **Stratum** | Nível hierárquico da fonte de tempo |
| **Precision** | Estimativa de desvio do relógio |
| **Root Delay** | Latência até a fonte primária |
| **Root Dispersion** | Dispersão dos relógios em relação à fonte primária |
| **ReferenceId** | Identificador da fonte de tempo atual |
| **Last Successful Sync Time** | Data/hora da última sincronização bem-sucedida |
| **Source** | Fonte de tempo atual |
| **Poll Interval** | Frequência de atualização (em potência de 2 segundos) |

Fechar todas as janelas.

---

## Aprendizado
- O NTP é infraestrutura crítica — sem ele, logs de diferentes sistemas ficam dessincronizados, dificultando investigações forenses
- O servidor `time.google.com` é um servidor stratum 1 altamente confiável e disponível gratuitamente
- O serviço `w32tm` é o componente NTP nativo do Windows — substitui o `net time` em versões modernas
- Em domínios Active Directory, o Domain Controller é automaticamente configurado como fonte NTP para os clientes do domínio
