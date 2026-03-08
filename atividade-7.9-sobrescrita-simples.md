# Atividade 7.9 – Sobrescrita Simples com Dados Aleatórios

## Objetivo
Executar a sobrescrita de uma partição com dados aleatórios no Kali Linux, tornando a recuperação de dados praticamente impossível.

## Conceito

### Sobrescrita de Dados
A **sobrescrita** (wiping) consiste em gravar dados sobre os dados existentes em uma partição, dificultando ou impossibilitando a recuperação das informações originais.

### Por que formatar não é suficiente?
Uma simples formatação apenas remove as referências aos arquivos, mas os dados continuam fisicamente no disco e podem ser recuperados com ferramentas forenses. A sobrescrita elimina fisicamente os dados.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Criar sistema de arquivos e montar a partição
```bash
mkfs.ext4 /dev/nvme1n1p1
mkdir -m 777 /home/aluno/Documentos/MeuHD
mount /dev/nvme1n1p1 /home/aluno/Documentos/MeuHD
```

### 3. Criar arquivo de teste
```bash
cd /home/aluno/Documentos/MeuHD
nano Teste.txt
# Conteúdo: "Teste"
```

### 4. Executar sobrescrita com dados aleatórios
```bash
dd if=/dev/urandom of=/dev/nvme1n1p1 bs=10M
```

### Parâmetros do dd

| Parâmetro | Descrição |
|-----------|-----------|
| `if=/dev/urandom` | Origem: gerador de dados aleatórios do sistema |
| `of=/dev/nvme1n1p1` | Destino: partição a ser sobrescrita |
| `bs=10M` | Tamanho do bloco: 10 megabytes por vez |

### 5. Verificar o resultado
```bash
df -h
# /dev/nvme1n1p1 aparecerá com 100% de uso
```

---

## Resultado Esperado
- A partição aparece com **100% de uso**
- O arquivo `Teste.txt` não pode mais ser listado
- Os dados foram sobrescritos com dados aleatórios

---

## Aprendizado
- `dd` é uma ferramenta poderosa para operações de baixo nível em discos
- `/dev/urandom` gera dados verdadeiramente aleatórios
- Sobrescrita com dados aleatórios dificulta análise forense
- Uma passagem já é suficiente para discos modernos (SSD/NVMe)
