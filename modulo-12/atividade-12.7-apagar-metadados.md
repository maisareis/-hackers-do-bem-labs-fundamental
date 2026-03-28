# Atividade 12.7 – Apagando Metadados com Exiftool no Kali Linux

## Objetivo
Remover os metadados de uma imagem JPEG utilizando o Exiftool, verificar a remoção no arquivo resultante e confirmar que o arquivo original com metadados foi preservado automaticamente.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

A **remoção de metadados** é uma prática importante para preservar a privacidade ao compartilhar arquivos. O Exiftool remove os metadados e automaticamente cria um backup do arquivo original com a extensão `_original`.

Situações em que remover metadados é recomendado:
- Publicar fotos em redes sociais (GPS, modelo do dispositivo)
- Compartilhar documentos com informações do autor
- Publicar imagens criadas com software proprietário
- Qualquer situação onde os metadados possam revelar informações sensíveis

---

## Pré-requisitos
- Kali Linux com Exiftool instalado
- Arquivos da Atividade 12.6 presentes na pasta de trabalho

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
Os arquivos `contrato_08_2021.pdf` e `IPTCpanel.jpg` devem estar presentes.

### 3. Remover todos os metadados da imagem
```bash
exiftool -all= IPTCpanel.jpg
```
O parâmetro `-all=` remove todos os metadados do arquivo. Um aviso sobre o perfil ICC pode aparecer — é normal.

Resultado esperado:
```
1 image files updated
```

### 4. Verificar os arquivos criados
```bash
ls
```
Dois novos arquivos devem aparecer:
- `IPTCpanel.jpg` — arquivo com metadados removidos
- `IPTCpanel.jpg_original` — backup automático do arquivo original

### 5. Confirmar a remoção dos metadados no arquivo modificado
```bash
exiftool IPTCpanel.jpg
```
A saída deve ser significativamente menor — apenas campos técnicos básicos permanecem (dimensões, codificação, etc.). Campos como `Software`, `Artist`, `Copyright`, `Modify Date` e dados IPTC não devem aparecer.

### 6. Confirmar que o arquivo original foi preservado
```bash
exiftool IPTCpanel.jpg_original
```
A saída completa com todos os metadados originais deve ser exibida, incluindo `Software: Adobe Photoshop CS2`, `Artist`, `Copyright` e todos os campos IPTC/XMP.

Não apagar os arquivos — serão usados na próxima atividade. Fechar o Terminal.

---

## Comparativo antes e depois da remoção

| | Antes (`_original`) | Depois (`IPTCpanel.jpg`) |
|---|---|---|
| **File Size** | 87 kB | 51 kB |
| **Software** | Adobe Photoshop CS2 | Não presente |
| **Artist** | IPTC CONTACT PANEL: CREATOR | Não presente |
| **Copyright** | IPTC STATUS PANEL | Não presente |
| **Modify Date** | 2009:05:19 | Não presente |
| **Campos IPTC/XMP** | Presentes (dezenas) | Não presentes |

---

## Aprendizado
- O Exiftool cria automaticamente um backup `_original` antes de modificar o arquivo — proteção importante contra perda acidental de dados
- A remoção de metadados reduz o tamanho do arquivo (87 kB → 51 kB neste caso)
- Remover metadados não altera o conteúdo visual da imagem
- Para scripts de limpeza em lote, o Exiftool aceita wildcards: `exiftool -all= *.jpg`
