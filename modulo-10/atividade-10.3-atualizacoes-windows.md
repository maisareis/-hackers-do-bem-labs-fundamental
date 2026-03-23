# Atividade 10.3 – Explorando as Atualizações no Windows Server 2022

## Objetivo
Verificar, instalar e configurar as atualizações do Windows Server 2022 (cliente), explorando as opções avançadas de atualização e o histórico de atualizações do sistema.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

As **atualizações do Windows** são patches de segurança, correções de bugs e melhorias de funcionalidade distribuídos pela Microsoft. Manter o sistema atualizado é uma das medidas mais eficazes de segurança, pois elimina vulnerabilidades conhecidas exploradas por atacantes.

O **Windows Update** gerencia o download e a instalação automática dessas atualizações, com opções de agendamento e controle sobre o que é instalado.

---

## Pré-requisitos
- Windows Server 2022 (cliente) com acesso como Administrator

---

## Passo a Passo

### 1. Acessar o Windows Server 2022 (cliente)
Conectar via RDP ao servidor cliente com usuário `Administrator`.

### 2. Abrir o Windows Update Settings
Na barra de tarefas, pesquisar por `Windows Update settings` e abrir.

### 3. Verificar atualizações disponíveis
Clicar no botão azul **Check for updates** e aguardar o download e instalação.

### 4. Reiniciar o servidor se necessário
Clicar em **Restart Now** se solicitado. Aguardar 5 minutos e reconectar via RDP, repetindo os passos 1 e 2.

### 5. Confirmar sistema atualizado
Verificar a mensagem **You're up to date**.

### 6. Configurar opções avançadas e voltar
Clicar em **Advanced options** → ativar todos os campos de **Update options** e **Update notifications** → clicar na seta de retorno no canto superior esquerdo.

### 7. Visualizar histórico de atualizações
Clicar em **View update history** para ver as atualizações instaladas.

Fechar todas as janelas.

---

## Opções de Update avançadas

| Opção | Função |
|-------|--------|
| **Receive updates for other Microsoft products** | Atualiza também Office, .NET e outros produtos Microsoft |
| **Download updates over metered connections** | Baixa atualizações mesmo em conexões com limite de dados |
| **Restart notifications** | Notifica antes de reiniciar automaticamente |
| **Active hours** | Define janela de tempo sem reinicializações automáticas |

---

## Aprendizado
- Manter o sistema atualizado é a medida de segurança com melhor custo-benefício — a maioria dos ataques bem-sucedidos explora vulnerabilidades com patches disponíveis
- O histórico de atualizações permite auditar quais patches foram instalados e quando, útil em análises forenses
- Em servidores de produção, é recomendado testar atualizações em ambiente de homologação antes de aplicar em produção
- As notificações de reinicialização são críticas em servidores, pois reinicializações inesperadas podem causar indisponibilidade de serviços
