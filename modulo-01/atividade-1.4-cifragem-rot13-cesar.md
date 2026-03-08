# Atividade 1.4 – Cifrando Texto com ROT13 e Cifra de César

## Objetivo
Explorar algoritmos clássicos de criptografia ROT13 e Cifra de César no Kali Linux para proteger a confidencialidade de mensagens.

## Conceitos

### Cifra de Substituição
Uma **cifra de substituição** substitui cada letra do texto original por outra letra, seguindo uma regra definida.

### ROT13
O **ROT13** (Rotate by 13) substitui cada letra pela letra 13 posições à frente no alfabeto. Por ser simétrico (26 letras / 2 = 13), cifrar e decifrar usam o mesmo processo.

```
A → N    N → A
B → O    O → B
...
```

### Cifra de César
A **Cifra de César** é uma cifra de substituição onde cada letra é deslocada um número fixo de posições no alfabeto. O número de posições é a chave da cifra.

---

## ROT13 na Prática

### Cifrar
```bash
echo "Hackers do bem - Fundamental" | rot13
# Unpxref qb orz - Shaqnzragny
```

### Decifrar (mesmo comando!)
```bash
echo "Unpxref qb orz - Shaqnzragny" | rot13
# Hackers do bem - Fundamental
```

---

## Cifra de César em Python

```python
def caesar_cipher(text, shift):
    result = ""
    for char in text:
        if char.isalpha():
            shift_amount = shift % 26
            if char.islower():
                shifted = ord(char) + shift_amount
                if shifted > ord("z"):
                    shifted -= 26
                result += chr(shifted)
            else:
                shifted = ord(char) + shift_amount
                if shifted > ord("Z"):
                    shifted -= 26
                result += chr(shifted)
        else:
            result += char
    return result

def main():
    text = input("Digite o texto a ser cifrado/descifrado: ")
    shift = int(input("Digite a quantidade de posições a ser deslocada: "))
    encrypted_text = caesar_cipher(text, shift)
    print("Texto cifrado/descifrado:", encrypted_text)

if __name__ == "__main__":
    main()
```

### Exemplo de uso

**Cifrando com deslocamento +10:**
```
Texto:  Hackers do bem - Fundamental
Cifrado: Rkmuobc ny low - Pexnkwoxdkv
```

**Decifrando com deslocamento -10:**
```
Texto:  Rkmuobc ny low - Pexnkwoxdkv
Decifrado: Hackers do bem - Fundamental
```

---

## Comparativo ROT13 vs Cifra de César

| | ROT13 | Cifra de César |
|---|---|---|
| **Deslocamento** | Fixo (13) | Variável (chave) |
| **Simétrico** | ✅ Sim | ❌ Não (precisa de -shift para decifrar) |
| **Segurança** | ❌ Muito baixa | ❌ Muito baixa |
| **Uso** | Ocultar spoilers/texto | Fins didáticos |

---

## Aprendizado
- ROT13 e Cifra de César são cifras históricas com fins didáticos
- Ambas são facilmente quebradas por análise de frequência
- Criptografia moderna usa algoritmos como AES, RSA e ChaCha20
- A chave de deslocamento deve ser mantida em segredo
