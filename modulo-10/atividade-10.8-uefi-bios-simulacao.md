# Atividade 10.8 – Explorando uma UEFI/BIOS por Simulação

## Objetivo
Explorar as principais seções de uma UEFI/BIOS moderna utilizando o simulador online da Lenovo, compreendendo as configurações de sistema, segurança e boot.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

A **UEFI** (Unified Extensible Firmware Interface) é a evolução moderna da BIOS (Basic Input/Output System). É o firmware responsável por inicializar o hardware antes do sistema operacional, oferecendo interface gráfica, suporte a discos maiores que 2TB (via GPT) e recursos avançados de segurança como Secure Boot.

O **Secure Boot** é um recurso da UEFI que garante que apenas software com assinatura digital confiável seja carregado durante a inicialização, impedindo a execução de bootloaders e kernels maliciosos.

---

## Pré-requisitos
- Computador pessoal com navegador web
- Acesso à internet

> ⚠️ Esta atividade é realizada no **computador pessoal**, não na VM da AWS.

---

## Passo a Passo

### 1. Acessar o simulador de BIOS da Lenovo
No navegador, acessar:
```
https://download.lenovo.com/bsco/index.html#/
```

### 2. Iniciar o simulador
Clicar em **Launch Simulator** → selecionar **Laptops**.

### 3. Selecionar fabricante e modelo
Selecionar **Lenovo Laptops** → **Lenovo Slim Pro 9 16IRP8 (83C0)** → **Graphics Interface Mode**.

### 4. Explorar a seção Information
Ler os campos disponíveis na tela inicial da BIOS:

| Campo | Descrição |
|-------|-----------|
| **Nome do produto** | Identificação do modelo do equipamento |
| **Versão da BIOS** | Versão atual do firmware instalado |
| **CPU** | Processador do equipamento |
| **Memória do sistema** | RAM instalada |
| **Secure Boot** | Status do Secure Boot (habilitado/desabilitado) |

### 5. Explorar a seção Configuration
Clicar em **Configuration** e analisar as configurações disponíveis:
- Hora e data do sistema
- Rede WLAN
- Graphics Device
- Intel(R) Virtualization Technology
- Always On USB
- Charge in Battery Mode
- System Performance Mode

### 6. Explorar a seção Security
Clicar em **Security** e analisar as opções de segurança:
- Set Hard Disk Password
- Intel Platform Trust Technology (TPM)
- Device Guard
- Secure Boot
- Reset to Setup Mode
- Restore Factory Keys

### 7. Explorar a seção Boot
Clicar em **Boot** e analisar as configurações de inicialização:
- USB Boot
- PXE Boot to LAN
- IPV4 PXE First
- EFI
- Windows Boot Manager
- EFI PXE Network

### 8. Sair sem salvar
Clicar em **Exit** → **Exit Discarding Changes** → **Yes**. Fechar o navegador.

---

## Principais seções da UEFI

| Seção | Função |
|-------|--------|
| **Information** | Dados do hardware e firmware |
| **Configuration** | Configurações de hardware e periféricos |
| **Security** | Senhas, TPM, Secure Boot |
| **Boot** | Ordem e opções de inicialização |
| **Exit** | Salvar, descartar ou restaurar configurações |

---

## Comparativo BIOS vs UEFI

| | BIOS | UEFI |
|---|---|---|
| **Interface** | Texto | Gráfica (com mouse) |
| **Disco máximo** | 2 TB (MBR) | Ilimitado (GPT) |
| **Tempo de boot** | Mais lento | Mais rápido |
| **Secure Boot** | ❌ Não suporta | ✅ Suporta |
| **Arquitetura** | 16 bits | 32/64 bits |

---

## Aprendizado
- A UEFI substituiu a BIOS em praticamente todos os computadores modernos, trazendo mais recursos de segurança e compatibilidade
- O Secure Boot é fundamental para prevenir ataques de bootkit — malware que se instala antes do sistema operacional
- A opção PXE Boot permite inicializar o sistema via rede, útil em ambientes corporativos para instalação em massa
- O Intel Virtualization Technology (VT-x) precisa estar habilitado na UEFI para que hypervisors como VirtualBox e VMware funcionem corretamente
