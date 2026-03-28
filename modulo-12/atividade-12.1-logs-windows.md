# Atividade 12.1 – Explorando os Logs no Windows Server 2022

## Objetivo
Explorar os diferentes tipos de logs disponíveis no Event Viewer do Windows Server 2022, compreender os campos de cada entrada de log e utilizar os filtros de pesquisa disponíveis.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **Event Viewer** (Visualizador de Eventos) é a ferramenta central de auditoria e diagnóstico do Windows. Registra eventos gerados pelo sistema operacional, aplicações e serviços, sendo essencial para análise de incidentes de segurança e troubleshooting.

### Tipos de logs em Windows Logs

| Log | Conteúdo |
|-----|---------|
| **Application** | Eventos gerados por aplicativos e programas |
| **Security** | Logins, autenticações, mudanças de política de segurança |
| **Setup** | Instalação e configuração de componentes do SO |
| **System** | Eventos do próprio sistema operacional (hardware, drivers, boot) |
| **Forwarded Events** | Eventos recebidos de outros computadores da rede |

---

## Pré-requisitos
- Windows Server 2022 com Active Directory configurado (Atividades 5.1–5.7)
- Atividades 9.1–9.3, 9.6 e 9.7 concluídas
- Acesso como Administrator

---

## Passo a Passo

### 1. Abrir o Event Viewer
Na barra de pesquisa, escrever `eventvwr.msc` e abrir.

### 2. Explorar os tipos de log
Na coluna esquerda, expandir **Windows Logs** e clicar em cada tipo para visualizar o histórico correspondente.

### 3. Abrir um evento do log Application
Clicar em **Application** → clicar 2 vezes no primeiro log exibido e analisar os campos:

| Campo | Descrição |
|-------|-----------|
| **Log Name** | Categoria do log (Application, Security, etc.) |
| **Source** | Programa ou componente que gerou o evento |
| **Event ID** | Número único identificador do tipo de evento |
| **Level** | Gravidade: Information, Warning, Error, Critical |
| **User** | Usuário associado ao evento |
| **OpCode** | Operação específica executada |
| **Logged** | Data e hora do registro |
| **Task Category** | Categoria da tarefa relacionada ao evento |
| **Computer** | Computador onde o evento ocorreu |

Clicar em **Close**.

### 4. Explorar os demais logs
Clicar em **Security**, **Setup**, **System** e **Forwarded Events** sequencialmente para ver seus históricos.

### 5. Acessar o filtro do log Application
Voltar a **Application** → no painel direito (Actions), clicar em **Filter Current Log...**.

### 6. Explorar os parâmetros de filtro
Na janela **Filter Current Log**, analisar os campos disponíveis:

| Campo de Filtro | Descrição |
|----------------|-----------|
| **Logged** | Filtro por intervalo de datas |
| **Event logs** | Tipo de log (Application por padrão) |
| **Event sources** | Filtrar por programa/componente específico |
| **Task category** | Filtrar por categoria de tarefa |
| **Keywords** | Filtrar por marcadores de evento (ex: Audit Success) |
| **Users** | Filtrar por usuário específico |
| **Computer(s)** | Filtrar por computador específico |

Fechar todas as janelas sem encerrar a sessão RDP.

---

## Aprendizado
- O Event Viewer é a ferramenta de auditoria nativa do Windows — fundamental para análise forense e resposta a incidentes
- O **Security log** é o mais crítico para segurança — registra logins, falhas de autenticação e mudanças de privilégio
- O **Event ID** é padronizado pela Microsoft — é possível pesquisar qualquer ID para entender o evento correspondente
- Os filtros permitem isolar eventos específicos em logs com milhares de entradas, tornando a análise viável
