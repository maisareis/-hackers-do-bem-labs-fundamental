# Atividade 7.10 – Sobrescrita com Padrão DoD 5220.22-M

## Objetivo
Executar a sobrescrita de uma partição seguindo o padrão militar DoD 5220.22-M usando o comando `shred` no Kali Linux.

## Conceito

### Padrão DoD 5220.22-M
O **DoD 5220.22-M** é um padrão de sanitização de dados do Departamento de Defesa dos Estados Unidos. Define o processo de sobrescrita de dados para garantir que informações confidenciais não possam ser recuperadas.

O padrão básico consiste em:
1. **Passagem 1:** Sobrescrita com dados aleatórios
2. **Passagem 2:** Sobrescrita com zeros (0x00)

### shred
O **shred** é uma ferramenta do Linux que implementa sobrescrita segura de arquivos e partições seguindo padrões como o DoD 5220.22-M.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Criar nova partição para o teste
```bash
fdisk /dev/nvme1n1
```
Dentro do fdisk:
- `n` → nova partição
- Enter → tipo primário
- Enter → número 2
- Enter → setor inicial padrão
- `+100M` → tamanho

- `w` → salvar e sair

```bash
partprobe /dev/nvme1n1
```

### 3. Verificar a criação da partição
```bash
lsblk
```

### 4. Executar sobrescrita com padrão DoD
```bash
shred -vfz -n 1 /dev/nvme1n1p2
```

**Saída esperada:**
```
shred: /dev/nvme1n1p2: passagem 1/2 (random)...
shred: /dev/nvme1n1p2: passagem 2/2 (000000)...
```

---

## Parâmetros do shred

| Parâmetro | Descrição |
|-----------|-----------|
| `-v` | Modo verboso: exibe o progresso |
| `-f` | Força a abertura mesmo sem permissão de escrita |
| `-z` | Adiciona passagem final com zeros para ocultar a sobrescrita |
| `-n 1` | Número de passagens aleatórias (1 passagem = DoD básico) |

---

## Comparativo de Métodos de Sobrescrita

| Método | Passagens | Segurança | Uso |
|--------|-----------|-----------|-----|
| Formatação simples | 0 | ❌ Baixa | Uso geral |
| `dd` com urandom | 1 aleatória | ✅ Boa | Discos modernos |
| DoD 5220.22-M básico | 1 aleatória + zeros | ✅✅ Muito boa | Dados sensíveis |
| DoD 5220.22-M completo | 7 passagens | ✅✅✅ Excelente | Dados críticos |
| Gutmann | 35 passagens | ✅✅✅✅ Máxima | Discos magnéticos antigos |

---

## Aprendizado
- O padrão DoD 5220.22-M garante que dados não possam ser recuperados
- `shred -n 1 -z` executa 1 passagem aleatória + 1 passagem com zeros
- Para HDDs antigos recomenda-se mais passagens; SSDs/NVMe modernos, 1 passagem já é suficiente
- A recuperação de dados após sobrescrita com DoD é praticamente impossível
