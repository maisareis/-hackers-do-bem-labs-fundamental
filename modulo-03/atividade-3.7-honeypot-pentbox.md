# Atividade 3.7 – Criando um Honeypot com PentBox no Kali Linux e testando no Windows Server 2022

## Objetivo

Configurar um **Honeypot** na porta 80 utilizando o software **PentBox** no Kali Linux e verificar o registro de tentativas de acesso originadas de um Windows Server 2022.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**. A implantação de honeypots em redes de produção sem autorização pode violar políticas corporativas e legislação vigente.

## Conceito

Um **Honeypot** é um sistema ou serviço simulado propositalmente vulnerável ou atrativo, projetado para atrair e registrar tentativas de acesso não autorizadas. É uma ferramenta de inteligência de ameaças amplamente usada por equipes de segurança (Blue Team) para detectar, monitorar e analisar comportamento de atacantes sem expor sistemas reais.

O **PentBox** é um conjunto de ferramentas de segurança escrito em Ruby, que inclui funcionalidades de criptografia, varredura de portas, fuzzing, honeypot e coleta de informações DNS e de geolocalização.

### Menu do PentBox relevante para esta atividade

```
2 - Network tools
  └── 3 - Honeypot
        ├── 1 - Fast Auto Configuration  ← usado nesta atividade
        └── 2 - Manual Configuration
```

### O que o Honeypot registra

Ao receber uma conexão, o PentBox exibe um alerta de intrusão com:

| Informação        | Descrição                                        |
|-------------------|--------------------------------------------------|
| IP de origem      | Endereço do host que tentou a conexão            |
| Porta de origem   | Porta efêmera usada pelo cliente                 |
| Data e hora       | Timestamp da tentativa                           |
| Cabeçalhos HTTP   | Método, Host, User-Agent, Accept-Encoding, etc.  |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Verificar o IP da máquina:
```bash
ifconfig
```

**4.** Acessar o diretório do PentBox e executá-lo:
```bash
cd /curso/pentbox/pentbox-1.8
./pentbox.rb
```

**5.** Selecionar **"2 - Network tools"**.

**6.** Selecionar **"3 - Honeypot"**.

**7.** Selecionar **"1 - Fast Auto Configuration"**.

O honeypot será ativado automaticamente na porta 80, exibindo:
```
HONEYPOT ACTIVATED ON PORT 80
```

**8.** Minimizar o Kali Linux e conectar ao **Windows Server 2022** via RDP com as credenciais do ambiente.

**9.** No Windows Server 2022, abrir o **Microsoft Edge** e acessar o IP do Kali Linux:
```
http://[IP_KALI]
```
O navegador exibirá uma mensagem de acesso negado com timestamp.

**10.** *(Print solicitado)* Voltar ao Kali Linux e observar no Terminal o alerta de intrusão registrado pelo PentBox, contendo IP de origem, porta, timestamp e cabeçalhos HTTP da requisição do Edge.

**11.** Fechar o Microsoft Edge, encerrar a conexão RDP com o Windows Server e fechar o Terminal no Kali Linux.

## Aprendizado

- O PentBox permite criar um honeypot funcional em segundos, sem configurações complexas, demonstrando como é simples monitorar tentativas de acesso em uma porta exposta.
- O honeypot registra automaticamente informações valiosas do atacante: IP, porta, horário e o User-Agent do navegador — que pode revelar o sistema operacional e o browser utilizado.
- Em ambientes reais, honeypots são posicionados em segmentos de rede onde nenhum acesso legítimo é esperado, tornando qualquer conexão recebida um indicador de comprometimento (IoC).
- A combinação de honeypot + SIEM permite correlacionar esses alertas com outros eventos de segurança para uma resposta a incidentes mais eficaz.
