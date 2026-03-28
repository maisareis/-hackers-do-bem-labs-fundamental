# Atividade 12.6 – Lendo Metadados com Exiftool no Kali Linux

## Objetivo
Extrair e analisar os metadados de uma imagem JPEG e de um documento PDF utilizando o Exiftool no Kali Linux.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais.

## Conceito

**Metadados** são dados sobre dados — informações embutidas nos arquivos digitais que descrevem suas propriedades. Em imagens, os metadados EXIF (Exchangeable Image File Format) podem revelar informações como câmera utilizada, data/hora da captura, software de edição e até coordenadas GPS.

O **Exiftool** é a principal ferramenta de linha de comando para leitura, escrita e remoção de metadados em dezenas de formatos de arquivo.

> ⚠️ Metadados podem expor informações sensíveis ao compartilhar arquivos publicamente — localização GPS em fotos, nome do autor em documentos, etc.

---

## Pré-requisitos
- Kali Linux com Exiftool instalado
- Conexão com a internet para baixar os arquivos de teste

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Acessar a pasta de trabalho e baixar a imagem
```bash
cd /home/aluno/Documentos/Arquivos
wget http://metadatadeluxe.pbworks.com/f/1242756531/IPTCpanel.jpg
```

### 3. Verificar o download
```bash
ls
```

### 4. Visualizar os metadados da imagem
```bash
exiftool IPTCpanel.jpg
```

### Principais metadados extraídos da imagem

| Campo | Valor / Descrição |
|-------|-----------------|
| **File Name** | Nome do arquivo |
| **File Size** | Tamanho em KB |
| **File Modification Date/Time** | Data da última modificação |
| **File Access Date/Time** | Data do último acesso |
| **File Permissions** | Permissões de acesso |
| **File Type** | Tipo do arquivo (JPEG) |
| **MIME Type** | Tipo MIME (image/jpeg) |
| **Software** | Software usado na criação (Adobe Photoshop CS2) |
| **Modify Date** | Data de modificação nos metadados EXIF |
| **Artist** | Criador da imagem |
| **Copyright** | Informação de direitos autorais |
| **Exif Image Width/Height** | Dimensões em pixels |
| **Color Space** | Espaço de cores utilizado |

### 5. Baixar o PDF de teste
```bash
wget https://www.cnj.jus.br/wp-content/uploads/2021/07/contrato_08_2021.pdf
ls
```

### 6. Visualizar os metadados do PDF
```bash
exiftool contrato_08_2021.pdf
```

### Principais metadados extraídos do PDF

| Campo | Valor / Descrição |
|-------|-----------------|
| **File Name** | Nome do arquivo |
| **File Size** | Tamanho em MB |
| **Page Count** | Número de páginas (626) |
| **Language** | Idioma do documento (pt-BR) |
| **Creator Tool** | Software usado na criação (Microsoft Word para Microsoft 365) |
| **Create Date** | Data de criação do documento |
| **Modify Date** | Data da última modificação |
| **Producer** | Software usado para gerar o PDF |
| **PDF Version** | Versão do padrão PDF (1.7) |
| **Document ID** | Identificador único do documento |

Não apagar os arquivos — serão usados na próxima atividade. Fechar o Terminal.

---

## Aprendizado
- Metadados EXIF de fotos podem revelar localização GPS, modelo do dispositivo e data/hora da captura — sempre verificar antes de publicar imagens
- Documentos PDF criados com Microsoft Word mantêm o nome do usuário e a data de criação nos metadados
- O Exiftool suporta mais de 100 formatos de arquivo — JPEG, PDF, MP4, DOCX, PNG, etc.
- Metadados são frequentemente usados em investigações forenses para determinar a autoria e o histórico de um arquivo
