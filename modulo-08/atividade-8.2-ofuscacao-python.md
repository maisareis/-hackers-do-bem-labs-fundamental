# Atividade 8.2 – Ofuscação de Código Python no Kali Linux

## Objetivo
Aplicar ofuscação em um código Python utilizando bytecode, tornando o código-fonte incompreensível para leitura humana sem perder a funcionalidade original.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais. Técnicas de ofuscação podem ser usadas de forma maliciosa para esconder código malicioso; o uso responsável é obrigação do profissional de segurança.

## Conceito

A **ofuscação de código** é uma técnica que transforma o código-fonte em uma versão difícil de compreender, mantendo o comportamento funcional original. É usada tanto para proteção de propriedade intelectual quanto, de forma maliciosa, para dificultar a análise de malware.

O **bytecode Python** é a representação compilada de um script Python. Ao compilar com otimização (`-OO`), o Python gera um arquivo `.pyc` que remove docstrings e assertivas, resultando em código binário ilegível para humanos.

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- Python 3 instalado

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Acessar a pasta e criar o arquivo Python
```bash
cd /home/aluno/Documentos/Arquivos
nano arquivo.py
```
Conteúdo inserido:
```python
def somar(a, b):
    return a + b

def subtrair(a, b):
    return a - b

resultado = somar(5, 3)
print(resultado)
```
Salvar com `Ctrl+X`, `s`, `Enter`.

### 3. Testar o código original
```bash
python arquivo.py
```
Resultado esperado:
```
8
```

### 4. Gerar o bytecode ofuscado e listar a pasta
```bash
python -OO -m py_compile arquivo.py
ls
```
Resultado esperado:
```
arquivo.py  __pycache__
```

### Parâmetros do comando

| Parâmetro | Descrição |
|-----------|-----------|
| `-OO` | Otimização máxima: remove docstrings e assertivas |
| `-m py_compile` | Compila o arquivo para bytecode |

### 5. Acessar a pasta do bytecode e listar o conteúdo
```bash
cd __pycache__
ls
```
Resultado esperado:
```
arquivo.cpython-313.opt-2.pyc
```
> O nome exato do arquivo `.pyc` pode variar conforme a versão do Python instalada.

### 6. Renomear o arquivo para extensão .py e confirmar
```bash
mv arquivo.cpython-313.opt-2.pyc arquivo_ofuscado.py
ls
```
Resultado esperado:
```
arquivo_ofuscado.py
```

### 7. Visualizar o conteúdo ofuscado
```bash
cat arquivo_ofuscado.py
```
O conteúdo exibido será ilegível — sequências de bytes binários sem sentido para leitura humana.

### 8. Executar o código ofuscado
```bash
python arquivo_ofuscado.py
```
Resultado esperado:
```
8
```
O código mantém a funcionalidade original mesmo estando ofuscado.

### 9. Limpeza da pasta de trabalho
```bash
cd ..
rm -r *
ls
```

---

## Explicação dos Comandos Principais

| Comando | Descrição |
|---------|-----------|
| `python -OO -m py_compile <arquivo>` | Compila o script para bytecode otimizado |
| `mv <origem> <destino>` | Renomeia ou move arquivos |
| `cat <arquivo>` | Exibe o conteúdo de um arquivo |

---

## Aprendizado
- O bytecode Python mantém a funcionalidade do código original, mas torna o código-fonte ilegível
- A ofuscação é uma técnica utilizada tanto para proteção de software quanto para ocultação de malware
- A flag `-OO` remove docstrings e assertivas, gerando um bytecode mais compacto
- Ferramentas de decompilação Python (como `uncompyle6`) podem reverter a ofuscação em alguns casos
