# Atividade 6.9 – Verificando Vulnerabilidade ao Clickjacking

## Objetivo
Utilizar a ferramenta online clickjacker.io para testar se sites são vulneráveis ao ataque Clickjacking.

## Ferramenta Utilizada
**clickjacker.io** → ferramenta online que verifica se um site possui as proteções necessárias contra Clickjacking.

---

## Campos Analisados

### X-Frame-Options
Cabeçalho HTTP que controla se o conteúdo pode ser exibido em iframes.

| Valor | Significado |
|-------|-------------|
| `DENY` | Não pode ser exibido em nenhum iframe |
| `SAMEORIGIN` | Só pode ser exibido em iframes do mesmo domínio |
| `Missing header` | Cabeçalho ausente → vulnerável |

### CSP Header (Frame-Ancestors)
Diretiva da Política de Segurança de Conteúdo que controla de quais domínios o site pode ser incorporado em iframes.

| Valor | Significado |
|-------|-------------|
| `frame-ancestors 'none'` | Não pode ser incorporado em nenhum iframe |
| `frame-ancestors 'self'` | Só pode ser incorporado pelo próprio site |
| `Missing anti-framing policy` | Política ausente → possível vulnerabilidade |

---

## Testes Realizados

### Site 1: esr.rnp.br
```
X-Frame-Options: SAMEORIGIN
CSP Header: Missing anti-framing policy
```
**Resultado:** Parcialmente protegido. O `SAMEORIGIN` impede Clickjacking externo, mas a ausência de CSP Frame-Ancestors é uma falha secundária.

### Site 2: www.hackthissite.org
```
X-Frame-Options: Missing header
CSP Header: Missing anti-framing policy
```
**Resultado:** ⚠️ **Vulnerável ao Clickjacking!** Ausência de ambas as proteções.

---

## Como Interpretar os Resultados

| X-Frame-Options | CSP Frame-Ancestors | Situação |
|-----------------|--------------------|---------| 
| DENY | frame-ancestors 'none' | ✅ Totalmente protegido |
| SAMEORIGIN | Presente | ✅ Bem protegido |
| SAMEORIGIN | Missing | ⚠️ Parcialmente protegido |
| Missing | Missing | ❌ Vulnerável |

---

## Aprendizado
- Nem todos os sites possuem proteção contra Clickjacking
- A ausência do cabeçalho `X-Frame-Options` é uma vulnerabilidade comum
- Ferramentas online como clickjacker.io facilitam testes rápidos de segurança
- Administradores de sites devem configurar corretamente os cabeçalhos de segurança HTTP
