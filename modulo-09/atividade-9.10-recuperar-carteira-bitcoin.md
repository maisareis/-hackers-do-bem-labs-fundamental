# Atividade 9.10 – Recuperando Carteira e Recebendo Bitcoins no Kali Linux

## Objetivo
Demonstrar a recuperação de uma carteira Bitcoin perdida utilizando a seed phrase no Electrum, e gerar um pedido de recebimento de Bitcoin com QR Code.

## Aviso Legal
> Esta atividade foi realizada exclusivamente em ambiente controlado e autorizado para fins educacionais. Nenhuma criptomoeda real foi utilizada.

## Conceito

A **recuperação de carteira** demonstra o princípio central das carteiras HD: a seed phrase contém toda a informação necessária para regenerar a carteira, independentemente do dispositivo ou software utilizado.

O **endereço Bitcoin** é o identificador público para recebimento de fundos — equivalente a um número de conta bancária, mas derivado criptograficamente da chave pública.

O **QR Code** do endereço Bitcoin facilita o pagamento via aplicativos móveis, codificando o endereço e o valor solicitado em formato legível por câmera.

---

## Pré-requisitos
- Atividade 9.9 concluída
- Seed phrase anotada da Atividade 9.9
- Kali Linux com Electrum instalado
- Executar como usuário `aluno` (não como root)

---

## Passo a Passo

### 1. Inicializar o Electrum
```bash
electrum
```

### 2. Simular perda da carteira
Na janela do Electrum, clicar em **File** → **Delete** → **Yes**. O Electrum fechará, simulando a perda do disco rígido.

### 3. Reinicializar o Electrum
```bash
electrum
```

### 4. Confirmar o nome da carteira
Manter o nome padrão `default_wallet` → **Next**.

### 5. Escolher o tipo de carteira
Selecionar **Standard wallet** → **Next**.

### 6. Selecionar recuperação por seed
Selecionar **I already have a seed** → **Next**.

### 7. Inserir a seed phrase
Digitar a seed phrase anotada na Atividade 9.9 → **Next**.

### 8. Confirmar senha
Inserir a senha criada anteriormente (se houver) ou deixar em branco → **Finish**.

A carteira foi recuperada com sucesso.

### 9. Acessar a aba de recebimento
Selecionar a aba **Receive**.

### 10. Preencher os dados do pedido de recebimento
No campo **Description**, inserir uma descrição, por exemplo:
```
Pagamento de aposta de futebol
```

### 11. Definir o valor solicitado
No campo **Requested amount**, inserir `1 mBTC` (1 milésimo de Bitcoin = 0,001 BTC).

### 12. Definir a validade do pedido
Manter o campo **Expiry** em `1 day`.

### 13. Gerar o endereço de recebimento
Clicar em **Onchain** para gerar o endereço de recebimento on-chain. O endereço é copiado automaticamente.

### 14. Copiar o endereço de recebimento
Clicar no campo do endereço de recebimento para copiá-lo. Este endereço pode ser enviado ao pagador.

### 15. Gerar e visualizar o QR Code
Clicar no ícone de **QR Code** para visualizar o QR Code correspondente ao pedido de recebimento.

### 16. Visualizar o QR Code e encerrar
Visualizar o QR Code gerado. Este pode ser escaneado por qualquer carteira Bitcoin para efetuar o pagamento de 0,001 BTC. Fechar o Electrum e o Terminal.

---

## Estrutura de um pedido de recebimento Bitcoin

| Campo | Valor do lab |
|-------|-------------|
| **Description** | Pagamento de aposta de futebol |
| **Amount** | 1 mBTC (0,001 BTC) |
| **Expiry** | 1 day |
| **Channel** | Onchain |
| **QR Code** | Endereço + valor codificados |

---

## Comparativo de unidades Bitcoin

| Unidade | Valor em BTC |
|---------|-------------|
| 1 BTC | 1,000000 BTC |
| 1 mBTC | 0,001 BTC |
| 1 μBTC (bit) | 0,000001 BTC |
| 1 satoshi | 0,00000001 BTC |

---

## Aprendizado
- A recuperação da carteira com a seed phrase demonstra que os fundos estão na blockchain, não no dispositivo
- O endereço Bitcoin on-chain é derivado da chave pública e identifica o destino do pagamento
- O QR Code facilita o pagamento móvel, codificando endereço e valor em formato escaneável
- A validade do pedido (`Expiry`) garante que endereços antigos não sejam reutilizados, preservando a privacidade
