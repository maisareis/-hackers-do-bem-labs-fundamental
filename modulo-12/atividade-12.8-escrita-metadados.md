# Atividade 12.8 – Escrevendo Metadados com Exiftool no Kali Linux

## Objetivo
Inserir e sobrescrever metadados em uma imagem JPEG utilizando o Exiftool, adicionando campos de direitos autorais, autor e data de criação fictícia.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

Além de ler e remover metadados, o Exiftool permite **escrever e modificar** metadados em arquivos. Isso tem aplicações legítimas como:

- Catalogar imagens com informações de autoria e direitos
- Corrigir datas incorretas em fotos
- Adicionar informações de localização

Mas também pode ser usado de forma maliciosa para falsificar a autoria ou data de criação de arquivos — o que é importante compreender para fins de análise forense.

---

## Pré-requisitos
- Kali Linux com Exiftool instalado
- Arquivo `IPTCpanel.jpg` (sem metadados) da Atividade 12.7

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Acessar a pasta e verificar os arquivos
```bash
cd /home/aluno/Documentos/Arquivos
ls
```

### 3. Adicionar direitos autorais ao arquivo
```bash
exiftool -rights="Direitos autorais do Professor" -CopyrightNotice="Direitos autorais do Professor" IPTCpanel.jpg
```
Dois métodos são usados simultaneamente pois diferentes formatos respondem a campos diferentes.

Resultado esperado:
```
1 image files updated
```

### 4. Verificar os campos adicionados
```bash
exiftool IPTCpanel.jpg
```
Os campos `Rights` e `Copyright Notice` devem aparecer com o valor inserido.

### 5. Adicionar o nome do autor
```bash
exiftool -XMP-dc:Creator="Prof. Max" "IPTCpanel.jpg"
```

### 6. Verificar o campo de autor
```bash
exiftool IPTCpanel.jpg
```
O campo `Creator: Prof. Max` deve aparecer nos metadados.

### 7. Inserir uma data de criação fictícia e verificar o resultado
```bash
exiftool -AllDates="1917:12:12 06:00:00" "IPTCpanel.jpg"
exiftool IPTCpanel.jpg
```
O parâmetro `-AllDates` modifica simultaneamente os campos `Modify Date`, `Date/Time Original` e `Create Date`.

Resultado esperado nos metadados:
```
Modify Date        : 1917:12:12 06:00:00
Date/Time Original : 1917:12:12 06:00:00
Create Date        : 1917:12:12 06:00:00
```

### 8. Limpar e fechar
```bash
rm -r *.*
ls
```
Fechar o Terminal.

---

## Resumo dos campos modificados

| Campo | Valor inserido | Comando |
|-------|---------------|---------|
| Rights / Copyright Notice | "Direitos autorais do Professor" | `-rights=` / `-CopyrightNotice=` |
| Creator | "Prof. Max" | `-XMP-dc:Creator=` |
| AllDates | 1917:12:12 06:00:00 | `-AllDates=` |

---

## Aprendizado
- Metadados podem ser completamente fabricados — a data de "criação" de um arquivo não é prova confiável por si só em análise forense
- O parâmetro `-AllDates` modifica todos os campos de data de uma vez — conveniente para correção em lote
- Em investigações forenses, a data de modificação do inode (no sistema de arquivos) é mais difícil de falsificar que os metadados internos do arquivo
- Para mais opções: `https://exiftool.org/`
