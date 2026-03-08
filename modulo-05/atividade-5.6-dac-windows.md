# Atividade 5.6 – Implementando o Controle de Acesso Discricionário (DAC) no Windows Server 2022 (cliente)

## Objetivo

Demonstrar o funcionamento do **Controle de Acesso Discricionário (DAC)** no Windows Server 2022 manipulando permissões de pastas e verificando os efeitos do bloqueio e restauração de acesso.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

O **DAC (Discretionary Access Control)** é o modelo de controle de acesso padrão do Windows, no qual o proprietário de um recurso decide quem pode acessá-lo e com quais permissões. As permissões são armazenadas em **ACLs (Access Control Lists)** associadas a cada arquivo ou pasta.

### Permissões NTFS básicas

| Permissão      | Descrição                                                         |
|----------------|-------------------------------------------------------------------|
| Full Control   | Leitura, gravação, execução, alteração de permissões e exclusão  |
| Modify         | Leitura, gravação, execução e exclusão (sem alterar permissões)  |
| Read & Execute | Leitura e execução de arquivos                                    |
| Read           | Apenas leitura do conteúdo                                        |
| Write          | Criação e modificação de arquivos                                 |

### Allow vs. Deny

| Tipo    | Comportamento                                                           |
|---------|-------------------------------------------------------------------------|
| Allow   | Concede a permissão ao usuário ou grupo                                 |
| Deny    | Nega explicitamente — tem precedência sobre qualquer Allow concedido    |

> ⚠️ Uma entrada **Deny** sobrepõe entradas **Allow** — mesmo que o usuário herde permissão via grupo, o Deny individual prevalece.

### Por que o Administrador não pode ser removido?

O Windows impede a remoção do Administrador das ACLs para evitar situações de bloqueio total irrecuperável. Isso é uma proteção do sistema.

## Passo a Passo

**1.** Conectar ao Windows Server 2022 (cliente) via RDP ao IP `192.168.98.30` com usuário `ALUNO\nome1` e senha conforme o ambiente de laboratório.

**2.** Clicar em "Type here to search" e escrever `Notepad`.

**3.** Clicar no botão **Notepad** para abrir o Bloco de notas.

**4.** Escrever `Teste` e salvar o arquivo como `Arquivo1.txt` na pasta **Documents**. Fechar o Bloco de notas.

**5.** Na barra de tarefas, clicar em **File Explorer**.

**6.** No campo esquerdo, clicar em **Documents** — confirmar que `Arquivo1` aparece no campo direito.

**7.** Clicar com o botão direito na pasta **Documents** → **Properties**.

**8.** Na janela aberta, clicar na aba **Security**.

**9.** Observar os 3 grupos/usuários listados: `SYSTEM`, `Nome1...` e `Administrators...`.

**10.** Clicar em **Edit...** → selecionar **Administrators** → confirmar que todas as permissões estão marcadas em **Allow** (Full Control).

**11.** Clicar em **Remove** — observar a mensagem negando a remoção do Administrador → **OK**.

**12.** Clicar em **Nome1** → ativar **Deny** para **Full Control** → **Apply** → **Yes** → **Continue** em todos os avisos até as janelas serem fechadas.

**13.** Voltar ao **File Explorer** — confirmar que o arquivo `Arquivo1` desapareceu e o acesso à pasta **Documents** foi negado.

**14.** No **File Explorer**, clicar em outra pasta (ex.: **Downloads**) e depois em **Documents**. Na mensagem de negação, clicar em **Continue** → inserir credenciais de `Administrator` e senha conforme o ambiente. Na mensagem "You have been denied permission to access this folder", clicar **Close**.

**15.** Clicar com o botão direito na pasta **Documents** → **Properties**.

**16.** Clicar na aba **Security** → **Edit...** → selecionar **Nome1**.

**17.** Desmarcar todas as caixas **Deny** nas permissões → **Apply** → **Continue** até o aviso desaparecer.

**18.** *(Print solicitado)* Clicar em outra pasta (ex.: **Downloads**) e depois em **Documents** — confirmar que o acesso foi restaurado e a pasta `Documents` está acessível novamente.

**19.** Fechar as janelas e encerrar a conexão RDP.

## Aprendizado

- O DAC permite que o proprietário do recurso gerencie as permissões de forma granular, incluindo a capacidade de negar acesso a si mesmo — demonstrando o princípio do "controle discricionário".
- Uma entrada **Deny** tem precedência absoluta sobre qualquer **Allow**, independentemente de grupos ou herança — por isso deve ser usada com cautela.
- O Windows protege a conta Administrador de ser removida das ACLs para garantir que sempre exista um caminho de recuperação de acesso.
- Em ambientes corporativos, o gerenciamento de permissões NTFS deve ser feito preferencialmente por grupos AD, não por usuários individuais — facilitando a manutenção e auditoria.
