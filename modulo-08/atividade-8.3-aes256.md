# Atividade 8.3 – Cifrando e Decifrando um Arquivo com AES-256 no Kali Linux

## Objetivo
Implementar criptografia simétrica AES-256 no modo CTR em um arquivo de imagem, cifrando e decifrando com sucesso utilizando o OpenSSL no Kali Linux.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais. O uso de técnicas de criptografia deve respeitar a legislação vigente.

## Conceito

O **AES-256** (Advanced Encryption Standard com chave de 256 bits) é o algoritmo de criptografia simétrica mais robusto disponível atualmente. É amplamente utilizado por governos, bancos e empresas para proteger dados sensíveis.

O **modo CTR** (Counter Mode) transforma o AES em um algoritmo de cifra de fluxo, gerando um fluxo de chave a partir de um contador incrementado a cada bloco. Suas vantagens incluem paralelização do processo e ausência de propagação de erros.

Na criptografia simétrica, a **mesma chave** é usada tanto para cifrar quanto para decifrar os dados.

---

## Pré-requisitos
- Kali Linux com acesso via Terminal
- OpenSSL instalado
- Conexão com a internet para baixar a imagem de teste

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Acessar a pasta e baixar a imagem
```bash
cd /home/aluno/Documentos/Arquivos
wget https://upload.wikimedia.org/wikipedia/commons/0/01/My_shoe.jpg
```

### 3. Visualizar a imagem no gerenciador de arquivos
Abrir o Thunar em Aplicativos → Gerenciador de Arquivos, navegar até `/home/aluno/Documentos/Arquivos/`, abrir a imagem e fechar o visualizador.

### 4. Verificar a presença da imagem e cifrá-la
```bash
ls
openssl enc -aes-256-ctr -salt -in My_shoe.jpg -out arquivo_criptografado
```
Informar e confirmar senha quando solicitado. Neste lab foi usada a senha `abcd1234`.

### Parâmetros do comando de cifragem

| Parâmetro | Descrição |
|-----------|-----------|
| `enc` | Comando do OpenSSL para operações de criptografia |
| `-aes-256-ctr` | Algoritmo AES-256 no modo CTR |
| `-salt` | Adiciona valor aleatório para aumentar a segurança da chave |
| `-in` | Arquivo de entrada a ser cifrado |
| `-out` | Arquivo de saída cifrado |

### 5. Verificar o arquivo cifrado e apagar o original
```bash
ls
rm My_shoe.jpg
ls
```
Resultado após remoção:
```
arquivo_criptografado
```

### 6. Decifrar o arquivo e verificar o resultado
```bash
openssl enc -d -aes-256-ctr -in arquivo_criptografado -out My_shoe2.jpg
ls
```
Informar a mesma senha utilizada na cifragem (`abcd1234`).

Resultado esperado:
```
arquivo_criptografado  My_shoe2.jpg
```

### Parâmetro adicional para decifragem

| Parâmetro | Descrição |
|-----------|-----------|
| `-d` | Indica operação de descriptografia |

### 7. Verificar a imagem recuperada no Thunar
Voltar ao Thunar, abrir `My_shoe2.jpg` e confirmar que é idêntica à imagem original.

### 8. Limpeza da pasta de trabalho
```bash
rm -r *
ls
```

---

## Explicação dos Comandos Principais

| Comando | Descrição |
|---------|-----------|
| `openssl enc -aes-256-ctr -salt -in <entrada> -out <saída>` | Cifra um arquivo com AES-256-CTR |
| `openssl enc -d -aes-256-ctr -in <entrada> -out <saída>` | Decifra um arquivo com AES-256-CTR |

---

## Comparativo de Modos de Operação AES

| Modo | Tipo | Paralelizável | Propagação de Erros |
|------|------|---------------|---------------------|
| **CTR** | Cifra de fluxo | ✅ Sim | ❌ Não |
| CBC | Cifra de bloco | ❌ Não | ✅ Sim |
| ECB | Cifra de bloco | ✅ Sim | ❌ Não (inseguro) |

---

## Aprendizado
- AES-256-CTR é considerado o modo mais seguro e eficiente para criptografia simétrica
- O parâmetro `-salt` garante que cifragens da mesma mensagem com a mesma senha produzam resultados diferentes
- O arquivo original é completamente recuperado após a decifragem com a senha correta
- Uma senha incorreta na decifragem resulta em um arquivo corrompido, não em erro imediato
