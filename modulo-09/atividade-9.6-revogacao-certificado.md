# Atividade 9.6 – Revogando um Certificado no Windows Server 2022

## Objetivo
Revogar um certificado digital emitido pela CA no Windows Server 2022, utilizando o motivo "Certificate Hold" para suspensão temporária.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

A **revogação de certificado** invalida um certificado antes do seu vencimento. Isso é necessário quando a chave privada é comprometida, o titular muda de função, ou o certificado foi emitido com dados incorretos.

### Motivos de revogação (Reason Codes)

| Código | Motivo | Descrição |
|--------|--------|-----------|
| `Unspecified` | Não especificado | Motivo genérico |
| `Key Compromise` | Chave comprometida | Chave privada foi exposta |
| `CA Compromise` | CA comprometida | A CA foi comprometida |
| `Affiliation Changed` | Afiliação mudou | Titular mudou de organização |
| `Superseded` | Substituído | Novo certificado emitido |
| `Cease of Operation` | Encerramento | Serviço encerrado |
| **`Certificate Hold`** | Suspensão temporária | Revogação reversível |

> O **Certificate Hold** é o único motivo reversível — o certificado pode ser reativado sem ser reemitido (ver Atividade 9.7).

---

## Pré-requisitos
- Atividades 9.1 e 9.2 concluídas
- Acesso ao Windows Server 2022 (servidor) como Administrator

---

## Passo a Passo

### 1. Acessar o Windows Server 2022 (servidor)
Conectar via RDP ao servidor com credenciais de Administrator.

### 2. Abrir o Server Manager
Pesquisar por `Server Manager` na barra de tarefas e abrir.

### 3. Abrir o console da Certification Authority
Clicar em **Tools** → **Certification Authority**.

### 4. Visualizar os certificados emitidos
Na coluna esquerda, expandir `aluno-DC01-CA` → clicar em **Issued Certificates**.

### 5. Revogar o certificado
Na coluna direita, clicar com o botão direito no certificado → **All Tasks** → **Revoke Certificate**.

### 6. Selecionar o motivo e confirmar a revogação
Na janela aberta, selecionar **Certificate Hold** como **Reason code** → **Yes**.

O certificado está agora revogado. Verificar que a pasta **Issued Certificates** está vazia.

---

## O que acontece após a revogação?

```
Certificado revogado
    └── Movido para: Revoked Certificates
    └── Adicionado à CRL (Certificate Revocation List)
    └── Clientes que verificam a CRL ou OCSP saberão que o certificado é inválido
```

---

## Aprendizado
- A revogação invalida o certificado imediatamente, antes do seu vencimento natural
- O motivo **Certificate Hold** é reversível — útil para suspensões temporárias
- Após a revogação, o certificado é movido para a pasta **Revoked Certificates** no `certsrv`
- A CRL (Certificate Revocation List) é a lista distribuída aos clientes com os certificados revogados
