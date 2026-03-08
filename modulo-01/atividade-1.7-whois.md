# Atividade 1.7 – Reconhecimento com WHOIS

## Objetivo
Utilizar a ferramenta WHOIS para consultar informações públicas de registro de domínios na Internet.

## Conceito
O **WHOIS** é um serviço de pesquisa que retorna informações sobre o registro de domínios, incluindo proprietário, contato técnico, servidores DNS e datas de registro. É amplamente utilizado para:

- Verificar disponibilidade de domínios
- Identificar proprietários de sites
- Investigar questões de segurança cibernética
- Reconhecimento passivo (OSINT)

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Consultar domínio institucional
```bash
whois esr.rnp.br
```

**Resultado:**
```
domain:      rnp.br
owner:       Rede Nacional de Ensino e Pesquisa
owner-c:     RCO217
tech-c:      RCO217
nserver:     nscl1.rnp.br 162.159.8.154
created:     before 19950101
changed:     20201126
status:      published
```

### 3. Consultar domínio de prefeitura estadual
```bash
whois guanambi.ba.gov.br
```

**Resultado:**
```
domain:      ba.gov.br
owner:       Cia. de Processamento de Dados do Estado da Bahia
owner-c:     CPDBA1
tech-c:      CPDBA1
nserver:     cpu0034.ba.gov.br 200.187.60.34
created:     19951227
status:      published
```

### 4. Consultar domínio de município
```bash
whois itapaje.ce.gov.br
```

**Resultado:**
```
domain:      ce.gov.br
owner:       EMPRESA DE TECNOLOGIA DA INFORMACAO DO CEARA-ETICE
owner-c:     RAOLI10
tech-c:      VLS171
nserver:     intsrv007.etice.ce.gov.br 189.90.164.21
created:     19960527
status:      published

nic-hdl-br:  RAOLI10
person:      Raimundo Osman Lima

nic-hdl-br:  VLS171
person:      Vera Lucia Carneiro de Sousa
```

---

## Campos Retornados pelo WHOIS

| Campo | Descrição |
|-------|-----------|
| `domain` | Nome do domínio consultado |
| `owner` | Proprietário do domínio |
| `owner-c` | Código identificador do proprietário |
| `tech-c` | Código do responsável técnico |
| `nserver` | Servidores DNS associados (IPv4 e IPv6) |
| `created` | Data de criação do domínio |
| `changed` | Data da última alteração |
| `status` | Status atual do domínio |
| `nic-hdl-br` | Handle do contato no Registro.br |
| `person` | Nome do responsável pelo domínio |

---

## Observações de Segurança

- Nomes de responsáveis técnicos expostos podem ser alvos de engenharia social
- Servidores DNS revelam a infraestrutura utilizada
- Status `TIMEOUT` em um servidor DNS indica possível problema de manutenção
- Administradores podem usar serviços de **privacidade de WHOIS** para proteger dados

---

## Aprendizado
- WHOIS retorna informações publicamente disponíveis sobre qualquer domínio
- Útil para reconhecimento passivo sem interagir diretamente com o alvo
- Revela nomes de responsáveis, servidores DNS e histórico do domínio
- Informações como e-mails e contatos técnicos são valiosas para ataques de engenharia social
