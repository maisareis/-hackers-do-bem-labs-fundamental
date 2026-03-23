# Atividade 10.6 – Explorando um Hypervisor Tipo 2 no Kali Linux

## Objetivo
Explorar as principais configurações e abas do VirtualBox no Kali Linux, compreendendo as opções disponíveis em um Hypervisor Tipo 2.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

Um **Hypervisor Tipo 2** (hosted hypervisor) é executado sobre um sistema operacional hospedeiro, diferente do Tipo 1 (bare-metal), que roda diretamente no hardware. O VirtualBox é um dos principais exemplos de Hypervisor Tipo 2.

| Tipo | Exemplo | Executa sobre |
|------|---------|--------------|
| **Tipo 1** | VMware ESXi, Hyper-V, KVM | Hardware diretamente |
| **Tipo 2** | VirtualBox, VMware Workstation | Sistema operacional hospedeiro |

O **VRDP** (VirtualBox Remote Desktop Protocol) permite conectar e controlar remotamente a interface de desktop de máquinas virtuais do VirtualBox pela rede, com suporte a autenticação via biblioteca `VBoxAuth`.

---

## Pré-requisitos
- Kali Linux com VirtualBox instalado

---

## Passo a Passo

### 1. Abrir o VirtualBox
Na barra de tarefas superior, clicar em **Aplicativos** → **Aplicações habituais** → **Sistema** → **Oracle VM Virtualbox**.

### 2. Acessar as Preferências
Clicar em **Arquivo** → **Preferências...**.

### 3. Verificar a aba Geral
Na aba **Geral**, observar:
- **Pasta Padrão para Máquinas:** `/home/aluno/VirtualBox VMs`
- **Biblioteca de Autenticação VRDP:** `VBoxAuth`

### 4. Explorar a aba Entrada — Gerenciador do VirtualBox
Na coluna esquerda, clicar em **Entrada** → aba **Gerenciador do VirtualBox** para ver os atalhos de teclado do gerenciador.

### 5. Explorar a aba Entrada — Máquina Virtual
Clicar na aba **Máquina Virtual** para ver os atalhos de teclado disponíveis durante a execução de VMs.

### 6. Explorar a aba Idioma
Na coluna esquerda, clicar em **Idioma** e verificar os idiomas disponíveis. A opção **Padrão** utiliza o idioma do sistema operacional hospedeiro (português neste caso).

### 7. Explorar a aba Tela
Na coluna esquerda, clicar em **Tela** e verificar as opções de interface gráfica:

| Opção | Descrição |
|-------|-----------|
| **Tamanho Máximo da Tela do Sistema Convidado** | Limita a resolução máxima da VM |
| **Fator de Escalonamento** | Ajusta o zoom da interface da VM |
| **Recursos Estendidos** | Habilita recursos gráficos adicionais |
| **Escalonamento de Fontes** | Ajusta o tamanho das fontes na interface |

### 8. Fechar as Preferências
Clicar em **OK**. Não fechar o VirtualBox — será utilizado na próxima atividade.

---

## Comparativo Hypervisor Tipo 1 vs Tipo 2

| Característica | Tipo 1 (Bare-metal) | Tipo 2 (Hosted) |
|---------------|---------------------|-----------------|
| **Performance** | ✅ Maior | ❌ Menor (overhead do SO hospedeiro) |
| **Uso típico** | Datacenters, produção | Desenvolvimento, testes, labs |
| **Exemplos** | VMware ESXi, Hyper-V | VirtualBox, VMware Workstation |
| **Gerenciamento** | Direto no hardware | Via SO hospedeiro |

---

## Aprendizado
- O VirtualBox é amplamente usado para labs educacionais por ser gratuito, open source e multiplataforma
- O VRDP permite acesso remoto às VMs sem a interface gráfica do VirtualBox, útil em servidores headless
- Hypervisors Tipo 2 são ideais para ambientes de desenvolvimento e testes por não exigirem hardware dedicado
- A pasta padrão de VMs pode ser alterada para discos com mais espaço, evitando problemas de armazenamento
