# Atividade 4.9 – Verificando a presença de TPM e USB no Kali Linux

## Objetivo

Verificar a presença do módulo **TPM** (Trusted Platform Module) e de portas **USB** no Kali Linux, compreendendo sua relevância para autenticação por hardware token.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

### TPM (Trusted Platform Module)

O **TPM** é um componente de hardware (chip) que oferece funções criptográficas seguras em nível de firmware, incluindo:

- Armazenamento seguro de chaves criptográficas
- Medições de integridade do sistema (boot seguro)
- Geração de números aleatórios
- Suporte a BitLocker, LUKS e outras soluções de criptografia de disco

Em máquinas virtuais, o TPM geralmente não está presente ou é emulado — por isso a mensagem `No TPM chip found` é esperada no ambiente do laboratório.

### USB como vetor de tokens de hardware

Portas USB são o meio de conexão de **hardware tokens** (como YubiKey, SafeNet eToken) usados como segundo fator físico de autenticação. A ausência de dispositivos USB em ambientes virtuais é esperada — as VMs não têm acesso direto ao barramento USB do host.

### Ferramentas utilizadas

| Ferramenta      | Função                                                         |
|-----------------|----------------------------------------------------------------|
| `journalctl`    | Consulta ao journal do systemd — logs do kernel e serviços    |
| `--grep`        | Filtra entradas do journal por palavra-chave                   |
| `-k`            | Exibe apenas mensagens do kernel                               |
| `usbview`       | Interface gráfica para visualização de dispositivos USB        |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Verificar a presença do módulo TPM nos logs do kernel:
```bash
journalctl -k --grep=tpm
```
A saída exibirá a mensagem:
```
kernel: ima: No TPM chip found, activating TPM-bypass!
```
Indicando que o IMA (Integrity Measurement Architecture) não encontrou chip TPM e ativou o modo bypass. Encerrar com `Ctrl+C`.

**4.** *(Print solicitado)* Sair do modo root e abrir o visualizador de USB:
```bash
exit
usbview
```
O programa **USB Viewer** abrirá sem exibir dispositivos — confirmando que em ambiente virtual não há acesso direto ao barramento USB do host.

**5.** Fechar o programa USB Viewer e o Terminal.

## Aprendizado

- A ausência de TPM em VMs é esperada: chips TPM são componentes físicos da placa-mãe. Alguns hipervisores oferecem TPM virtual (vTPM), mas o ambiente do laboratório não o implementa.
- O IMA (Integrity Measurement Architecture) usa o TPM para armazenar hashes de arquivos críticos do sistema, detectando adulterações. Sem TPM, o IMA opera em modo bypass.
- A mensagem `No TPM chip found` em um sistema físico seria um indicador de problema de hardware ou BIOS — em servidores de produção, o TPM é fundamental para boot seguro e criptografia de disco.
- Portas USB em sistemas de produção devem ser gerenciadas por políticas de controle de dispositivos (DLP) para evitar uso de pendrives não autorizados ou instalação de keyloggers físicos.
- Para testar o USB Viewer com dispositivos reais, recomenda-se executar em um computador físico pessoal.
