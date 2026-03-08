# Atividade 2.10 – Políticas de Segurança no Windows Server 2022

## Objetivo
Explorar a Política de Segurança Local (secpol) no Windows Server 2022 como controle gerencial de segurança.

## Conceito

### Controle Gerencial
Um **controle gerencial** define as regras e políticas que governam o comportamento de segurança de um sistema. No Windows, as políticas locais de segurança definem requisitos de senha, bloqueio de contas, auditoria e privilégios.

### Local Security Policy (secpol.msc)
A **Política de Segurança Local** é uma ferramenta gráfica do Windows que permite configurar políticas de segurança sem a necessidade de um domínio Active Directory.

---

## Ambiente
- **Sistema:** Windows Server 2022
- **IP:** `[CONFIDENCIAL]`
- **Usuário:** `[CONFIDENCIAL]`

---

## Acessar a Ferramenta
```
Barra de tarefas → "Type here to search" → secpol → Local Security Policy
```

---

## Account Policies → Password Policy

| Política | Descrição |
|----------|-----------|
| **Enforce password history** | Número de senhas anteriores que não podem ser reutilizadas |
| **Maximum password age** | Tempo máximo antes de exigir troca de senha |
| **Minimum password age** | Tempo mínimo antes de permitir troca de senha |
| **Minimum password length** | Número mínimo de caracteres da senha |
| **Password must meet complexity requirements** | Exige maiúsculas, minúsculas, números e caracteres especiais |

---

## Account Policies → Account Lockout Policy

| Política | Descrição |
|----------|-----------|
| **Account lockout duration** | Tempo que a conta permanece bloqueada após exceder tentativas |
| **Account lockout threshold** | Número de tentativas falhas antes de bloquear a conta |
| **Reset account lockout counter after** | Tempo para resetar o contador de tentativas falhas |

---

## Local Policies → Audit Policy

| Política | O que audita |
|----------|-------------|
| **Audit account logon events** | Tentativas de logon em conta |
| **Audit account management** | Criação, modificação e exclusão de contas |
| **Audit directory service access** | Acesso a objetos do Active Directory |
| **Audit logon events** | Tentativas de logon no sistema |
| **Audit object access** | Acesso a arquivos, pastas e registros |
| **Audit policy change** | Alterações nas políticas de auditoria |
| **Audit privilege use** | Uso de privilégios elevados |
| **Audit process tracking** | Criação e término de processos |
| **Audit system events** | Reinicializações e desligamentos |

---

## Local Policies → Security Options (principais)

| Política | Descrição |
|----------|-----------|
| **Administrator account status** | Ativa/desativa a conta de administrador built-in |
| **Guest account status** | Ativa/desativa a conta de convidado |
| **Interactive logon: Do not display last user name** | Não mostra o último usuário logado |
| **Interactive logon: Do not require CTRL+ALT+DEL** | Permite/exige o atalho de segurança |
| **Domain member: Digitally encrypt secure channel data** | Criptografia no canal seguro do domínio |

---

## Boas Práticas de Política de Senhas

| Parâmetro | Valor Recomendado |
|-----------|------------------|
| Histórico de senhas | 24 senhas anteriores |
| Idade máxima | 60-90 dias |
| Comprimento mínimo | 12 caracteres |
| Complexidade | Habilitada |
| Bloqueio após tentativas | 5 tentativas |
| Duração do bloqueio | 30 minutos |

---

## Aprendizado
- Políticas de senha bem configuradas dificultam ataques de força bruta
- O bloqueio de conta protege contra ataques de tentativa e erro
- A auditoria registra eventos para análise posterior de incidentes
- Security Options controlam comportamentos críticos de segurança do sistema
- Em ambientes corporativos, estas políticas são gerenciadas via Group Policy (GPO)
