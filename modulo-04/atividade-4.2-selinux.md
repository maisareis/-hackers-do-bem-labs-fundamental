# Atividade 4.2 – Explorando a ferramenta SELinux no Kali Linux

## Objetivo

Ativar, configurar e inspecionar o **SELinux** no Kali Linux, compreendendo seus modos de operação, usuários, políticas de login, variáveis booleanas e definições de portas.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.  
> ⚠️ **ATENÇÃO: Não execute os passos fora da ordem indicada. Erros na configuração do SELinux podem causar indisponibilidade de acesso ao Kali Linux.**

## Conceito

O **SELinux** (Security-Enhanced Linux) é um conjunto de modificações no kernel Linux que implementa o **Controle de Acesso Obrigatório (MAC)**. Diferente do modelo tradicional baseado em permissões de usuário/grupo (DAC), o SELinux associa rótulos de segurança a arquivos, processos e recursos, aplicando políticas mais granulares independentemente das permissões do usuário.

### Modos de operação do SELinux

| Modo          | Comportamento                                                                 |
|---------------|-------------------------------------------------------------------------------|
| `enforcing`   | Aplica as políticas — nega acessos não autorizados e registra violações       |
| `permissive`  | Não nega acessos — apenas registra violações em `/var/log/audit/audit.log`    |
| `disabled`    | SELinux completamente desativado                                               |

### Principais comandos SELinux

| Comando              | Descrição                                                      |
|----------------------|----------------------------------------------------------------|
| `selinux-activate`   | Ativa o SELinux e reconfigura o GRUB                          |
| `sestatus`           | Exibe o status atual do SELinux                               |
| `semanage user -l`   | Lista usuários SELinux e seus papéis/níveis MLS               |
| `semanage login -l`  | Lista mapeamentos entre usuários de login e usuários SELinux  |
| `semanage boolean -l`| Lista variáveis booleanas e seus estados                      |
| `semanage port -l`   | Lista tipos de porta e seus protocolos/números                |

### Papéis (roles) SELinux

| Papel          | Descrição                                                              |
|----------------|------------------------------------------------------------------------|
| `guest_r`      | Acesso mínimo — usuários convidados                                    |
| `user_r`       | Acesso padrão de usuário comum                                         |
| `staff_r`      | Acesso de equipe administrativa                                        |
| `sysadm_r`     | Acesso de administrador do sistema                                     |
| `system_r`     | Acesso de processos do sistema                                         |
| `unconfined_r` | Acesso irrestrito (sem confinamento SELinux)                           |
| `xguest_r`     | Acesso isolado para convidados em ambientes gráficos                   |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Ativar o SELinux (reconfigura o GRUB e prepara o relabeling):
```bash
selinux-activate
```
O comando gera novo arquivo de configuração do GRUB incorporando suporte ao kernel SELinux. A mensagem `SE Linux is activated. You may need to reboot now.` confirma a ativação.

**4.** Reiniciar a máquina virtual:
```bash
reboot
```
Durante a reinicialização, o SELinux realiza o *relabeling* de todo o sistema de arquivos, o que pode levar aproximadamente 12 minutos. A conexão RDP será interrompida — isso é esperado.

**5.** Aguardar ~12 minutos e reconectar via RDP com as credenciais do laboratório.

**6.** *(Print solicitado)* Verificar o status do SELinux após a reinicialização:
```bash
sudo -i
sestatus
```
O status exibirá: `SELinux status: enabled`, modo atual `permissive`, política carregada `default`, MLS habilitado e versão máxima de política do kernel `33`.

**7.** Listar usuários SELinux, seus prefixos MLS e papéis:
```bash
semanage user -l
```

**8.** Listar mapeamentos de login para usuários SELinux:
```bash
semanage login -l
```
O usuário `__default__` e `root` são mapeados para `unconfined_u`; o `sddm` (gerenciador de display) é mapeado para `xdm`.

**9.** Listar variáveis booleanas e seus estados:
```bash
semanage boolean -l
```
Cada booleana controla um comportamento específico de segurança — a maioria inicia desabilitada (`off, off`).

**10.** Listar tipos de porta SELinux:
```bash
semanage port -l
```

**11.** Desabilitar o SELinux editando o arquivo de configuração:
```bash
nano /etc/selinux/config
```
Alterar `SELINUX=permissive` para `SELINUX=disabled`. Salvar com `Ctrl+X` → `S` → `Enter`.

**12.** Reiniciar a máquina:
```bash
reboot
```

**13.** Aguardar ~3 minutos, reconectar via RDP e confirmar que o SELinux está desabilitado:
```bash
sudo -i
sestatus
```
Resultado esperado: `SELinux status: disabled`.

## Aprendizado

- O SELinux adiciona uma camada de segurança independente das permissões tradicionais do Linux: mesmo que um processo seja comprometido, as políticas SELinux limitam o que ele pode acessar.
- O modo `permissive` é útil para diagnóstico — permite observar quais acessos seriam negados em modo `enforcing` sem interromper o funcionamento do sistema.
- O *relabeling* é necessário na primeira ativação pois o SELinux precisa associar rótulos de segurança a todos os arquivos do sistema de arquivos.
- Variáveis booleanas permitem ajustar políticas sem recompilar — por exemplo, habilitar acesso a Bluetooth para usuários convidados (`xguest_use_bluetooth`).
- Em ambientes de produção, o modo `enforcing` é o recomendado para máxima segurança.
