# 🤖 GUIA DE COMANDOS - REPTO MOSTRABOT

**Guia prático para educadores e alunos**

---

## 🎯 Introdução

Este guia ensina como programar o REPTO MOSTRABOT usando a **Plataforma Jabuti Edu Nuvem (PJE)**.

O robô executa comandos que você digita na plataforma, permitindo criar desde movimentos simples até comportamentos autônomos complexos.

---

## 📚 Índice

1. [Comandos Básicos de Movimento](#-1-comandos-básicos-de-movimento)
2. [Comandos dos LEDs dos Olhos](#-2-comandos-dos-leds-dos-olhos)
3. [Comandos de Som](#-3-comandos-de-som)
4. [Comandos de Garra/Braço](#-4-comandos-de-garrabraço)
5. [Comandos de Sensores](#-5-comandos-de-sensores)
6. [Estruturas de Controle](#-6-estruturas-de-controle)
7. [Programas Autônomos](#-7-programas-autônomos)
8. [Exemplos Progressivos](#-8-exemplos-progressivos)
9. [Dicas e Melhores Práticas](#-9-dicas-e-melhores-práticas)

---

## 🏃 1. Comandos Básicos de Movimento

### Comandos de Direção

| Comando | Exemplo | Descrição |
|---------|---------|-----------|
| `pf` | `pf 1000` | Para frente por 1000ms (1 segundo) |
| `pt` | `pt 500` | Para trás por 500ms |
| `pd` | `pd 300` | Vira para direita por 300ms |
| `pe` | `pe 300` | Vira para esquerda por 300ms |

### ⏱️ Tempo nos Comandos

- **Com tempo**: `pf 500` = move por 500 milissegundos
- **Sem tempo**: `pf` = move por 120ms (tempo padrão)
- **1 segundo = 1000ms**

### 🎯 Exemplos Básicos

```
# Quadrado simples
pf 1000
pd 300
pf 1000
pd 300
pf 1000
pd 300
pf 1000
pd 300
```

```
# Ida e volta
pf 2000
pt 2000
```

---

## 👀 2. Comandos dos LEDs dos Olhos

### Comandos de LED

| Comando | Exemplo | Descrição |
|---------|---------|-----------|
| `ld` | `ld 500` | Pisca LED direito por 500ms |
| `le` | `le 300` | Pisca LED esquerdo por 300ms |
| `lde` | `lde 1000` | Pisca ambos LEDs por 1000ms |

### ✨ Comportamento dos LEDs

- **Liga automaticamente** → aguarda tempo → **desliga automaticamente**
- **Sem tempo**: `ld` = pisca por 500ms (padrão)
- **LEDs são os "olhos" do robô**

### 🎯 Exemplos de LEDs

```
# Piscar alternado
ld 200
le 200
ld 200
le 200
```

```
# Semáforo de LEDs
lde 1000    # Pisca ambos por 1s
ld 500      # Só direito por 500ms
le 500      # Só esquerdo por 500ms
```

```
# Feedback visual durante movimento
pf 1000
ld 100      # Pisca direito (indicando sucesso)
pd 500
le 100      # Pisca esquerdo (mudou direção)
```

---

## 🔊 3. Comandos de Som

### Comando de Buzzer

| Comando | Exemplo | Descrição |
|---------|---------|-----------|
| `bu` | `bu 500` | Toca buzzer por 500ms |

### 🎯 Exemplos de Som

```
# Sinal de alerta
bu 100
bu 100
bu 500
```

```
# Som com movimento
pf 1000
bu 200      # Bipe de confirmação
```

---

## 🦾 4. Comandos de Garra/Braço

### Comandos de Servo

| Comando | Exemplo | Descrição |
|---------|---------|-----------|
| `bc` | `bc` | Baixa garra (10 graus) |
| `sc` | `sc` | Sobe garra (170 graus) |
| `servo1` | `servo1 90` | Move servo 1 para 90 graus |
| `servo2` | `servo2 45` | Move servo 2 para 45 graus |

### 🎯 Exemplos de Garra

```
# Pegar objeto
bc          # Baixa garra
pf 500      # Aproxima do objeto
sc          # Fecha/sobe garra
pt 500      # Volta com objeto
```

```
# Servo personalizado
servo1 0    # Posição inicial
servo1 90   # Meio caminho
servo1 180  # Posição final
```

---

## 👁️ 5. Comandos de Sensores

### Configuração de Sensores

| Comando | Exemplo | Descrição |
|---------|---------|-----------|
| `sensor linha` | `sensor linha` | Ativa sensores de linha |
| `sensor ultra` | `sensor ultra` | Ativa sensor ultrassônico |
| `sensor off` | `sensor off` | Desativa sensores (prepara LEDs) |

### 🚨 Importante sobre Sensores

- **Apenas UM tipo** de sensor ativo por vez
- **`sensor off`** é necessário para usar LEDs dos olhos
- **Sensores compartilham pinos** D3 e D8 com os LEDs

### 🎯 Exemplos de Sensores

```
# Testar sensor de linha
sensor linha
# Agora robô pode "ver" a linha

sensor off
ld 500      # Agora pode usar LEDs
```

```
# Testar sensor ultrassônico
sensor ultra
# Agora robô pode "ver" obstáculos

sensor off
le 300      # Volta para LEDs
```

---

## 🔄 6. Estruturas de Controle

### Repetição (REPITA)

```
repita 5
  pf 500
  pd 200
fim repita
```

### Condições (SE)

#### Para Sensor de Linha

| Condição | Significado |
|----------|-------------|
| `sensord` | Sensor direito detecta linha |
| `sensore` | Sensor esquerdo detecta linha |
| `linhaok` | Ambos sensores no branco (caminho livre) |
| `perdeu` | Ambos sensores na linha (cruzamento) |

#### Para Sensor Ultrassônico

| Condição | Significado |
|----------|-------------|
| `dist 30` | Distância menor ou igual a 30cm |
| `distm 50` | Distância maior ou igual a 50cm |

### 🎯 Exemplos de Estruturas

```
# Repetir movimento
repita 4
  pf 1000
  pd 300
fim repita
# Faz um quadrado!
```

```
# Piscar LEDs em sequência
repita 3
  ld 200
  le 200
  lde 300
fim repita
```

---

## 🤖 7. Programas Autônomos

### Seguidor de Linha

```
sensor linha
repita 20
  seguir-linha 100
  se sensord
    ld 100        # LED indica curva direita
    pd 150
  fim se
  se sensore
    le 100        # LED indica curva esquerda
    pe 150
  fim se
fim repita
sensor off
bu 500            # Som de fim
```

### Desvio de Obstáculos

```
sensor ultra
repita 15
  seguir-ultra 200
  se dist 20
    pt 300        # Recua
    pd 400        # Vira
    lde 200       # Pisca LEDs (obstáculo detectado)
  fim se
fim repita
sensor off
```

---

## 📈 8. Exemplos Progressivos

### 🟢 **Nível 1 - Iniciante**

#### Primeiro Programa
```
pf 1000
bu 200
```

#### Movimento Básico
```
pf 1000
pd 300
pf 1000
pt 1000
```

### 🟡 **Nível 2 - Intermediário**

#### Com LEDs
```
pf 1000
ld 300
pd 500
le 300
pf 1000
lde 500
```

#### Com Repetição
```
repita 3
  pf 800
  ld 200
  pd 400
  le 200
fim repita
bu 1000
```

### 🔴 **Nível 3 - Avançado**

#### Seguidor com Feedback Visual
```
sensor linha
repita 10
  seguir-linha 150
  se linhaok
    lde 100      # Verde = linha OK
  fim se
  se perdeu
    repita 3
      ld 100
      le 100     # Vermelho = perdeu linha
    fim repita
  fim se
fim repita
sensor off
```

#### Robô Reativo
```
# Combina movimento, sensores e feedback
sensor ultra
repita 20
  seguir-ultra 100

  se dist 15
    # Obstáculo muito próximo
    pt 400
    lde 300      # Alerta visual
    bu 200       # Alerta sonoro
    pd 600
  fim se

  se distm 50
    # Caminho livre - acelera
    pf 200
    ld 100       # LED direito = acelerando
  fim se
fim repita
sensor off
```

---

## 💡 9. Dicas e Melhores Práticas

### ⚡ **Dicas de Movimento**

- **Ajustar tempos**: Teste diferentes valores para encontrar o ideal
- **Movimentos suaves**: Use tempos menores para precisão
- **Calibração**: Use `atrasar_mote` ou `atrasar_motd` se robô não anda reto

### 👀 **Dicas de LEDs**

- **Feedback visual**: Use LEDs para indicar estados do robô
- **Duração adequada**: 100-300ms para piscadas rápidas, 500-1000ms para avisos
- **Sempre `sensor off`**: Necesário antes de usar LEDs

### 🎯 **Dicas de Programação**

- **Teste pequeno**: Comece com programas simples
- **Use comentários**: `# isto é um comentário` (se a PJE suportar)
- **Progredindo**: Comece com movimento → LEDs → sensores → autonomia

### 🚨 **Evite Problemas**

- **Não misture sensores**: Só um tipo ativo por vez
- **Tempos muito longos**: Evite mais de 5000ms sem `yield`
- **LEDs sem sensor off**: LEDs não funcionam com sensores ativos

### 🔧 **Solução de Problemas**

| Problema | Solução |
|----------|---------|
| LEDs não funcionam | Execute `sensor off` primeiro |
| Robô não anda reto | Use comandos de calibração |
| Sensor não responde | Verifique `sensor linha` ou `sensor ultra` |
| Programa travou | Reduza tempos dos comandos |

---

## 🏆 Desafios Práticos

### 🎯 **Desafio 1**: Faça o robô desenhar um triângulo
### 🎯 **Desafio 2**: Crie uma sequência de LEDs que conte de 1 a 5
### 🎯 **Desafio 3**: Programe um robô que segue linha e pisca LEDs nas curvas
### 🎯 **Desafio 4**: Faça um "robô reativo" que combina sensores, movimento e feedback

---

## 📞 Contato

**Dúvidas sobre programação do REPTO MOSTRABOT?**

📧 **Email**: eloirjr@gmail.com
🏢 **EJR Robótica Educacional**

---

*Versão 1.0 - Janeiro 2026*