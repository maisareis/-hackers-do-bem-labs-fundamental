# Atividade 9.2 – Emitindo Certificados Digitais com Autoridade Certificadora no Windows Server 2022

## Objetivo
Emitir certificados digitais para os clientes da rede utilizando a Autoridade Certificadora configurada no Windows Server 2022, através da criação de um template personalizado e de uma GPO de auto-inscrição.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

Um **Certificate Template** define as propriedades dos certificados emitidos pela CA, como algoritmo, validade, propósito e quem pode receber o certificado.

A **Auto-Enrollment (auto-inscrição)** permite que computadores e usuários do domínio recebam certificados automaticamente, sem intervenção manual, através de uma GPO.

O comando **gpupdate /force** força a aplicação imediata das políticas de grupo, sem aguardar o ciclo normal de atualização.

---

## Pré-requisitos
- Atividade 9.1 concluída (CA configurada)
- Console `certsrv` aberto

---

## Passo a Passo

### 1. Abrir o console de Certificate Templates
Na janela `certsrv – [Certification Authority (Local)]`, expandir `aluno-DC01-CA` → clicar com o botão direito em **Certificate Templates** → **Manage**.

### 2. Duplicar o template Workstation Authentication
Na janela **Certificate Template Console**, localizar **Workstation Authentication** → botão direito → **Duplicate Template**.

### 3. Configurar o nome do novo template
Na aba **General**, alterar o **Template display name** para `ALUNO COMPUTADOR`.

### 4. Configurar Subject Name
Na aba **Subject Name**, habilitar a caixa **User principal name (UPN)**.

### 5. Configurar permissões de segurança
Na aba **Security**, selecionar **Domain Computers (ALUNO\Domain Computers)** → ativar **Allow** para **Read** e **Autoenroll** → **Apply** → **OK**. Fechar a janela.

### 6. Emitir o novo template
De volta em `certsrv`, clicar com o botão direito em **Certificate Templates** → **New** → **Certificate Template to Issue**.

### 7. Selecionar o template criado
Na janela **Enable Certificate Templates**, selecionar **ALUNO COMPUTADOR** → **OK**.

### 8. Confirmar o template na CA
Clicar na pasta **Certificate Templates** e verificar que `ALUNO COMPUTADOR` aparece na coluna direita.

### 9. Abrir o Group Policy Management
No **Server Manager**, clicar em **Tools** → **Group Policy Management**.

### 10. Criar nova GPO
Expandir **Forest: aluno.hacker.com** → **Domains** → **aluno.hacker.com** → **Group Policy Objects** → botão direito → **New**.

Inserir o nome:
```
COMPUTADOR: Politica de Inscricao de Certificado
```
Clicar em **OK**.

### 11. Editar a GPO criada
Clicar com o botão esquerdo na nova GPO → **Edit**.

### 12. Configurar Auto-Enrollment
Na janela **Group Policy Management Editor**, expandir **Computer Configuration** → **Policies** → **Windows Settings** → **Security Settings** → clicar em **Public Key Policies** → clicar 2 vezes em **Certificate Services Client – Auto-Enrollment**.

### 13. Habilitar o Auto-Enrollment
Mudar o **Configuration Model** de `Not configured` para `Enabled`. Habilitar as caixas **Renew expired...** e **Update certificates...** → **Apply** → **OK**.

### 14. Configurar Automatic Certificate Request
Na coluna direita, clicar com botão direito em **Automatic Certificate Request Settings** → **New** → **Automatic Certificate Request...** → **Next** 2 vezes → **Finish**.

### 15. Abrir o Command Prompt e forçar atualização da GPO
Na barra de tarefas, abrir o **Command Prompt** e executar:
```cmd
gpupdate /force
```
Resultado esperado:
```
Updating policy...

Computer Policy update has completed successfully.
User Policy update has completed successfully.
```

### 16. Verificar emissão do certificado e fechar CMD
Fechar o CMD. Voltar à janela `certsrv`, clicar na pasta **Issued Certificates** e verificar que um certificado foi emitido para o cliente Windows Server 2022.

### 17. Encerrar a sessão RDP
Fechar a conexão RDP com o Windows Server (servidor).

---

## Fluxo de emissão automática de certificados

```
CA (aluno-DC01-CA)
    └── Template: ALUNO COMPUTADOR
            └── GPO: Politica de Inscricao de Certificado
                    └── gpupdate /force
                            └── Certificado emitido para o cliente
```

---

## Aprendizado
- Certificate Templates definem as características e permissões dos certificados emitidos
- A permissão **Autoenroll** permite que computadores do domínio recebam certificados automaticamente
- O `gpupdate /force` é essencial para aplicar políticas de grupo imediatamente sem reinicializar
- A pasta **Issued Certificates** no `certsrv` confirma que os certificados foram emitidos com sucesso
