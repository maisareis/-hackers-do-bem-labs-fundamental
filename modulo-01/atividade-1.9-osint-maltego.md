# Atividade 1.9 – Reconhecimento Passivo OSINT com Maltego

## Objetivo
Utilizar o Maltego para realizar reconhecimento passivo OSINT a partir do site da Universidade Federal do Ceará (ufc.br).

## Conceito

### OSINT (Open Source Intelligence)
O **OSINT** é a coleta e análise de informações disponíveis publicamente para fins de inteligência. No contexto de segurança, é usado para mapear a superfície de ataque de um alvo sem interagir diretamente com seus sistemas.

### Reconhecimento Passivo
O **reconhecimento passivo** coleta informações sem enviar pacotes diretamente ao alvo, tornando a atividade difícil de detectar pelos sistemas de monitoramento.

---

## Passo a Passo

### 1. Abrir o Maltego
```bash
maltego
```

### 2. Criar novo grafo
Clicar em **New** para criar um novo grafo de investigação.

### 3. Adicionar entidade Website
- Na **Entity Palette**, localizar **Website** no grupo **Infrastructure**
- Arrastar para o painel central
- Alterar o domínio para `ufc.br`

### 4. Descobrir domínios relacionados
```
Clique direito → + All Transforms → [Utilities] To Domains [within Properties]
```
Resultado: domínio principal da UFC é exibido.

### 5. Descobrir nomes DNS associados
```
Clique direito no site ufc.br → [Utilities] To DNSNames [within Properties]
```

### 6. Encontrar pessoas vinculadas
```
Clique direito no site ufc.br → [Utilities] To Person [PGP]
```
Resultado: nomes de pessoas vinculadas ao domínio são apresentados.

### 7. Encontrar e-mails institucionais
```
Clique direito no site ufc.br → [Utilities] To Email Addresses [PGP]
```
Resultado: e-mails institucionais de colaboradores são apresentados.

### 8. Encontrar servidor de e-mail
```
Clique direito no site ufc.br → [Utilities] To DNS Name – MX (mail server)
```
Resultado: servidores de e-mail são do **Google** (Google Workspace).

---

## Informações Coletadas via Maltego

| Transform | Tipo de Informação |
|-----------|-------------------|
| To Domains | Domínios e subdomínios relacionados |
| To DNSNames | Registros DNS (A, CNAME, MX, etc.) |
| To Person [PGP] | Nomes de pessoas vinculadas ao domínio |
| To Email Addresses [PGP] | E-mails institucionais públicos |
| To DNS Name – MX | Servidor de e-mail utilizado |

---

## Por que Isso é Relevante em Segurança?

| Informação | Risco Potencial |
|-----------|----------------|
| E-mails institucionais | Alvos para spear phishing |
| Nomes de funcionários | Engenharia social direcionada |
| Servidor de e-mail (MX) | Identificação da plataforma para ataques |
| Subdomínios | Superfície de ataque expandida |

---

## Aprendizado
- Maltego permite mapear visualmente relações entre entidades públicas
- E-mails e nomes de funcionários são valiosos para ataques de engenharia social
- Reconhecimento passivo não deixa rastros nos logs do alvo
- Organizações devem minimizar a exposição de informações públicas sensíveis
- Uso de Google Workspace como servidor de e-mail é uma informação relevante para o perfil do alvo
