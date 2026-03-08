# Atividade 6.5 – Automação de Tarefas com PowerShell no Windows Server

## Objetivo
Explorar scripts PowerShell para automatizar coleta de informações de sistema e eventos no Windows Server 2022.

## Conceito
O **PowerShell** é uma linguagem de script e shell de linha de comando da Microsoft, muito utilizada para automação de tarefas em ambientes Windows. O cmdlet `Get-CimInstance` permite consultar informações do sistema via WMI (Windows Management Instrumentation).

---

## Script 1 – Coleta de Informações de Hardware

```powershell
$systemInfo = Get-CimInstance -ClassName Win32_ComputerSystem
$osInfo = Get-CimInstance -ClassName Win32_OperatingSystem
$cpuInfo = Get-CimInstance -ClassName Win32_Processor
$memoryInfo = Get-CimInstance -ClassName Win32_PhysicalMemory

Write-Host "Informações do Sistema:"
Write-Host "Nome do Computador: $($systemInfo.Name)"
Write-Host "Sistema Operacional: $($osInfo.Caption)"
Write-Host "Arquitetura do Sistema: $($osInfo.OSArchitecture)"
Write-Host "Processador: $($cpuInfo.Name)"
Write-Host "Memória Total: $($memoryInfo.Capacity / 1GB) GB"
```

### Classes WMI Utilizadas

| Classe | Informação Coletada |
|--------|-------------------|
| `Win32_ComputerSystem` | Nome do computador |
| `Win32_OperatingSystem` | Sistema operacional e arquitetura |
| `Win32_Processor` | Modelo do processador |
| `Win32_PhysicalMemory` | Capacidade de memória RAM |

---

## Script 2 – Relatório de Eventos de Inicialização

```powershell
$events = Get-WinEvent -LogName System | Where-Object { $_.Id -eq 6005 -or $_.Id -eq 6006 }

$reportFile = "C:\Relatorio_Eventos_Inicializacao.txt"

$events | ForEach-Object {
    $eventTime = $_.TimeCreated
    $eventMessage = $_.Message
    $report = "Data e Hora: $eventTime`nMensagem: $eventMessage`n`n"
    $report | Out-File -Append -FilePath $reportFile
}

Write-Host "Relatório salvo em $reportFile"
```

### IDs de Eventos Monitorados

| ID | Descrição |
|----|-----------|
| `6005` | Inicialização do sistema (Event Log started) |
| `6006` | Desligamento do sistema (Event Log stopped) |

---

## Passo a Passo

### 1. Abrir o PowerShell
Pesquisar "Windows PowerShell" na barra de tarefas.

### 2. Executar o Script 1
Colar e executar o script de coleta de hardware.

### 3. Executar o Script 2
Colar e executar o script de eventos de inicialização.

### 4. Verificar o relatório gerado
Abrir o arquivo `C:\Relatorio_Eventos_Inicializacao.txt` no File Explorer.

---

## Aprendizado
- PowerShell permite automatizar tarefas administrativas no Windows
- `Get-CimInstance` consulta informações do sistema via WMI
- `Get-WinEvent` acessa o registro de eventos do Windows
- `Out-File -Append` adiciona conteúdo a um arquivo sem sobrescrever
- IDs de eventos 6005 e 6006 registram inicializações e desligamentos do sistema
