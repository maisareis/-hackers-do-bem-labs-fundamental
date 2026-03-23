# Atividade 9.3 – Conhecendo o Certificado Digital no Windows Server 2022 (Cliente)

## Objetivo
Identificar e analisar o certificado digital emitido pela CA no Windows Server 2022 (cliente), compreendendo seus campos, propósitos e o caminho de certificação.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **certmgr** (Certificate Manager) é o console do Windows para gerenciar certificados digitais do usuário atual. Permite visualizar, importar, exportar e excluir certificados.

A **cadeia de certificação** (Certification Path) mostra a hierarquia de confiança: do certificado do usuário/dispositivo até a Root CA, passando pelas CAs intermediárias.

O **OCSP** (Online Certificate Status Protocol) é um protocolo usado para verificar em tempo real se um certificado foi revogado, como alternativa às CRLs (Certificate Revocation Lists).

---

## Pré-requisitos
- Atividades 9.1 e 9.2 concluídas
- Acesso ao Windows Server 2022 (cliente) com usuário do domínio

---

## Passo a Passo

### 1. Acessar o Windows Server 2022 (cliente)
Conectar via RDP ao servidor cliente com usuário do domínio `ALUNO\nome1`.

### 2. Abrir o Command Prompt e testar conectividade
Abrir o CMD e verificar conectividade com o servidor:
```cmd
ping [IP do servidor]
```
> ⚠️ Se o ping falhar, interromper o lab, aguardar 5 minutos, reinicializar e voltar ao passo 1.

### 3. Fechar o CMD
Fechar a janela do Command Prompt.

### 4. Abrir o Certificate Manager
Na barra de tarefas, pesquisar por `Manage user certificates` e abrir.

### 5. Localizar o certificado da CA
Na janela `certmgr – [Certificates – Current User]`, expandir **Trusted Root Certification Authorities** → clicar em **Certificates**.

> Se o certificado `aluno-DC01-CA` não aparecer: abrir o CMD, executar `gpupdate /force`, reiniciar o Windows Server e voltar ao passo 1.

### 6. Abrir o certificado da CA
Clicar 2 vezes no certificado `aluno-DC01-CA`.

### 7. Ler a aba General
Ler toda a descrição da aba **General** (propósito, emissor, validade).

### 8. Analisar a aba Details
Selecionar a aba **Details** e analisar os campos:

| Campo | Descrição |
|-------|-----------|
| **Version** | Versão do padrão X.509 do certificado |
| **Signature algorithm** | Algoritmo usado para assinar o certificado |
| **Signature hash algorithm** | Hash usado na assinatura |
| **Valid to** | Data de expiração do certificado |
| **Public Key** | Chave pública contida no certificado |
| **Key Usage** | Propósitos permitidos para o certificado |

### 9. Analisar a aba Certification Path
Selecionar a aba **Certification Path**, ler a hierarquia de confiança e clicar em **OK** para fechar.

### 10. Verificar os propósitos do certificado
Clicar com o botão direito no certificado `aluno-DC01-CA` → **Properties** → aba **General** → ler os propósitos listados em **Certificate purposes** → **Cancel**.

### 11. Observar a opção OCSP
Verificar que a aba **OCSP** está disponível para gerenciamento do ciclo de vida do certificado. Clicar em **Cancel**.

### 12. Fechar o Certificate Manager
Fechar a janela `certmgr`.

---

## Campos do Certificado X.509

| Campo | Significado |
|-------|-------------|
| **Issued To** | Entidade para quem o certificado foi emitido |
| **Issued By** | CA que emitiu e assinou o certificado |
| **Valid From / To** | Período de validade |
| **Public Key** | Chave pública do titular |
| **Key Usage** | Usos permitidos (assinatura, cifragem, etc.) |
| **Signature Algorithm** | Algoritmo usado pela CA para assinar |

---

## Aprendizado
- O `certmgr` permite visualizar todos os certificados do usuário atual no Windows
- A aba **Details** revela os campos técnicos do padrão X.509
- O **Certification Path** mostra a cadeia de confiança até a Root CA
- O OCSP permite verificar o status de revogação de um certificado em tempo real, sendo mais eficiente que as CRLs
