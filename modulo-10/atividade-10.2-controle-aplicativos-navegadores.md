# Atividade 10.2 – Controle de Aplicativos e Navegadores no Windows Server 2022

## Objetivo
Explorar e configurar os recursos de controle de aplicativos e navegadores do Windows Security, com foco em Reputation-based Protection e Exploit Protection no Windows Server 2022 (cliente).

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

A **Reputation-based Protection** utiliza dados de reputação coletados pela Microsoft para determinar se um arquivo ou aplicativo é potencialmente malicioso, bloqueando ou permitindo sua execução com base em histórico e confiabilidade da fonte.

A **Exploit Protection** protege o sistema operacional e aplicativos contra técnicas de exploração usadas por malware, aplicando diversas mitigações de segurança em nível de processo e sistema.

---

## Pré-requisitos
- Windows Server 2022 (cliente) com acesso como Administrator

---

## Passo a Passo

### 1. Acessar o Windows Server 2022 (cliente)
Conectar via RDP ao servidor cliente com usuário `Administrator`.

### 2. Abrir App & Browser Control
Na barra de tarefas, pesquisar por `App & browser control` e abrir.

### 3. Visualizar o status da Reputation-based Protection
Verificar que o campo **Reputation-based protection** está desabilitado.

### 4. Acessar as configurações de Reputation-based Protection
Clicar em **Reputation-based protection settings**.

### 5. Verificar o Check apps and files
Confirmar que **Check apps and files** está habilitado.

### 6. Habilitar o Potentially Unwanted App Blocking
Clicar no interruptor de **Off** para **On** no campo **Potentially unwanted app blocking**.

### 7. Voltar à página anterior
Clicar no botão de retorno (seta esquerda) no canto superior esquerdo.

### 8. Confirmar o status atualizado
Verificar que **Reputation-based protection** agora está habilitado.

### 9. Acessar o Exploit Protection Settings
Clicar em **Exploit protection settings**.

### 10. Analisar as proteções do sistema ativas por padrão
Verificar e ler as mitigações de segurança habilitadas na aba **System settings**:

| Proteção | Descrição |
|----------|-----------|
| **Control Flow Guard (CFG)** | Monitora o fluxo de execução para bloquear desvios maliciosos |
| **Data Execution Prevention (DEP)** | Impede execução de código em regiões de memória de dados |
| **Mandatory ASLR** | Carrega módulos em endereços de memória aleatórios a cada execução |
| **Bottom-up ASLR** | Aleatoriza alocações de memória na pilha e heap |
| **High-entropy ASLR** | ASLR com maior aleatoriedade, dificultando ataques de força bruta |
| **SEHOP** | Valida cadeias de exceção contra desvios maliciosos |
| **Validate heap integrity** | Detecta corrupção de memória no heap durante a execução |

Fechar todas as janelas.

---

## Comparativo das proteções de memória

| Proteção | Técnica | Objetivo |
|----------|---------|----------|
| DEP | Marca páginas de memória como não-executáveis | Bloqueia execução de shellcode em dados |
| ASLR | Randomização de endereços | Impede localização previsível de código |
| CFG | Validação de ponteiros de função | Bloqueia ataques de desvio de fluxo |
| SEHOP | Validação de cadeia de exceções | Protege contra SEH overwrites |
| Heap Integrity | Verificação de metadados do heap | Detecta heap sprays e overwrites |

---

## Aprendizado
- A Reputation-based Protection é uma camada de segurança baseada em inteligência coletiva da Microsoft — quanto mais dados, mais precisa a detecção
- O bloqueio de PUAs (Potentially Unwanted Applications) reduz a superfície de ataque de softwares legítimos com comportamentos indesejados
- As proteções de Exploit Protection funcionam em conjunto — DEP + ASLR + CFG formam camadas complementares de defesa
- Essas mitigações dificultam significativamente a exploração mesmo quando uma vulnerabilidade é encontrada
