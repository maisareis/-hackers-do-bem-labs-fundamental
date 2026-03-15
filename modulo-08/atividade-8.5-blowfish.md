# Atividade 8.5 – Cifrando e Decifrando um Arquivo com Blowfish no Kali Linux

## Objetivo
Implementar criptografia simétrica Blowfish de 128 bits no modo CFB em um arquivo de imagem, cifrando e decifrando com sucesso utilizando o OpenSSL no Kali Linux.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais. O uso de técnicas de criptografia deve respeitar a legislação vigente.

## Conceito

O **Blowfish** é um algoritmo de criptografia simétrica por blocos de 64 bits, criado por Bruce Schneier em 1993 como alternativa ao DES. Utiliza chaves de tamanho variável entre 32 e 448 bits, sendo 128 bits a configuração mais comum. É conhecido por sua velocidade em software e ausência de patentes.

O **modo CFB** (Cipher Feedback) transforma a cifra de bloco em cifra de fluxo, processando dados em segmentos menores que o bloco. Erros em um segmento cifrado se propagam para os segmentos subsequentes.

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
wget "https://upload.wikimedia.org/wikipedia/commons/9/9e/CRS-20_Dragon%E2%80%93Enhanced.jpg"
```

### 3. Visualizar a imagem no gerenciador de arquivos
Abrir o Thunar em Aplicativos → Gerenciador de Arquivos, navegar até `/home/aluno/Documentos/Arquivos/`, visualizar a imagem e fechar o visualizador.

### 4. Verificar a presença da imagem, cifrá-la e listar
```bash
ls
openssl enc -bf-cfb -salt -in CRS-20_Dragon–Enhanced.jpg -out arquivo_criptografado
ls
```
Informar e confirmar senha quando solicitado. Neste lab foi usada a senha `abcd1234`.

Resultado esperado após cifragem:
```
arquivo_criptografado  CRS-20_Dragon–Enhanced.jpg
```

### Parâmetros do comando de cifragem

| Parâmetro | Descrição |
|-----------|-----------|
| `enc` | Comando do OpenSSL para operações de criptografia |
| `-bf-cfb` | Algoritmo Blowfish no modo CFB |
| `-salt` | Adiciona valor aleatório para aumentar a segurança da chave |
| `-in` | Arquivo de entrada a ser cifrado |
| `-out` | Arquivo de saída cifrado |

### 5. Apagar o original, decifrar e verificar o resultado
```bash
rm CRS-20_Dragon–Enhanced.jpg
ls
openssl enc -bf-cfb -d -in arquivo_criptografado -out imagem.jpg
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
| `-bf-cfb` | Algoritmo Blowfish no modo CFB |
| `-d` | Indica operação de descriptografia |
| `-in` | Arquivo cifrado de entrada |
| `-out` | Arquivo decifrado de saída |

### 6. Verificar a imagem recuperada no Thunar
Voltar ao Thunar e abrir `imagem.jpg`, confirmando que é idêntica à imagem original `CRS-20_Dragon–Enhanced.jpg`.

### 7. Limpeza da pasta de trabalho
```bash
rm -r *
ls
```

---

## Comparativo de Algoritmos Simétricos

| Algoritmo | Tamanho do Bloco | Tamanho da Chave | Status |
|-----------|-----------------|-----------------|--------|
| **AES-256** | 128 bits | 256 bits | ✅ Padrão atual |
| **3DES** | 64 bits | 112–168 bits | ⚠️ Legado |
| **Blowfish** | 64 bits | 32–448 bits | ⚠️ Legado |

---

## Aprendizado
- O Blowfish foi criado como alternativa livre e rápida ao DES nos anos 90
- Apesar de ainda funcional, o tamanho de bloco de 64 bits torna o Blowfish vulnerável a ataques modernos em grandes volumes de dados
- Para novos sistemas, AES-256 é sempre a escolha recomendada
- O OpenSSL permite experimentar algoritmos legados para fins educacionais e de compatibilidade
