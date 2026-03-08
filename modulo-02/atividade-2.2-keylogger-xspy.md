# Atividade 2.2 – Keylogger XSPY no Kali Linux

## Objetivo
Demonstrar o funcionamento de um keylogger utilizando o XSPY no Kali Linux.

## ⚠️ Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente de laboratório controlado, com fins educacionais. O uso de keyloggers contra pessoas sem autorização é **crime**.

## Conceito

### Keylogger
Um **keylogger** é um software que registra todas as teclas pressionadas pelo usuário, sem seu conhecimento. As informações capturadas podem incluir senhas, mensagens, dados bancários e qualquer outro texto digitado.

### XSPY
O **XSPY** é um keylogger para sistemas Linux que monitora eventos de teclado no servidor X11 (sistema gráfico do Linux).

---

## Passo a Passo

### 1. Acessar a pasta Documentos
```bash
cd /home/aluno/Documentos
ls  # pasta vazia
```

### 2. Configurar o teclado na VM
```
Settings → Keyboard → Send Alt codes → Save
```

### 3. Iniciar o keylogger
```bash
xspy >> teste.log
```
O processo fica em execução em segundo plano, capturando tudo que é digitado.

### 4. Digitar o texto alvo
Abrir o Editor de Texto (Mousepad) e **digitar** (não colar):
```
Hacker do bem!
```

### 5. Encerrar o keylogger
```
Ctrl + C
```

### 6. Verificar o arquivo capturado
```bash
ls
# teste.log

cat teste.log
```

**Saída:**
```
opened :10.0 for snoopng

Shift_L hacker do bemShift_L 1Control_L c
```

> **Nota:** Por limitações da VM via RDP, o texto pode aparecer ligeiramente diferente. Em execução local, a captura é fiel ao texto digitado.

---

## Como Funciona Tecnicamente

O XSPY monitora eventos do servidor **X11** (sistema de janelas do Linux), interceptando as teclas pressionadas antes mesmo de chegarem à aplicação de destino.

```
Usuário digita → Evento X11 → XSPY intercepta → Salva em arquivo
                                    ↓
                          Aplicação recebe normalmente
```

---

## Tipos de Keyloggers

| Tipo | Descrição |
|------|-----------|
| **Software** | Programa instalado no SO (como o XSPY) |
| **Hardware** | Dispositivo físico entre teclado e computador |
| **Kernel** | Opera no nível do kernel do SO |
| **Hipervisor** | Intercepta antes do SO |

---

## Como se Proteger

- Usar autenticação de dois fatores (2FA)
- Verificar processos em execução com `ps aux`
- Usar teclados virtuais para senhas sensíveis
- Manter antivírus/EDR atualizado
- Auditar processos com acesso ao servidor X11

---

## Aprendizado
- Keyloggers capturam todo texto digitado, incluindo senhas
- O XSPY funciona no nível do servidor gráfico X11
- Detecção é difícil pois o processo parece legítimo
- 2FA protege mesmo que a senha seja capturada
