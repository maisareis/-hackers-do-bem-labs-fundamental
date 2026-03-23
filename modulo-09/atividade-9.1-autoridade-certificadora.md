# Atividade 9.1 – Criando uma Autoridade Certificadora no Windows Server 2022

## Objetivo
Implementar uma Autoridade Certificadora (CA) no Windows Server 2022 usando o Active Directory Certificate Services (AD CS), tornando o servidor apto a emitir e gerenciar certificados digitais na rede.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

Uma **Autoridade Certificadora (CA)** é uma entidade confiável responsável por emitir, assinar e revogar certificados digitais. Ela é a base da PKI (Public Key Infrastructure).

| Tipo de CA | Descrição |
|-----------|-----------|
| **Root CA** | CA raiz, topo da hierarquia de confiança |
| **Enterprise CA** | Integrada ao Active Directory, emite certificados automaticamente para membros do domínio |
| **Standalone CA** | Independente do AD, requer aprovação manual das solicitações |

> ⚠️ **Pré-requisito:** As Atividades 5.1 a 5.5 (Active Directory) devem estar concluídas. Sem o AD DS configurado, a opção "Enterprise CA" não estará disponível.

---

## Pré-requisitos
- Windows Server 2022 com Active Directory configurado (Atividades 5.1–5.5)
- Acesso como Administrator

---

## Passo a Passo

### 1. Acessar o Server Manager
Abrir o Server Manager via barra de pesquisa do Windows.

### 2. Iniciar a instalação de roles
Clicar em **Manage** → **Add Roles and Features** → **Next** 3 vezes.

### 3. Selecionar o Active Directory Certificate Services
Em **Server Roles**, marcar **Active Directory Certificate Services** → **Add Features** → **Next** 3 vezes.

### 4. Confirmar a instalação da Certification Authority
Em **Role Services**, confirmar que **Certification Authority** está marcada → **Next** → **Install**.

Aguardar a conclusão da instalação com a mensagem `Installation started on DC01.aluno.hacker.com`.

### 5. Configurar o AD CS
Ao finalizar, clicar em **Configure Active Directory Certificate Services on the Destination server**.

### 6. Definir credenciais e tipo de CA
Na janela **AD CS Configuration** → **Next** → marcar **Certification Authority** → **Next**.

### 7. Selecionar Enterprise CA
Verificar que **Enterprise CA** está selecionada → **Next**.

### 8. Selecionar Root CA
Verificar que **Root CA** está selecionada → **Next**.

### 9. Criar nova chave privada
Verificar que **Create a new private key** está selecionada → **Next** → **Next** (aceitar RSA 2048 bits e SHA256).

### 10. Confirmar nome comum da CA
Aceitar o nome padrão `aluno-DC01-CA` → **Next**.

### 11. Aumentar validade dos certificados
Alterar a validade de **5 para 10 anos** → **Next**.

### 12. Confirmar localização do banco de dados
Aceitar o local padrão `C:\Windows\system32\CertLog` → **Next**.

### 13. Revisar e configurar
Ler o resumo → **Configure** → aguardar a mensagem `Configuration succeeded` → **Close** 2 vezes.

### 14. Abrir o console da Certification Authority
Clicar no menu **Windows** → expandir **Windows Administrative Tools** → clicar em **Certification Authority**.

### 15. Verificar a CA criada
Confirmar a presença de **Certification Authority (Local)** → **aluno-DC01-CA**.

### 16. Confirmar o funcionamento da CA
Verificar que a CA `aluno-DC01-CA` está presente e ativa no console. Não fechar a janela — será utilizada na próxima atividade.

---

## Estrutura da PKI criada

```
Root CA
└── aluno-DC01-CA (Enterprise CA)
    └── Emite certificados para membros do domínio aluno.hacker.com
```

---

## Explicação dos Parâmetros Principais

| Parâmetro | Valor escolhido | Motivo |
|-----------|----------------|--------|
| Tipo de CA | Enterprise CA | Integração automática com o AD |
| Hierarquia | Root CA | Primeira CA da hierarquia |
| Algoritmo | RSA 2048 bits | Padrão de segurança atual |
| Hash | SHA256 | Padrão recomendado |
| Validade | 10 anos | Maior que o padrão (5 anos) para o lab |

---

## Aprendizado
- A Enterprise CA exige o Active Directory para funcionar — é por isso que as Atividades 5.1–5.5 são pré-requisito
- A Root CA é o ponto de confiança máximo na hierarquia PKI; seu certificado é autoassinado
- O nome comum `aluno-DC01-CA` identifica a CA na rede e nos certificados emitidos
- A validade da CA deve ser maior que a validade dos certificados que ela emitirá
