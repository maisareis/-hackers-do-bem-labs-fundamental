# Atividade 6.4 – Antivírus ClamAV e Assinaturas de Malware

## Objetivo
Explorar o antivírus ClamAV no Kali Linux, conhecendo sua base de assinaturas de malware e entendendo como funcionam os antimalwares baseados em assinatura.

## Conceito

### Antivírus Baseado em Assinatura
Um **antivírus baseado em assinatura** funciona comparando arquivos com uma base de dados de padrões conhecidos de malware (assinaturas). Cada assinatura é um identificador único de um malware específico.

### ClamAV
O **ClamAV** é um antivírus open source amplamente utilizado em servidores Linux. Suas principais ferramentas são:

| Ferramenta | Descrição |
|-----------|-----------|
| `clamscan` | Escaneamento de arquivos e diretórios |
| `clambc` | Teste de bytecode de assinaturas |
| `sigtool` | Gerenciamento de assinaturas |
| `freshclam` | Atualização da base de assinaturas |

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Ver opções do clambc
```bash
clambc -h
```

### 3. Explorar o sigtool
```bash
sigtool -h
```

### 4. Listar assinaturas de malware
```bash
sigtool --list-sigs
```
> Este comando lista todas as assinaturas de malware conhecidas pelo ClamAV.

---

## Estrutura de uma Assinatura
Exemplo de assinatura listada:
```
BC.Img.Exploit.CVE_2017_12099-6336630-0
```
- **BC** → tipo bytecode
- **Img** → categoria imagem
- **Exploit** → tipo de ameaça
- **CVE_2017_12099** → identificador da vulnerabilidade

---

## CVE (Common Vulnerabilities and Exposures)
O **CVE** é um sistema de identificação padronizado para vulnerabilidades de segurança. Cada vulnerabilidade recebe um identificador único no formato `CVE-ANO-NÚMERO`.

### Fontes para pesquisa de CVEs
- **NVD (National Vulnerability Database):** https://nvd.nist.gov
- **CVE Details:** https://www.cvedetails.com

---

## Exemplo Estudado: CVE-2017-12099
- **Software afetado:** Blender 2.78c
- **Tipo de ataque:** Integer Overflow
- **Impacto:** Execução de código arbitrário via arquivo `.blend` malicioso
- **Categoria:** Overflow

### O que é Integer Overflow?
O **Integer Overflow** ocorre quando uma operação aritmética produz um resultado maior do que o tipo de dado pode armazenar, causando comportamento inesperado que pode ser explorado por atacantes.

---

## Aprendizado
- Antivírus baseados em assinatura identificam malwares por padrões conhecidos
- O ClamAV mantém uma base de dados atualizada de assinaturas
- CVEs são identificadores padronizados para vulnerabilidades
- Integer Overflow é uma vulnerabilidade comum em softwares que não validam entradas numéricas
