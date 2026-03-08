# Atividade 5.8 – Implementando o Controle de Acesso Discricionário (DAC) no Kali Linux

## Objetivo

Implementar o **Controle de Acesso Discricionário (DAC)** no Kali Linux criando um arquivo e modificando suas permissões com o comando `chmod`.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

No Linux, o DAC é implementado por meio do sistema de permissões POSIX. Cada arquivo possui um conjunto de permissões dividido em três escopos:

```
-rwxr--r--
│├─┤├─┤├─┤
│ │  │  └── Outros (o)
│ │  └───── Grupo (g)
│ └──────── Proprietário (u)
└────────── Tipo (- arquivo, d diretório, l link)
```

### Permissões POSIX

| Símbolo | Octal | Significado          |
|---------|-------|----------------------|
| `r`     | 4     | Leitura (read)       |
| `w`     | 2     | Gravação (write)     |
| `x`     | 1     | Execução (execute)   |
| `-`     | 0     | Sem permissão        |

### Sintaxe do chmod

```bash
chmod [quem][operador][permissão] arquivo   # Modo simbólico
chmod [octal] arquivo                        # Modo numérico
```

| Quem | Significado       |
|------|-------------------|
| `u`  | Proprietário      |
| `g`  | Grupo             |
| `o`  | Outros            |
| `a`  | Todos (u+g+o)     |

| Operador | Significado               |
|----------|---------------------------|
| `+`      | Adiciona permissão        |
| `-`      | Remove permissão          |
| `=`      | Define permissão exata    |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP com as credenciais do ambiente de laboratório.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Navegar até a pasta Documentos e criar o arquivo `texto.txt`:
```bash
cd /home/aluno/Documentos/
nano texto.txt
```
Inserir o texto `Teste1`, salvar com `Ctrl+X` → `s` → `Enter`.

**4.** Listar os arquivos da pasta:
```bash
ls
```

**5.** Verificar as permissões atuais do arquivo:
```bash
ls -l
```
Saída: `-rw-r--r-- 1 root root 7 [data] texto.txt`

O arquivo pertence ao `root` e possui:
- Proprietário (`root`): leitura e gravação (`rw-`)
- Grupo (`root`): apenas leitura (`r--`)
- Outros: apenas leitura (`r--`)

**6.** Adicionar permissão de execução ao proprietário:
```bash
chmod u+rwx texto.txt
```
O parâmetro `u+rwx` concede leitura, gravação e execução (`rwx`) ao proprietário (`u`).

**7.** *(Print solicitado)* Verificar as novas permissões:
```bash
ls -l
```
Saída esperada: `-rwxr--r-- 1 root root 7 [data] texto.txt`

O proprietário agora tem permissão de execução adicionada (`rwx`), enquanto grupo e outros permanecem com apenas leitura (`r--`).

**8.** Fechar o Terminal. **Não apagar o arquivo `texto.txt`** — será usado na atividade 5.9.

## Aprendizado

- O DAC no Linux permite ao proprietário do arquivo controlar com precisão quem pode ler, gravar ou executar cada recurso — sem necessidade de intervenção de um administrador centralizado.
- A permissão de execução (`x`) em arquivos de texto não tem efeito prático, mas em scripts (`*.sh`) é o que permite sua execução direta como programa.
- O modo simbólico (`u+rwx`) é mais legível para modificações incrementais; o modo octal (`chmod 755`) é mais eficiente para definir o conjunto completo de permissões de uma vez.
- Arquivos criados como `root` pertencem ao `root` — para que o usuário `aluno` possa modificá-los, seria necessário usar `chown aluno texto.txt` para transferir a propriedade.
