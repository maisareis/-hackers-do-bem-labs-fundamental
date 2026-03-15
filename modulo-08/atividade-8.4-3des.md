# Atividade 8.4 – Cifrando e Decifrando um Arquivo com 3DES no Kali Linux

## Objetivo
Implementar criptografia simétrica 3DES no modo CFB em um arquivo de imagem, cifrando e decifrando com sucesso utilizando o OpenSSL no Kali Linux.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais. O uso de técnicas de criptografia deve respeitar a legislação vigente.

## Conceito

O **3DES** (Triple DES) aplica o algoritmo DES três vezes consecutivas sobre os dados, aumentando a segurança do DES original. Apesar de ter sido amplamente utilizado no passado, hoje é considerado legado em comparação ao AES, mas ainda presente em sistemas mais antigos.

O **modo CFB** (Cipher Feedback) é um modo de operação que transforma uma cifra de bloco em uma cifra de fluxo, processando os dados em segmentos menores que o tamanho do bloco. Uma característica importante é que erros em um bloco cifrado se propagam para os blocos seguintes.

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
wget https://upload.wikimedia.org/wikipedia/commons/5/5a/Hyla_japonica_sep01.jpg
```

### 3. Visualizar a imagem no gerenciador de arquivos
Abrir o Thunar em Aplicativos → Gerenciador de Arquivos, navegar até `/home/aluno/Documentos/Arquivos/` e visualizar a imagem.

### 4. Verificar a presença da imagem, cifrá-la e listar
```bash
ls
openssl enc -des-ede3-cfb -salt -in Hyla_japonica_sep01.jpg -out arquivo_criptografado
ls
```
Informar e confirmar senha quando solicitado. Neste lab foi usada a senha `abcd1234`.

Resultado esperado após cifragem:
```
arquivo_criptografado  Hyla_japonica_sep01.jpg
```

### Parâmetros do comando de cifragem

| Parâmetro | Descrição |
|-----------|-----------|
| `enc` | Comando do OpenSSL para operações de criptografia |
| `-des-ede3-cfb` | Algoritmo 3DES (Triple DES) no modo CFB |
| `-salt` | Adiciona valor aleatório para aumentar a segurança da chave |
| `-in` | Arquivo de entrada a ser cifrado |
| `-out` | Arquivo de saída cifrado |

### 5. Apagar o original, decifrar e verificar o resultado
```bash
rm Hyla_japonica_sep01.jpg
ls
openssl enc -des-ede3-cfb -d -in arquivo_criptografado -out imagem.jpg
ls
```
Informar a mesma senha utilizada na cifragem (`abcd1234`).

Resultado esperado:
```
arquivo_criptografado  imagem.jpg
```

### Parâmetros do comando de decifragem

| Parâmetro | Descrição |
|-----------|-----------|
| `-des-ede3-cfb` | Algoritmo 3DES no modo CFB |
| `-d` | Indica operação de descriptografia |
| `-in` | Arquivo cifrado de entrada |
| `-out` | Arquivo decifrado de saída |

### 6. Verificar a imagem recuperada no Thunar
Voltar ao Thunar e abrir `imagem.jpg`, confirmando que é idêntica à imagem original `Hyla_japonica_sep01.jpg`.

### 7. Limpeza da pasta de trabalho
```bash
rm -r *
ls
```

---

## Explicação dos Comandos Principais

| Comando | Descrição |
|---------|-----------|
| `openssl enc -des-ede3-cfb -salt -in <entrada> -out <saída>` | Cifra um arquivo com 3DES-CFB |
| `openssl enc -des-ede3-cfb -d -in <entrada> -out <saída>` | Decifra um arquivo com 3DES-CFB |

---

## Comparativo AES-256 vs 3DES

| | AES-256 | 3DES |
|---|---|---|
| **Tamanho da chave** | 256 bits | 112–168 bits |
| **Velocidade** | ✅ Rápido | ❌ Lento |
| **Segurança atual** | ✅ Recomendado | ⚠️ Legado |
| **Uso moderno** | Padrão atual | Sistemas legados |

---

## Aprendizado
- O 3DES aplica o DES três vezes para aumentar a segurança do algoritmo original
- O modo CFB propaga erros de cifragem para blocos subsequentes
- O 3DES é considerado legado; para novos sistemas, AES-256 é o padrão recomendado
- O OpenSSL suporta múltiplos algoritmos criptográficos com uma interface unificada
