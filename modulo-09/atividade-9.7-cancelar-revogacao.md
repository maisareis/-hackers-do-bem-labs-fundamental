# Atividade 9.7 – Cancelando a Revogação de um Certificado no Windows Server 2022

## Objetivo
Cancelar a revogação de um certificado previamente suspenso com o motivo "Certificate Hold", restaurando sua validade no Windows Server 2022.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

O **Unrevoke** (cancelamento de revogação) só é possível quando o certificado foi revogado com o motivo **Certificate Hold**. Este é o único motivo de revogação reversível no padrão X.509.

Após o cancelamento, o certificado volta para a pasta **Issued Certificates** e é removido da CRL, tornando-se válido novamente para os clientes da rede.

---

## Pré-requisitos
- Atividade 9.6 concluída (certificado revogado com Certificate Hold)
- Acesso ao Windows Server 2022 (servidor) como Administrator

---

## Passo a Passo

### 1. Abrir o Server Manager
Pesquisar por `Server Manager` na barra de tarefas e abrir.

### 2. Abrir o console da Certification Authority
Clicar em **Tools** → **Certification Authority**.

### 3. Visualizar os certificados revogados
Na coluna esquerda, expandir `aluno-DC01-CA` → clicar em **Revoked Certificates**.

### 4. Inspecionar o certificado revogado
Na coluna direita, clicar 2 vezes no certificado revogado e explorar as abas:

| Aba | Conteúdo |
|-----|---------|
| **General** | Informações gerais do certificado |
| **Details** | Campos técnicos do certificado X.509 |
| **Path** | Cadeia de certificação |

Clicar em **OK**.

### 5. Ver atributos e extensões
Clicar com o botão direito no certificado → **All Tasks** → **View Attributes/Extensions...** → explorar as abas **Attributes** e **Extensions** → **OK**.

### 6. Cancelar a revogação
Clicar com o botão direito no certificado → **All Tasks** → **Unrevoke Certificate**.

### 7. Confirmar que o certificado saiu de Revoked Certificates
Verificar que o certificado desapareceu da pasta **Revoked Certificates**.

### 8. Verificar em Issued Certificates
Clicar na pasta **Issued Certificates** na coluna esquerda.

### 9. Confirmar a restauração do certificado
Verificar que o certificado aparece novamente em **Issued Certificates**, confirmando que está válido e ativo.

---

## Comparativo: Revogação vs Unrevoke

| | Revogação (Certificate Hold) | Unrevoke |
|---|---|---|
| **Ação** | Suspende o certificado | Restaura o certificado |
| **CRL** | Certificado adicionado | Certificado removido |
| **Pasta** | Moved to Revoked Certificates | Volta para Issued Certificates |
| **Reversível** | ✅ Sim (somente Certificate Hold) | N/A |

---

## Aprendizado
- Somente certificados revogados com **Certificate Hold** podem ter a revogação cancelada
- O Unrevoke remove o certificado da CRL, restaurando sua validade para os clientes
- Certificados revogados por outros motivos (Key Compromise, Superseded, etc.) não podem ser reativados
- A inspeção das abas **Attributes** e **Extensions** revela metadados adicionais do ciclo de vida do certificado
