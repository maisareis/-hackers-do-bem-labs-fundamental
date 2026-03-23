# Atividade 10.7 – Criando uma VM em Hypervisor Tipo 2 no Kali Linux

## Objetivo
Criar uma máquina virtual com a imagem Debian netinst no VirtualBox, configurando os recursos de memória, armazenamento e sistema operacional convidado.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **Debian netinst** (network install) é uma imagem mínima de instalação que contém apenas o necessário para iniciar o processo de instalação, baixando os demais pacotes via internet durante a configuração. Resulta em uma instalação mais leve e personalizada.

Uma **máquina virtual (VM)** é um ambiente computacional isolado que emula hardware físico, permitindo executar sistemas operacionais convidados dentro de um sistema hospedeiro.

> ⚠️ Ambientes de VM em nuvem (como a AWS usada neste lab) geralmente não possuem driver de virtualização aninhada (nested virtualization). Nesse caso, a VM pode ser criada mas não inicializada sem erro de kernel driver.

---

## Pré-requisitos
- Kali Linux com VirtualBox aberto (continuação da Atividade 10.6)
- Imagem ISO do Debian netinst disponível em `/curso/`

---

## Passo a Passo

### 1. Fechar aviso de USB e criar nova VM
Fechar a janela com a mensagem de erro de USB → clicar em **Novo**.

### 2. Definir o nome da VM
Inserir o nome `DebianNetinst`.

### 3. Selecionar a imagem ISO
No campo **Imagem ISO**, clicar em **Outro** → navegar por **Outros locais** → **Computador** → **curso** → selecionar `debian-12.5.0-amd64-netinst.iso` com 2 cliques.

Clicar em **Próximo(N)**.

### 4. Configurar o usuário do sistema convidado
No campo **Nome do Usuário**, alterar para `DebianNetinst`. Verificar que a senha padrão é `changeme` → clicar em **Próximo(N)**.

### 5. Alocar memória RAM
Alocar **512MB** de **Memória Base** → clicar em **Próximo(N)**.

### 6. Configurar o disco virtual
Modificar o **Tamanho do Disco** para **10.22 GB** → clicar em **Próximo(N)**.

### 7. Revisar o sumário e finalizar
Ler todo o sumário de configuração → clicar em **Finalizar**.

### 8. Confirmar a criação da VM
Verificar que a VM `DebianNetinst` aparece na coluna esquerda do VirtualBox.

### 9. Encerrar
Fechar todas as janelas e encerrar a conexão RDP.

---

## Configurações da VM criada

| Parâmetro | Valor |
|-----------|-------|
| **Nome** | DebianNetinst |
| **ISO** | debian-12.5.0-amd64-netinst.iso |
| **Memória RAM** | 512 MB |
| **Disco virtual** | 10.22 GB |
| **Usuário convidado** | DebianNetinst |

---

## Aprendizado
- O Debian netinst é ideal para criar VMs leves, pois permite escolher apenas os componentes necessários durante a instalação
- Em ambientes de nuvem sem suporte a virtualização aninhada, VMs podem ser criadas mas não inicializadas — em hardware físico o processo funciona normalmente
- A alocação de recursos (RAM e disco) deve ser balanceada com os recursos disponíveis no hospedeiro para não degradar a performance
- Esses mesmos passos podem ser reproduzidos em qualquer computador pessoal com VirtualBox instalado
