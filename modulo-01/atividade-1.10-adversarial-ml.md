# Atividade 1.10 – Adversarial Machine Learning

## Objetivo
Visualizar e entender ataques de Adversarial Machine Learning utilizando a ferramenta adversarial.js.

## Ferramenta
**adversarial.js** → https://kennysong.github.io/adversarial.js/

Demonstra ataques adversariais em tempo real no navegador, sem necessidade de instalação.

---

## Conceito

### Machine Learning
O **aprendizado de máquina** é uma área da IA onde modelos são treinados para reconhecer padrões em dados como imagens, textos e sons. Um modelo treinado para reconhecimento de placas de trânsito, por exemplo, aprende a identificar corretamente placas de STOP, YIELD, etc.

### Adversarial Machine Learning
O **Adversarial Machine Learning** explora vulnerabilidades em modelos de aprendizado de máquina. Atacantes introduzem **perturbações imperceptíveis** nos dados de entrada para enganar o modelo, fazendo-o classificar incorretamente.

```
Imagem Original       +   Perturbação          =   Imagem Adversarial
STOP (99% confiança)      (ruído invisível)        YIELD (95% confiança)
       ↓                                                    ↓
  Classificação CORRETA                          Classificação ERRADA
```

---

## Passo a Passo

### 1. Acessar a ferramenta
```
https://kennysong.github.io/adversarial.js/
```

### 2. Selecionar o modelo
Escolher **GTSRB (street sign recognition)** — reconhecimento de placas de trânsito.

### 3. Testar a imagem original
- Na coluna **"Original Image"**, clicar em **RUN NEURAL NETWORK**
- Resultado: classificação **correta** (ex: STOP)

### 4. Gerar a imagem adversarial
- Na coluna **"Adversarial Image"**, clicar em **GENERATE**
- Aguardar alguns segundos
- Uma imagem visualmente idêntica à original é gerada

### 5. Testar a imagem adversarial
- Na coluna **"Adversarial Image"**, clicar em **RUN NEURAL NETWORK**
- Resultado: classificação **incorreta** (ex: YIELD em vez de STOP)
- **O sistema de IA foi enganado!**

---

## Como Funciona a Perturbação

O algoritmo calcula matematicamente o menor ruído possível que, quando adicionado à imagem original, faz o modelo errar na classificação. Esse ruído é:

- **Imperceptível** para o olho humano
- **Eficaz** contra o modelo de aprendizado de máquina
- **Intencional** — calculado para maximizar o erro do modelo

---

## Impactos em Domínios Críticos

| Domínio | Risco |
|---------|-------|
| Veículos autônomos | Ignorar placas de STOP ou sinalização |
| Reconhecimento facial | Bypass de sistemas de autenticação |
| Diagnóstico médico por IA | Laudos incorretos em exames |
| Filtros de spam | E-mails maliciosos passam despercebidos |
| Sistemas de vigilância | Evasão de detecção por câmeras |

---

## Defesas Contra Ataques Adversariais

| Técnica | Descrição |
|---------|-----------|
| **Treinamento adversarial** | Incluir exemplos adversariais no treinamento |
| **Detecção de anomalias** | Identificar entradas com perturbações suspeitas |
| **Ensemble de modelos** | Usar múltiplos modelos em conjunto |
| **Certificação de robustez** | Verificar matematicamente os limites do modelo |

---

## Aprendizado
- Modelos de IA podem ser enganados por perturbações imperceptíveis para humanos
- O ataque é especialmente perigoso em sistemas críticos de segurança
- A robustez adversarial é uma área ativa de pesquisa em IA
- Sistemas críticos não devem depender exclusivamente de modelos de ML sem validação adicional
