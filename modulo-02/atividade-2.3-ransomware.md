# Atividade 2.3 – Explorando Ransomwares no Kali Linux

## Objetivo
Explorar um repositório educacional de ransomwares e entender como criminosos obtêm essas ferramentas.

## ⚠️ Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente de laboratório controlado, com fins educacionais. **NUNCA execute o ransomware criado em uma máquina Windows real** — resultará na execução do ataque e criptografia irreversível dos arquivos.

## Conceito

### Ransomware
O **ransomware** é um tipo de malware que criptografa os arquivos da vítima e exige um resgate (ransom) para fornecer a chave de descriptografia. É um dos ataques cibernéticos mais prejudiciais e lucrativos atualmente.

### WannaCry
O **WannaCry** foi um dos maiores ataques de ransomware da história, ocorrido em maio de 2017. Infectou mais de 200.000 computadores em 150 países, afetando hospitais, empresas e governos. Explorava a vulnerabilidade **EternalBlue** no protocolo SMB do Windows.

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Executar o criador de ransomware
```bash
cd /curso/Ransomware
python3 Ransomware
```

### 3. Ver os comandos disponíveis
```
Ransomware®Creator~> help
```

### 4. Listar ransomwares disponíveis
```
Ransomware®Creator~> show
```

**Ransomwares disponíveis:**
```
01. Ransomware.Cerber        09. Ransomware.Radamant
02. Ransomware.Cryptowall    10. Ransomware.Rex
03. Ransomware.Jigsaw        11. Ransomware.Satana
04. Ransomware.Locky         12. Ransomware.TeslaCrypt
05. Ransomware.Mamba         13. Ransomware.Vipasana
06. Ransomware.Matsnu        14. Ransomware.WannaCry
07. Ransomware.Petrwrap      15. Ransomware.WannaCry_Plus
08. Ransomware.Petya
```

### 5. Criar o WannaCry
```
Ransomware®Creator~> 14

 File name: Ransomware.WannaCry.zip
 File type: .zip
 Password : infected

[+] Loading... 100 % [success]
 File saved as /sdcard
```

### 6. Verificar o arquivo criado
```bash
cd /
ls
# bin  curso  etc  ...  sdcard  ...
```

### 7. Visualizar no gerenciador de arquivos
- Abrir o Thunar
- Navegar para `/`
- Abrir o arquivo `sdcard`
- Visualizar o executável `.exe` de 3,5MB (o ransomware WannaCry)

---

## Linha do Tempo do WannaCry

| Data | Evento |
|------|--------|
| Abril 2017 | NSA desenvolve EternalBlue |
| Abril 2017 | Shadow Brokers vaza ferramentas da NSA |
| Maio 2017 | WannaCry infecta 200.000+ computadores |
| Maio 2017 | Marcus Hutchins encontra o kill switch acidental |

---

## Como o WannaCry Funcionava

```
1. Explorar EternalBlue (SMB v1) → 2. Instalar backdoor DoublePulsar
→ 3. Baixar WannaCry → 4. Criptografar arquivos → 5. Exigir resgate em Bitcoin
```

---

## Aprendizado
- Ransomwares são facilmente encontrados na internet por criminosos
- O WannaCry explorou uma vulnerabilidade conhecida e não corrigida
- Backups regulares são a principal defesa contra ransomware
- Manter sistemas atualizados evita a exploração de vulnerabilidades conhecidas
