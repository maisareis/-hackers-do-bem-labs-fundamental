# Atividade 5.10 – Implementando a Rotação de Senha no Kali Linux

## Objetivo

Configurar as políticas de **rotação de senhas** no Kali Linux editando o arquivo `/etc/login.defs` para definir validade máxima, tempo mínimo e período de aviso de expiração.

## Aviso Legal

> Todo material desta atividade deve ser usado **somente para fins acadêmicos**.

## Conceito

A **rotação de senhas** é uma prática de segurança que força a troca periódica de senhas, reduzindo o risco de uso indevido de credenciais comprometidas. No Linux, as políticas globais de senha são definidas em `/etc/login.defs` e aplicadas a novos usuários criados após a alteração.

### Parâmetros do `/etc/login.defs`

| Parâmetro        | Descrição                                                                      |
|------------------|--------------------------------------------------------------------------------|
| `PASS_MAX_DAYS`  | Número máximo de dias em que uma senha é válida antes de expirar               |
| `PASS_MIN_DAYS`  | Número mínimo de dias que uma senha deve ser usada antes de poder ser trocada  |
| `PASS_WARN_AGE`  | Dias de antecedência em que o usuário é avisado sobre o vencimento da senha    |

### Campos da saída do `passwd -S`

| Campo       | Exemplo      | Significado                                                    |
|-------------|--------------|----------------------------------------------------------------|
| Usuário     | `root`       | Nome do usuário                                                |
| Status      | `P`          | P = senha definida; L = conta bloqueada; NP = sem senha        |
| Data        | `2024-07-02` | Data da última alteração de senha                              |
| Min days    | `0`          | Mínimo de dias antes de poder trocar a senha                   |
| Max days    | `99999`      | Máximo de dias de validade da senha                            |
| Warn days   | `7`          | Dias de aviso antes do vencimento                              |
| Inativo     | `-1`         | Dias após expiração para desativar a conta (-1 = nunca)        |

### Alterações realizadas

| Parâmetro        | Valor anterior | Valor novo | Significado da mudança                                  |
|------------------|----------------|------------|----------------------------------------------------------|
| `PASS_MAX_DAYS`  | 99999          | 270        | Senha expira após 270 dias (~9 meses)                    |
| `PASS_MIN_DAYS`  | 0              | 90         | Senha deve ser usada por pelo menos 90 dias antes de trocar |
| `PASS_WARN_AGE`  | 7              | 10         | Aviso de expiração com 10 dias de antecedência           |

## Passo a Passo

**1.** Conectar ao Kali Linux via RDP ao IP `192.168.98.40` com usuário `aluno` e senha `rnpesr`.

**2.** Abrir o Terminal e elevar privilégios:
```bash
sudo -i
```

**3.** Verificar os parâmetros atuais de senha do usuário `root`:
```bash
passwd -S
```
Saída esperada:
```
root P 2024-07-02 0 99999 7 -1
```

**4.** Sair do modo root e verificar os parâmetros do usuário `aluno`:
```bash
exit
passwd -S
```
Saída esperada:
```
aluno P 2025-09-06 0 99999 7 -1
```

**5.** *(Print solicitado)* Editar o arquivo de configuração global de senhas e modificar os parâmetros:
```bash
sudo nano /etc/login.defs
```
Localizar e alterar de:
```
PASS_MAX_DAYS   99999
PASS_MIN_DAYS   0
PASS_WARN_AGE   7
```
Para:
```
PASS_MAX_DAYS   270
PASS_MIN_DAYS   90
PASS_WARN_AGE   10
```
Salvar com `Ctrl+X` → `s` → `Enter`.

**6.** Fechar o Terminal.

## Aprendizado

- As configurações em `/etc/login.defs` afetam apenas **novos usuários** criados após a alteração — para aplicar a usuários existentes, seria necessário usar `chage -M 270 -m 90 -W 10 nomeusuario` individualmente.
- `PASS_MIN_DAYS 90` impede que usuários troquem a senha imediatamente após uma troca forçada, evitando que retornem à senha anterior de forma rápida.
- `PASS_MAX_DAYS 270` estabelece um ciclo de rotação de ~9 meses — em ambientes de alta segurança, valores entre 60 e 90 dias são mais comuns.
- Em distribuições modernas, recomenda-se complementar a rotação com autenticação multifator (MFA), pois senhas expiradas trocadas por senhas fracas podem ser um risco maior que senhas longas sem expiração.
- O comando `chage -l nomeusuario` permite verificar a política de senha de um usuário específico em detalhes.
