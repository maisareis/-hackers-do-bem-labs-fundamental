# Atividade 2.4 – Múltiplos Payloads de Malware com Brutal

## Objetivo
Explorar o repositório Brutal, que contém múltiplos payloads maliciosos educacionais para entender como ataques cibernéticos são estruturados.

## ⚠️ Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente de laboratório controlado, com fins educacionais. **NUNCA execute os payloads criados em máquinas sem autorização.**

## Conceito

### Payload
Um **payload** é o componente de um malware responsável pela ação maliciosa em si (roubo de dados, destruição, controle remoto, etc.). É a "carga" do ataque.

### Hoax
Um **hoax** é um software de brincadeira/pegadinha que não causa dano permanente, mas perturba o uso normal do computador (rotacionar tela, abrir pop-ups, etc.).

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Acessar a pasta Brutal
```bash
cd /curso/Brutal
ls
# Brutal.sh  LICENSE  output  PaensyLib  README.md  src  temp  tools
```

### 3. Executar o Brutal
```bash
chmod +x Brutal.sh
./Brutal.sh
```

### 4. Explorar as opções disponíveis
```
[01]  Meterpreter Reverse TCP Injection using Powershell
[02]  Download and Execute Backdoor
[03]  Get Credential information With Mimikatz [Send to gmail]
[04]  Retrieve lots of passwords stored on a local computer [gmail]
[05]  Payload Prank for attack computer [Fun with Windows]
[06]  Payload to Manage Windows [add user, rdp enable, telnet]
[07]  Attacking Windows [At your Own Risk]
[08]  Help and Tutorials
[09]  Credits
[10]  Exit
```

### 5. Acessar os payloads de brincadeira (Prank)
```
Screetsec@Brutal: >> 5
```

**Opções de Prank:**
```
[01]  Simple Payload Hellow World
[02]  Don't Fuck It Up
[03]  I Will Learn to Lock My Computer
[04]  Write a message to notepad
[05]  Screen Rotation Prank
[06]  Auto Shutdown prank
[07]  Play youtube Rick Roll
[08]  Auto Facebook Post
[09]  Crashing Windows with Fork Bomb
[10]  Back
```

### 6. Criar payload de rotação de tela
```
Screetsec@Prank: >> 5

 Succes Create Payload
 Now Copy the generated output/Screen-Rotation-Pranks.ino to your HID
```

### 7. Verificar arquivos gerados
Navegar para `/curso/Brutal/src/prank/` no gerenciador de arquivos e visualizar os arquivos `.ino`.

---

## Tipos de Payloads no Brutal

| Categoria | Descrição | Risco |
|-----------|-----------|-------|
| Meterpreter | Acesso remoto completo | ⚠️ Alto |
| Backdoor | Acesso persistente | ⚠️ Alto |
| Mimikatz | Roubo de credenciais | ⚠️ Alto |
| Password Dump | Extração de senhas | ⚠️ Alto |
| Prank/Hoax | Brincadeiras | ℹ️ Baixo |

---

## Arquivos .ino e HID Attacks
Os arquivos `.ino` são scripts para dispositivos **Arduino** configurados como **HID (Human Interface Device)**. Um pendrive Arduino pode simular um teclado e executar automaticamente comandos no computador da vítima ao ser conectado.

---

## Aprendizado
- Repositórios de malware educacional demonstram a facilidade de obter ferramentas maliciosas
- Payloads variam de simples brincadeiras a ataques com acesso total ao sistema
- Ataques HID via Arduino são difíceis de detectar pois o dispositivo parece um teclado legítimo
- Políticas de dispositivos USB podem mitigar ataques HID
