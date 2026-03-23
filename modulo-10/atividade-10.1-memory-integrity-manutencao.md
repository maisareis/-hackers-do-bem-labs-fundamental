# Atividade 10.1 – Explorando Integridade de Memória e Manutenção no Windows Server 2022

## Objetivo
Habilitar o recurso Memory Integrity do Windows Security e explorar as funcionalidades de manutenção e confiabilidade do sistema no Windows Server 2022 (cliente).

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **Windows Security** é o centro unificado de segurança do Windows Server 2022, anteriormente conhecido como Windows Defender Security Center. Centraliza funcionalidades de proteção, monitoramento e configuração de segurança.

A **Core Isolation** utiliza virtualização baseada em hardware para isolar processos críticos do kernel dos demais componentes do sistema, reduzindo a superfície de ataque contra rootkits e ataques ao kernel.

A **Memory Integrity** (Integrity Protection) protege a memória do kernel contra injeção de código malicioso e exploração de vulnerabilidades, usando tecnologias de execução confiável e verificação de integridade de código.

O **Security and Maintenance** permite visualizar histórico de eventos, falhas e problemas de estabilidade, além de gerenciar tarefas de manutenção automática do servidor.

---

## Pré-requisitos
- Windows Server 2022 (cliente) com acesso como Administrator

---

## Passo a Passo

### 1. Acessar o Windows Server 2022 (cliente)
Conectar via RDP ao servidor cliente com usuário `Administrator`.

### 2. Abrir o Device Security
Na barra de tarefas, pesquisar por `Device security` e abrir.

### 3. Visualizar a Core Isolation
Ler a seção **Core Isolation** e compreender o isolamento baseado em virtualização.

### 4. Acessar Core Isolation Details
Clicar em **Core isolation details**.

### 5. Visualizar o Memory Integrity
Ler a seção **Memory Integrity** e compreender a proteção contra manipulação de memória.

### 6. Habilitar o Memory Integrity
Clicar no interruptor de **Off** para **On** para habilitar o Memory Integrity.

### 7. Reiniciar o servidor
Na barra de tarefas, clicar com o botão direito no símbolo do Windows → **Shut down or sign out** → **Restart** → **Continue**.

Aguardar 3 minutos e reconectar via RDP.

### 8. Abrir o Security and Maintenance
Na barra de tarefas, pesquisar por `Security and Maintenance` e abrir.

### 9. Visualizar o histórico de confiabilidade
Clicar em **Maintenance** → **View reliability history** para ver o histórico de eventos e falhas do sistema.

### 10. Visualizar todos os relatórios de problemas
Clicar em **View all problem reports** → **OK** 2 vezes para sair.

### 11. Configurações de manutenção
Clicar em **Maintenance** → **Change Maintenance Settings** para ver as opções de agendamento → **Cancel**.

### 12. Iniciar a manutenção
Clicar em **Maintenance** → **Start maintenance**.

### 13. Confirmar o início da manutenção
Verificar o aviso **Maintenance in progress** confirmando que o processo foi iniciado. Fechar todas as janelas.

---

## Recursos do Windows Security

| Recurso | Função |
|---------|--------|
| **Core Isolation** | Isola processos do kernel via virtualização |
| **Memory Integrity** | Protege memória do kernel contra injeção de código |
| **Reliability History** | Histórico de eventos, falhas e atualizações |
| **Problem Reports** | Relatórios detalhados de problemas do sistema |
| **Maintenance Settings** | Agendamento de tarefas automáticas de manutenção |

---

## Aprendizado
- A Memory Integrity requer reinicialização para entrar em vigor — e pode não estar disponível em ambientes de VM sem suporte a virtualização aninhada
- O Core Isolation é a camada de proteção que torna possível a Memory Integrity, usando o Hyper-V para criar um ambiente isolado
- O histórico de confiabilidade é uma ferramenta essencial para diagnóstico de problemas intermitentes em servidores
- O Security and Maintenance centraliza tanto a segurança quanto a saúde operacional do servidor
