# APOSTILA COMPLETA DO MOSTRABOT 5em1 v5.2 BETA

## Guia Pratico para Professores - EJR Robotica Educacional

---

# INDICE

1. [Introducao ao MostraBot](#1-introducao-ao-mostrabot)
2. [Conhecendo o Hardware](#2-conhecendo-o-hardware)
3. [Sinais Visuais e Sonoros do Robo](#3-sinais-visuais-e-sonoros-do-robo)
4. [Primeiros Passos - Ligando o Robo](#4-primeiros-passos---ligando-o-robo)
5. [Modulo 1, 2 e 3 - Comandos de Movimento](#5-modulo-1-2-e-3---comandos-de-movimento)
6. [Comandos de LED (Olhos)](#6-comandos-de-led-olhos)
7. [Comandos de Servo (Garra)](#7-comandos-de-servo-garra)
8. [Comando de Buzzer (Som)](#8-comando-de-buzzer-som)
9. [Modulo 4 - Sensor Seguidor de Linha](#9-modulo-4---sensor-seguidor-de-linha)
10. [Modulo 4 - Sensor Ultrassonico](#10-modulo-4---sensor-ultrassonico)
11. [Estruturas de Controle (REPITA e SE)](#11-estruturas-de-controle-repita-e-se)
12. [Condicoes para o Comando SE](#12-condicoes-para-o-comando-se)
13. [Calibracao dos Motores](#13-calibracao-dos-motores)
14. [Configuracao de Velocidades](#14-configuracao-de-velocidades)
15. [Velocidades Personalizadas (EEPROM)](#15-velocidades-personalizadas-eeprom)
16. [Protecoes do Firmware](#16-protecoes-do-firmware)
17. [Tabela de Referencia Rapida](#17-tabela-de-referencia-rapida)
18. [Exemplos Praticos Completos](#18-exemplos-praticos-completos)
19. [Resolucao de Problemas](#19-resolucao-de-problemas)
20. [Glossario](#20-glossario)

---

# 1. Introducao ao MostraBot

## O que e o MostraBot?

O **MostraBot** e um robo educacional desenvolvido pela **EJR Robotica Educacional** especialmente para uso em salas de aula. Ele foi projetado para ensinar programacao e robotica de forma progressiva, do mais simples ao mais complexo.

## A Plataforma PJE Nuvem

O MostraBot funciona conectado a **Plataforma Jabuti Edu (PJE) Nuvem**. Isso significa que:

```
+------------------------------------------------------------------+
|                                                                  |
|   PROFESSOR/ALUNO                        MOSTRABOT               |
|   +--------------+                       +--------------+        |
|   |   Digita     |    Internet           |   Executa    |        |
|   |  comandos    | -------------------->|   comandos   |        |
|   |   na PJE     |                       |  no robo     |        |
|   +--------------+                       +--------------+        |
|         |                                     ^                  |
|         |                                     |                  |
|         v                                     |                  |
|   +--------------+                            |                  |
|   |  PJE NUVEM   |----------------------------+                  |
|   |  (servidor)  |       WiFi                                    |
|   +--------------+                                               |
|                                                                  |
+------------------------------------------------------------------+
```

O professor ou aluno digita comandos no navegador, e o robo executa automaticamente!

## Progressao Pedagogica

```
+----------------------------------------------------------------------------+
|                        JORNADA DE APRENDIZADO                              |
+----------------------------------------------------------------------------+
|                                                                            |
|  INICIANTE          INTERMEDIARIO           AVANCADO                       |
|  (Modulos 1-3)      (Modulo 4 Linha)        (Modulo 4 Ultra)               |
|                                                                            |
|  +---------+        +-------------+         +--------------+               |
|  | Controle|   -->  |  Seguidor   |   -->   |  Navegacao   |               |
|  | Direto  |        |  de Linha   |         |  Autonoma    |               |
|  +---------+        +-------------+         +--------------+               |
|                                                                            |
|  "Anda para         "Segue uma             "Explora o                      |
|   frente!"           linha preta"           ambiente sozinho"              |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 2. Conhecendo o Hardware

## O Microcontrolador

O MostraBot usa o **Wemos D1 Mini**, uma placa baseada no chip ESP8266 com WiFi integrado.

## Mapa dos Componentes

```
            +--------------------------------------------+
            |           FRENTE DO ROBO                   |
            |                                            |
            |         +----------------+                 |
            |         |     GARRA      |                 |
            |         | (Servo1+Servo2)|                 |
            |         +----------------+                 |
            |                |                           |
            |         +------+------+                    |
            |         |   SENSOR    |                    |
            |         | ULTRASSONICO|                    |
            |         |   (Olhos)   |                    |
            |         +-------------+                    |
            |                                            |
            |    LED           LED                       |
            |  Esquerdo      Direito                     |
            |    (D8)         (D3)                       |
            |                                            |
            +--------------------------------------------+
            |                                            |
            |   +-----+              +-----+             |
            |   |Motor|              |Motor|             |
            |   |  1  |              |  2  |             |
            |   |(ESQ)|              |(DIR)|             |
            |   +--+--+              +--+--+             |
            |      |                   |                 |
            |     ooo                 ooo                |
            |    RODAS               RODAS               |
            |                                            |
            +--------------------------------------------+
                        TRASEIRA DO ROBO
```

## Tabela de Pinagem Completa

| Componente | Pino | GPIO | Funcao |
|------------|------|------|--------|
| Motor 1 Frente | D0 | GPIO16 | Roda esquerda para frente |
| Motor 1 Re | D5 | GPIO14 | Roda esquerda para tras |
| Motor 2 Frente | D1 | GPIO5 | Roda direita para frente |
| Motor 2 Re | D4 | GPIO2 | Roda direita para tras |
| Servo 1 | D6 | GPIO12 | Garra (servo esquerdo) |
| Servo 2 | D7 | GPIO13 | Garra (servo direito) |
| D3 (Compartilhado) | D3 | GPIO0 | Ultra TRIG / Linha Dir / LED Dir |
| D8 (Compartilhado) | D8 | GPIO15 | Ultra ECHO / Linha Esq / LED Esq |
| LED/Buzzer | D2 | GPIO4 | LED externo e buzina |
| LED Interno | - | GPIO4 | Heartbeat de status |

## Regra de Hardware Importante

```
+----------------------------------------------------------------------------+
|                          ATENCAO PROFESSOR!                                |
+----------------------------------------------------------------------------+
|                                                                            |
|  Os pinos D3 e D8 sao COMPARTILHADOS e sao pinos de BOOT do ESP8266!       |
|                                                                            |
|  Isso significa que:                                                       |
|                                                                            |
|  1. Os sensores devem estar DESCONECTADOS ao ligar o robo                  |
|  2. Conecte os sensores SOMENTE depois do robo conectar ao WiFi            |
|  3. O robo sempre inicia em modo SENSOR OFF                                |
|                                                                            |
|  Na pratica: Ligue o robo, espere os 3 bipes, depois conecte os sensores   |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 3. Sinais Visuais e Sonoros do Robo

Esta e uma das secoes mais importantes! O robo "fala" com voce atraves de LEDs e sons. Aprenda a interpretar esses sinais.

## Sequencia de Inicializacao (Boot)

Quando voce liga o robo, acontece a seguinte sequencia:

```
+----------------------------------------------------------------------------+
|                        SEQUENCIA DE BOOT                                   |
+----------------------------------------------------------------------------+
|                                                                            |
|  PASSO 1: Liga o robo                                                      |
|  --------------------                                                      |
|           |                                                                |
|           v                                                                |
|  PASSO 2: LED pisca alternadamente (conectando WiFi)                       |
|           * o * o * o * o * o * o * o * o * o  (a cada 500ms)              |
|           |                                                                |
|           v                                                                |
|  PASSO 3: Conexao bem-sucedida?                                            |
|           |                                                                |
|     +-----+-----+                                                          |
|     |           |                                                          |
|     v           v                                                          |
|   SIM          NAO                                                         |
|     |           |                                                          |
|     v           v                                                          |
|  LED pisca    LED pisca                                                    |
|  3 vezes      10 vezes                                                     |
|  (lento)      (rapido)                                                     |
|  * * *        * * * * * * * * * *                                          |
|     |           |                                                          |
|     v           v                                                          |
|  PASSO 4: Sons de confirmacao (se conectou)                                |
|                                                                            |
|  BIPE 1: Tom baixo   (2000 Hz por 200ms)                                   |
|  BIPE 2: Tom medio   (2500 Hz por 200ms)                                   |
|  BIPE 3: Tom alto    (3000 Hz por 200ms)                                   |
|                                                                            |
|  Sons ascendentes = "Estou pronto!"                                        |
|     |                                                                      |
|     v                                                                      |
|  PASSO 5: Robo esta online e pronto para receber comandos                  |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Sinais Durante a Operacao Normal

### LED de Heartbeat (Batimento Cardiaco)

Quando o robo esta conectado e funcionando normalmente:

```
+----------------------------------------------------------------------------+
|                        HEARTBEAT - ROBO SAUDAVEL                           |
+----------------------------------------------------------------------------+
|                                                                            |
|  Tempo:    0s        3s        6s        9s        12s                     |
|            |         |         |         |          |                      |
|            v         v         v         v          v                      |
|  LED:     *         *         *         *          *                       |
|           ^         ^         ^         ^          ^                       |
|        100ms     100ms     100ms     100ms      100ms                      |
|                                                                            |
|  O LED pisca UMA VEZ a cada 3 segundos = Robo esta conectado e bem!        |
|                                                                            |
+----------------------------------------------------------------------------+
```

### LED Durante Execucao de Comandos

```
+----------------------------------------------------------------------------+
|                     LED DURANTE EXECUCAO DE COMANDOS                       |
+----------------------------------------------------------------------------+
|                                                                            |
|  Quando o robo RECEBE comandos da PJE:                                     |
|                                                                            |
|  +-------------------------------------------------------------+           |
|  |  LED ACENDE  ======================  LED APAGA              |           |
|  |      *                               o                      |           |
|  |      ^                               ^                      |           |
|  |  Recebeu                        Terminou                    |           |
|  |  comandos                       de executar                 |           |
|  +-------------------------------------------------------------+           |
|                                                                            |
|  Se o LED fica ACESO por muito tempo = comandos sendo executados           |
|  Se o LED fica APAGADO = aguardando novos comandos                         |
|                                                                            |
+----------------------------------------------------------------------------+
```

### LED Quando Perde Conexao WiFi

```
+----------------------------------------------------------------------------+
|                        PERDA DE CONEXAO                                    |
+----------------------------------------------------------------------------+
|                                                                            |
|  Se o WiFi cair durante a operacao:                                        |
|                                                                            |
|  LED pisca RAPIDO a cada 300ms:                                            |
|  * o * o * o * o * o * o * o * o * o * o * o * o * o * o                   |
|                                                                            |
|  Isso indica: "Estou tentando reconectar!"                                 |
|                                                                            |
|  Se falhar ao buscar comandos da PJE:                                      |
|  LED pisca 3 vezes rapido: * * *                                           |
|  (100ms ligado, 100ms desligado, 3 vezes)                                  |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Resumo Visual de Todos os Sinais

```
+-------------------------------------+--------------------------------------+
|           SINAL                     |           SIGNIFICADO                |
+-------------------------------------+--------------------------------------+
| LED pisca alternado (500ms)         | Conectando ao WiFi...                |
+-------------------------------------+--------------------------------------+
| LED pisca 3x lento                  | WiFi conectado com sucesso!          |
+-------------------------------------+--------------------------------------+
| LED pisca 10x rapido                | Falha na conexao WiFi                |
+-------------------------------------+--------------------------------------+
| 3 bipes ascendentes                 | Boot completo, pronto!               |
+-------------------------------------+--------------------------------------+
| LED pisca 1x a cada 3s              | Heartbeat - robo saudavel            |
+-------------------------------------+--------------------------------------+
| LED aceso continuo                  | Executando comandos                  |
+-------------------------------------+--------------------------------------+
| LED pisca rapido (300ms)            | Perdeu conexao, tentando reconectar  |
+-------------------------------------+--------------------------------------+
| LED pisca 3x rapido                 | Erro ao buscar comandos da PJE       |
+-------------------------------------+--------------------------------------+
```

---

# 4. Primeiros Passos - Ligando o Robo

## Checklist Antes de Ligar

```
+----------------------------------------------------------------------------+
|                        CHECKLIST PRE-OPERACAO                              |
+----------------------------------------------------------------------------+
|                                                                            |
|  [ ] Bateria carregada (ou fonte de alimentacao conectada)                 |
|  [ ] Sensores de linha DESCONECTADOS (se for usar)                         |
|  [ ] Sensor ultrassonico DESCONECTADO (se for usar)                        |
|  [ ] Area livre de obstaculos perigosos                                    |
|  [ ] Rede WiFi disponivel e funcionando                                    |
|  [ ] Acesso a plataforma PJE no navegador                                  |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Passo a Passo para Iniciar

```
+----------------------------------------------------------------------------+
|                       PROCEDIMENTO DE INICIALIZACAO                        |
+----------------------------------------------------------------------------+
|                                                                            |
|  PASSO 1: Ligue o robo                                                     |
|  -----------------------------------------------------------------         |
|  Pressione o botao de energia ou conecte a bateria.                        |
|  Observe o LED comecar a piscar.                                           |
|                                                                            |
|                                                                            |
|  PASSO 2: Aguarde a conexao WiFi                                           |
|  -----------------------------------------------------------------         |
|  O LED vai piscar alternadamente por ate 15 segundos.                      |
|  DICA: Cada pisca = 500ms. Entao 30 piscas = timeout.                      |
|                                                                            |
|                                                                            |
|  PASSO 3: Confirme a conexao                                               |
|  -----------------------------------------------------------------         |
|  SUCESSO: LED pisca 3 vezes lentamente + 3 bipes                           |
|  FALHA: LED pisca 10 vezes rapidamente (reinicie e tente novamente)        |
|                                                                            |
|                                                                            |
|  PASSO 4: Conecte os sensores (se necessario)                              |
|  -----------------------------------------------------------------         |
|  SOMENTE APOS o boot completo (3 bipes), conecte:                          |
|  - Sensores de linha nos pinos D3/D8, OU                                   |
|  - Sensor ultrassonico nos pinos D3/D8                                     |
|                                                                            |
|                                                                            |
|  PASSO 5: Acesse a PJE                                                     |
|  -----------------------------------------------------------------         |
|  Abra o navegador e acesse: pje.ejrrobotica.com.br                         |
|  Faca login e selecione o robo cadastrado.                                 |
|                                                                            |
|                                                                            |
|  PASSO 6: Teste basico                                                     |
|  -----------------------------------------------------------------         |
|  Digite um comando simples para testar:                                    |
|                                                                            |
|     pf 500                                                                 |
|                                                                            |
|  O robo deve andar para frente por 0.5 segundos.                           |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 5. Modulo 1, 2 e 3 - Comandos de Movimento

Os modulos 1, 2 e 3 da PJE ensinam comandos de movimento direto. O robo executa exatamente o que voce manda.

## Os 4 Comandos Basicos de Movimento

```
+----------------------------------------------------------------------------+
|                        COMANDOS DE MOVIMENTO                               |
+----------------------------------------------------------------------------+
|                                                                            |
|                          ^                                                 |
|                          |                                                 |
|                     +---------+                                            |
|                     |   PF    |  PF = Para Frente                          |
|                     | FRENTE  |                                            |
|                     +---------+                                            |
|                                                                            |
|            <--------------+-------------->>                                |
|       +---------+         |         +---------+                            |
|       |   PE    |         |         |   PD    |                            |
|       |ESQUERDA |         |         | DIREITA |                            |
|       +---------+         |         +---------+                            |
|                           |                                                |
|                     +---------+                                            |
|                     |   PT    |  PT = Para Tras                            |
|                     |  TRAS   |                                            |
|                     +---------+                                            |
|                           |                                                |
|                           v                                                |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Sintaxe dos Comandos

```
COMANDO <tempo_em_milissegundos>
```

### Exemplos Detalhados:

| Comando | Significado | Duracao |
|---------|-------------|---------|
| `pf 1000` | Anda para frente | 1 segundo (1000ms) |
| `pf 500` | Anda para frente | 0.5 segundos |
| `pf 2000` | Anda para frente | 2 segundos |
| `pt 1000` | Anda para tras | 1 segundo |
| `pd 200` | Gira para direita | 200ms (~90 graus) |
| `pe 200` | Gira para esquerda | 200ms (~90 graus) |
| `pf` | Anda para frente | 120ms (padrao) |

## Tabela de Conversao de Tempo

```
+----------------------------------------------------------------------------+
|                    TABELA DE REFERENCIA DE TEMPO                           |
+----------------------------------------------------------------------------+
|                                                                            |
|   Tempo desejado          Valor a usar          Exemplo                    |
|   --------------          ------------          -------                    |
|   0.1 segundos            100                   pf 100                     |
|   0.25 segundos           250                   pf 250                     |
|   0.5 segundos            500                   pf 500                     |
|   1 segundo               1000                  pf 1000                    |
|   1.5 segundos            1500                  pf 1500                    |
|   2 segundos              2000                  pf 2000                    |
|   5 segundos              5000                  pf 5000                    |
|                                                                            |
|   DICA: 1 segundo = 1000 milissegundos                                     |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Tabela de Angulos de Giro (Aproximados)

```
+----------------------------------------------------------------------------+
|                    REFERENCIA DE ANGULOS DE GIRO                           |
+----------------------------------------------------------------------------+
|                                                                            |
|   Angulo desejado         Tempo aproximado      Comando                    |
|   ---------------         ----------------      -------                    |
|   45 graus                ~110ms                pd 110 ou pe 110           |
|   90 graus                ~200-220ms            pd 200 ou pe 200           |
|   180 graus               ~440ms                pd 440 ou pe 440           |
|   360 graus               ~880ms                pd 880 ou pe 880           |
|                                                                            |
|   NOTA: Os tempos podem variar dependendo da bateria e superficie!         |
|         Ajuste conforme necessario atraves da calibracao.                  |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Exemplos Praticos de Movimento

### Exemplo 1: Quadrado
```
pf 1000
pd 200
pf 1000
pd 200
pf 1000
pd 200
pf 1000
pd 200
```

### Exemplo 2: Triangulo
```
pf 1000
pd 260
pf 1000
pd 260
pf 1000
pd 260
```

### Exemplo 3: Zigue-Zague
```
pf 500
pd 100
pf 500
pe 100
pf 500
pd 100
pf 500
```

---

# 6. Comandos de LED (Olhos)

Os LEDs do MostraBot funcionam como "olhos" que podem piscar para dar feedback visual.

## Os 3 Comandos de LED

```
+----------------------------------------------------------------------------+
|                          COMANDOS DE LED                                   |
+----------------------------------------------------------------------------+
|                                                                            |
|   +-----------------------------------------------------------+            |
|   |                  FRENTE DO ROBO                           |            |
|   |                                                           |            |
|   |        LED ESQUERDO           LED DIREITO                 |            |
|   |        (Pino D8)              (Pino D3)                   |            |
|   |            O                      O                       |            |
|   |            |                      |                       |            |
|   |            v                      v                       |            |
|   |         Comando               Comando                     |            |
|   |           LE                    LD                        |            |
|   |                                                           |            |
|   |              Ambos LEDs = LDE                             |            |
|   |                                                           |            |
|   +-----------------------------------------------------------+            |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Sintaxe dos Comandos LED

| Comando | Descricao | Pino |
|---------|-----------|------|
| `ld <ms>` | Pisca LED direito por X milissegundos | D3 |
| `le <ms>` | Pisca LED esquerdo por X milissegundos | D8 |
| `lde <ms>` | Pisca AMBOS os LEDs por X milissegundos | D3 + D8 |

## Como os LEDs Funcionam

```
+----------------------------------------------------------------------------+
|                      COMPORTAMENTO DO LED                                  |
+----------------------------------------------------------------------------+
|                                                                            |
|   Comando: ld 1000                                                         |
|                                                                            |
|   Tempo:    0ms                              1000ms                        |
|             |                                  |                           |
|             v                                  v                           |
|   LED:      *=================================o                            |
|             ^                                  ^                           |
|          LIGA                               DESLIGA                        |
|                                                                            |
|   O LED permanece ACESO durante todo o tempo especificado,                 |
|   depois APAGA automaticamente.                                            |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Exemplos com LEDs

### Piscar alternadamente:
```
ld 300
le 300
ld 300
le 300
```

### Piscar os dois juntos:
```
lde 500
lde 500
lde 500
```

### Movimento com feedback visual:
```
pf 1000
ld 200
pt 1000
le 200
```

## Valores Padrao

Se voce nao especificar o tempo, o LED pisca por **500ms**:

```
ld          # Pisca LED direito por 500ms
le          # Pisca LED esquerdo por 500ms
lde         # Pisca ambos por 500ms
```

## IMPORTANTE: Conflito com Sensores

```
+----------------------------------------------------------------------------+
|                          ATENCAO!                                          |
+----------------------------------------------------------------------------+
|                                                                            |
|  Os LEDs D3 e D8 compartilham os pinos com os sensores!                    |
|                                                                            |
|  Antes de usar comandos de LED, certifique-se de que:                      |
|                                                                            |
|     sensor off                                                             |
|                                                                            |
|  Se o sensor estiver ativo (linha ou ultra), os comandos                   |
|  LD, LE e LDE nao funcionarao corretamente!                                |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 7. Comandos de Servo (Garra)

O MostraBot possui uma garra controlada por dois servos que trabalham em conjunto.

## Anatomia da Garra

```
+----------------------------------------------------------------------------+
|                          SISTEMA DE GARRA                                  |
+----------------------------------------------------------------------------+
|                                                                            |
|                    +---------------------+                                 |
|                    |      GARRA          |                                 |
|                    |    (aberta)         |                                 |
|                    |  /            \     |                                 |
|                    | /              \    |                                 |
|                    |/                \   |                                 |
|                    +--------+--------+   |                                 |
|                    |SERVO1  | SERVO2 |   |                                 |
|                    | (D6)   |  (D7)  |   |                                 |
|                    +--------+--------+   |                                 |
|                                          |                                 |
|                    SC = Subir Garra (170 graus) = ABERTA                   |
|                    BC = Baixar Garra (10 graus) = FECHADA                  |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Comandos Basicos da Garra

| Comando | Descricao | Angulo do Servo |
|---------|-----------|-----------------|
| `bc` | Baixar/Fechar garra | 10 graus |
| `sc` | Subir/Abrir garra | 170 graus |
| `servo1 <angulo>` | Controle fino servo 1 | 0 a 180 graus |
| `servo2 <angulo>` | Controle fino servo 2 | 0 a 180 graus |

## Exemplos de Uso da Garra

### Pegar um objeto:
```
sc              # Abre a garra
pf 500          # Aproxima do objeto
bc              # Fecha a garra (pega o objeto)
pt 500          # Recua com o objeto
```

### Soltar um objeto:
```
pf 500          # Aproxima do destino
sc              # Abre a garra (solta o objeto)
pt 500          # Recua
bc              # Fecha a garra novamente
```

## Controle Fino dos Servos

Para movimentos mais precisos, use os comandos `servo1` e `servo2`:

```
servo1 90       # Posiciona servo 1 em 90 graus
servo2 90       # Posiciona servo 2 em 90 graus
servo1 45       # Posiciona servo 1 em 45 graus
servo2 135      # Posiciona servo 2 em 135 graus
```

---

# 8. Comando de Buzzer (Som)

O buzzer permite que o robo emita sons para feedback ou diversao.

## Comando BU

```
bu <tempo_em_ms>
```

| Parametro | Descricao | Padrao |
|-----------|-----------|--------|
| tempo | Duracao do bipe em milissegundos | 800ms |

## Frequencia do Som

O buzzer emite um tom de **2000 Hz** (2 kHz) - um tom medio-agudo, facil de ouvir.

## Exemplos de Uso

### Bipe simples:
```
bu 500          # Bipe de 0.5 segundos
```

### Bipe de confirmacao:
```
pf 1000
bu 200          # Bipe curto apos movimento
pt 1000
bu 200
```

### Padrao de alerta:
```
bu 100
bu 100
bu 100
bu 500
```

### Sem especificar tempo:
```
bu              # Bipe de 800ms (padrao)
```

---

# 9. Modulo 4 - Sensor Seguidor de Linha

O seguidor de linha permite que o robo siga autonomamente uma linha preta sobre fundo branco.

## Como Funciona o Sensor

```
+----------------------------------------------------------------------------+
|                    PRINCIPIO DO SEGUIDOR DE LINHA                          |
+----------------------------------------------------------------------------+
|                                                                            |
|                      SENSORES IR                                           |
|                    +-----+-----+                                           |
|                    | ESQ | DIR |                                           |
|                    | D8  | D3  |                                           |
|                    +--+--+--+--+                                           |
|                       |     |                                              |
|                       v     v                                              |
|   ==============+===========+=================                             |
|   BRANCO        |   LINHA   |    BRANCO                                    |
|                 |   PRETA   |                                              |
|   ==============+===========+=================                             |
|                                                                            |
|   A linha deve passar ENTRE os dois sensores!                              |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Logica de Deteccao

```
+----------------------------------------------------------------------------+
|                     LOGICA DOS SENSORES IR                                 |
+----------------------------------------------------------------------------+
|                                                                            |
|   SENSOR VE BRANCO (HIGH)    |    SENSOR VE LINHA/PRETO (LOW)              |
|   -------------------------  |    -------------------------------          |
|   -> Reflete luz             |    -> Absorve luz                           |
|   -> Retorna HIGH            |    -> Retorna LOW                           |
|                              |                                             |
|                              |                                             |
|   SITUACAO 1: Centralizado   |    SITUACAO 2: Desviou p/ esquerda          |
|   +-----+-----+              |    +-----+-----+                            |
|   | ESQ | DIR |              |    | ESQ | DIR |                            |
|   |HIGH |HIGH |              |    | LOW |HIGH |                            |
|   +--+--+--+--+              |    +--+--+--+--+                            |
|      |     |                 |       |     |                               |
|   ===+=====+===              |    ===+=====|===                            |
|   -> Segue reto              |    -> Corrige para esquerda                 |
|                              |                                             |
|                              |                                             |
|   SITUACAO 3: Desviou p/ dir |    SITUACAO 4: Cruzamento/Fim               |
|   +-----+-----+              |    +-----+-----+                            |
|   | ESQ | DIR |              |    | ESQ | DIR |                            |
|   |HIGH | LOW |              |    | LOW | LOW |                            |
|   +--+--+--+--+              |    +--+--+--+--+                            |
|      |     |                 |       |     |                               |
|   ===|=====+===              |    ===+=====+===                            |
|   -> Corrige para direita    |    -> Reduz velocidade                      |
|                              |                                             |
+----------------------------------------------------------------------------+
```

## Ativando o Modo Linha

### Passo 1: Ativar o sensor
```
sensor linha
```

### Passo 2: Executar o seguidor
```
seguir-linha <tempo_ms>
```

## Exemplo Completo de Seguidor de Linha

```
sensor linha
repita 50
  seguir-linha 100
fim repita
sensor off
bu 500
```

**Explicacao:**
1. `sensor linha` - Ativa os sensores IR nos pinos D3/D8
2. `repita 50` - Repete 50 vezes o comando interno
3. `seguir-linha 100` - Executa um passo de 100ms de seguir linha
4. `fim repita` - Fecha o loop
5. `sensor off` - Desativa os sensores
6. `bu 500` - Bipe de finalizacao

## Dicas para a Pista

```
+----------------------------------------------------------------------------+
|                    DICAS PARA MONTAR A PISTA                               |
+----------------------------------------------------------------------------+
|                                                                            |
|  [OK] Use fita isolante preta sobre cartolina branca                       |
|  [OK] Largura da linha: 2-3 cm                                             |
|  [OK] Curvas suaves (nao muito fechadas)                                   |
|  [OK] Superficie fosca (evite brilho)                                      |
|  [OK] Ambiente com luz uniforme                                            |
|                                                                            |
|  [X] Evite linhas muito finas (< 1.5cm)                                    |
|  [X] Evite curvas em angulo reto (90 graus)                                |
|  [X] Evite superficies reflexivas                                          |
|  [X] Evite luz solar direta                                                |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 10. Modulo 4 - Sensor Ultrassonico

O sensor ultrassonico permite que o robo "enxergue" obstaculos e navegue autonomamente.

## Como Funciona o Sensor Ultrassonico

```
+----------------------------------------------------------------------------+
|                  PRINCIPIO DO SENSOR ULTRASSONICO                          |
+----------------------------------------------------------------------------+
|                                                                            |
|                         SENSOR HC-SR04                                     |
|                    +-------------------+                                   |
|                    |    (o)     (o)    |                                   |
|                    |   TRIG    ECHO    |                                   |
|                    |    D3      D8     |                                   |
|                    +-------------------+                                   |
|                            |                                               |
|                            |  Envia onda sonora                            |
|                            |  =================>  +----------+             |
|                            |                      |          |             |
|                            |  Recebe eco          | OBSTACULO|             |
|                            |  <=================  |          |             |
|                            |                      +----------+             |
|                                                                            |
|   Tempo entre envio e retorno = Distancia ao obstaculo                     |
|                                                                            |
|   Alcance: 2cm a 300cm (3 metros)                                          |
|   Precisao: +/- 3mm                                                        |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Modos de Navegacao Ultrassonica

O MostraBot v5.2 Beta possui **5 modos** de navegacao ultrassonica:

```
+-----------------------------+----------------------------------------------+
|         COMANDO             |              DESCRICAO                       |
+-----------------------------+----------------------------------------------+
| seguir-ultra-agil           | Modo RAPIDO - ideal para criancas 5-7 anos   |
|                             | Explora rapidamente, pode raspar em quinas   |
|                             | A crianca pode ajudar a centralizar          |
+-----------------------------+----------------------------------------------+
| seguir-ultra-inteligente    | Modo COGNITIVO - demonstra inteligencia      |
|                             | Mais lento, mas desvia sem tocar obstaculos  |
|                             | Mapeia o ambiente e evita ciclos             |
+-----------------------------+----------------------------------------------+
| seguir-ultra-memoria        | Modo com MEMORIA de direcao                  |
|                             | Lembra da ultima direcao tomada              |
|                             | Util para padroes previsiveis                |
+-----------------------------+----------------------------------------------+
| seguir-ultra-hibrido        | Modo HIBRIDO - combina agil + inteligente    |
|                             | Comeca agil, fica inteligente em dificuldade |
|                             | Retorna ao agil quando resolve               |
+-----------------------------+----------------------------------------------+
| seguir-ultra                | ALIAS para modo agil (compatibilidade)       |
|                             | Mesmo comportamento do seguir-ultra-agil     |
+-----------------------------+----------------------------------------------+
```

## Detalhamento dos Modos

### 1. Modo Agil (seguir-ultra-agil)

```
+----------------------------------------------------------------------------+
|                          MODO AGIL                                         |
+----------------------------------------------------------------------------+
|                                                                            |
|  PUBLICO: Criancas de 5 a 7 anos                                           |
|                                                                            |
|  COMPORTAMENTO:                                                            |
|  +------------------------------------------------------------+            |
|  |                                                            |            |
|  |   1. Anda para frente ate detectar obstaculo (<=15cm)      |            |
|  |   2. Para e escaneia 270 graus (frente, direita, esquerda) |            |
|  |   3. Escolhe direcao com mais espaco livre                 |            |
|  |   4. Valida com micro-avanco antes de seguir               |            |
|  |   5. Repete o processo                                     |            |
|  |                                                            |            |
|  +------------------------------------------------------------+            |
|                                                                            |
|  CARACTERISTICAS:                                                          |
|  [OK] Rapido para encontrar caminhos                                       |
|  [OK] Aceita ajuda manual (crianca pode centralizar)                       |
|  [OK] Nao volta por onde veio (origem bloqueada apos 15 ciclos)            |
|  [X]  Pode raspar em quinas                                                |
|  [X]  Nao e tao "inteligente" para desvios complexos                       |
|                                                                            |
+----------------------------------------------------------------------------+
```

### 2. Modo Inteligente (seguir-ultra-inteligente)

```
+----------------------------------------------------------------------------+
|                       MODO INTELIGENTE                                     |
+----------------------------------------------------------------------------+
|                                                                            |
|  PUBLICO: Demonstracoes de capacidade cognitiva                            |
|                                                                            |
|  COMPORTAMENTO:                                                            |
|  +------------------------------------------------------------+            |
|  |                                                            |            |
|  |   1. Faz leitura PRECISA parado (mediana de 5 amostras)    |            |
|  |   2. Giro EXPLORATORIO coletando tendencias                |            |
|  |   3. Avanca em passos INCREMENTAIS (150ms cada)            |            |
|  |   4. Verificacao LATERAL a cada 8 avancos                  |            |
|  |   5. Detecta CICLOS (padroes repetidos 3x)                 |            |
|  |   6. Recuperacao automatica se ficar preso                 |            |
|  |                                                            |            |
|  +------------------------------------------------------------+            |
|                                                                            |
|  CARACTERISTICAS:                                                          |
|  [OK] Nao toca nos obstaculos (desvia antes)                               |
|  [OK] Mapeia o ambiente mentalmente                                        |
|  [OK] Detecta e evita loops/ciclos                                         |
|  [OK] Mais preciso em ambientes complexos                                  |
|  [X]  Mais lento que o modo agil                                           |
|  [X]  Pode parecer "indeciso" em certos momentos                           |
|                                                                            |
+----------------------------------------------------------------------------+
```

### 3. Modo Memoria (seguir-ultra-memoria)

```
+----------------------------------------------------------------------------+
|                        MODO MEMORIA                                        |
+----------------------------------------------------------------------------+
|                                                                            |
|  COMPORTAMENTO:                                                            |
|  +------------------------------------------------------------+            |
|  |                                                            |            |
|  |   1. Lembra da ultima direcao tomada (esq/dir)             |            |
|  |   2. Ao encontrar obstaculo, prefere a mesma direcao       |            |
|  |   3. Se a direcao falhar, troca a preferencia              |            |
|  |   4. Aguarda confirmacao antes de mudar definitivamente    |            |
|  |                                                            |            |
|  +------------------------------------------------------------+            |
|                                                                            |
|  UTIL PARA: Ambientes com padroes previsiveis                              |
|                                                                            |
+----------------------------------------------------------------------------+
```

### 4. Modo Hibrido (seguir-ultra-hibrido)

```
+----------------------------------------------------------------------------+
|                         MODO HIBRIDO                                       |
+----------------------------------------------------------------------------+
|                                                                            |
|  COMPORTAMENTO:                                                            |
|  +------------------------------------------------------------+            |
|  |                                                            |            |
|  |   1. INICIA em modo AGIL (rapido)                          |            |
|  |                                                            |            |
|  |   2. Se encontrar 3+ obstaculos seguidos:                  |            |
|  |      -> MUDA para modo INTELIGENTE                         |            |
|  |                                                            |            |
|  |   3. Apos resolver a situacao (5 ciclos):                  |            |
|  |      -> VOLTA para modo AGIL                               |            |
|  |                                                            |            |
|  |   4. Repete conforme necessario                            |            |
|  |                                                            |            |
|  +------------------------------------------------------------+            |
|                                                                            |
|  MELHOR DOS DOIS MUNDOS: Rapido quando pode, cuidadoso quando precisa      |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Ativando o Modo Ultrassonico

### Passo 1: Ativar o sensor
```
sensor ultra
```

### Passo 2: Escolher e executar o modo
```
seguir-ultra-agil 5000        # 5 segundos em modo agil
seguir-ultra-inteligente 5000 # 5 segundos em modo inteligente
seguir-ultra-hibrido 5000     # 5 segundos em modo hibrido
```

## Exemplos Completos

### Exemplo 1: Exploracao Agil (5-7 anos)
```
sensor ultra
repita 10
  seguir-ultra-agil 3000
fim repita
sensor off
bu 1000
```

### Exemplo 2: Demonstracao Inteligente
```
sensor ultra
repita 5
  seguir-ultra-inteligente 5000
fim repita
sensor off
lde 1000
bu 500
```

### Exemplo 3: Navegacao Hibrida
```
sensor ultra
repita 20
  seguir-ultra-hibrido 2000
fim repita
parar-ultra
sensor off
bu 300
bu 300
bu 300
```

## Comando para Parar a Navegacao

```
parar-ultra
```

Este comando interrompe imediatamente qualquer navegacao ultrassonica em andamento.

## Reacoes Visuais Durante Navegacao

Durante a navegacao ultrassonica, o robo da feedback visual e sonoro:

```
+----------------------------------------------------------------------------+
|              FEEDBACK DURANTE NAVEGACAO ULTRASSONICA                       |
+----------------------------------------------------------------------------+
|                                                                            |
|  EVENTO                           |  REACAO DO ROBO                        |
|  ---------------------------------|----------------------------------------|
|  Detectou obstaculo proximo       |  Para, gira, escaneia                  |
|  Encontrou caminho livre          |  Avanca suavemente                     |
|  Validando direcao                |  Micro-avanco (80ms)                   |
|  Direcao rejeitada                |  Micro-recuo (100ms) + tenta outra     |
|  Todas direcoes bloqueadas        |  Giro 180 graus (meia-volta)           |
|  Ciclo detectado                  |  Tenta rota alternativa                |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 11. Estruturas de Controle (REPITA e SE)

As estruturas de controle permitem criar programas mais complexos e inteligentes.

## Comando REPITA (Loop)

O comando `repita` executa um bloco de comandos varias vezes.

### Sintaxe:
```
repita <numero_de_vezes>
  <comandos>
fim repita
```

### Exemplo Basico:
```
repita 4
  pf 500
  pd 200
fim repita
```
Este codigo faz o robo andar em quadrado!

### Exemplo com LEDs:
```
repita 5
  ld 200
  le 200
fim repita
```
Os LEDs piscam alternadamente 5 vezes.

### Niveis de Aninhamento

Voce pode colocar um `repita` dentro de outro (ate 3 niveis):

```
repita 3
  pf 500
  repita 2
    pd 100
    ld 100
  fim repita
fim repita
```

```
+----------------------------------------------------------------------------+
|                     LIMITE DE ANINHAMENTO                                  |
+----------------------------------------------------------------------------+
|                                                                            |
|  NIVEL 1:  repita 5                                                        |
|              |                                                             |
|  NIVEL 2:    repita 3                                                      |
|                |                                                           |
|  NIVEL 3:      repita 2        <- MAXIMO PERMITIDO                         |
|                  |                                                         |
|                  comandos                                                  |
|                fim repita                                                  |
|              fim repita                                                    |
|            fim repita                                                      |
|                                                                            |
|  ATENCAO: Nao e possivel ter um 4o nivel de repita!                        |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Comando SE (Condicional)

O comando `se` executa comandos somente se uma condicao for verdadeira.

### Sintaxe:
```
se <condicao>
  <comandos>
fim se
```

### Exemplo com Sensor de Linha:
```
sensor linha
se sensord
  pd 150
fim se
sensor off
```
Se o sensor direito detectar a linha, gira para direita.

### Exemplo com Ultrassonico:
```
sensor ultra
se dist 20
  pt 300
  pd 200
fim se
sensor off
```
Se houver obstaculo a menos de 20cm, recua e gira.

## Combinando REPITA e SE

```
sensor linha
repita 30
  seguir-linha 100
  se perdeu
    pt 200
    pd 200
  fim se
fim repita
sensor off
```

---

# 12. Condicoes para o Comando SE

## Condicoes de Sensor de Linha

```
+-----------------+--------------------------------------------------------------+
|    CONDICAO     |                     SIGNIFICADO                              |
+-----------------+--------------------------------------------------------------+
|    sensord      |  Sensor DIREITO esta NA LINHA (detectou preto)               |
+-----------------+--------------------------------------------------------------+
|    nsensord     |  Sensor DIREITO esta NO BRANCO (nao detectou linha)          |
+-----------------+--------------------------------------------------------------+
|    sensore      |  Sensor ESQUERDO esta NA LINHA (detectou preto)              |
+-----------------+--------------------------------------------------------------+
|    nsensore     |  Sensor ESQUERDO esta NO BRANCO (nao detectou linha)         |
+-----------------+--------------------------------------------------------------+
|    linhaok      |  AMBOS sensores no branco (robo centralizado)                |
+-----------------+--------------------------------------------------------------+
|    perdeu       |  AMBOS sensores na linha (perdeu a linha ou cruzamento)      |
+-----------------+--------------------------------------------------------------+
```

### Diagrama Visual das Condicoes de Linha:

```
         LINHAOK                    SENSORE                    SENSORD
      (centralizado)            (saiu p/ direita)          (saiu p/ esquerda)

    +-----+-----+              +-----+-----+              +-----+-----+
    |BRANCO|BRANCO|              |PRETO|BRANCO|              |BRANCO|PRETO|
    | ESQ | DIR |              | ESQ | DIR |              | ESQ | DIR |
    +--+--+--+--+              +--+--+--+--+              +--+--+--+--+
       |     |                    |     |                    |     |
    ===|=====|===              ===+=====|===              ===|=====+===
       ||   ||                    ||   ||                    ||   ||
       =======                    =======                    =======
         linha                      linha                      linha


         PERDEU                    NSENSORD                   NSENSORE
      (cruzamento)              (direito no branco)        (esquerdo no branco)

    +-----+-----+              +-----+-----+              +-----+-----+
    |PRETO|PRETO|              |  ?  |BRANCO|              |BRANCO|  ?  |
    | ESQ | DIR |              | ESQ | DIR |              | ESQ | DIR |
    +--+--+--+--+              +--+--+--+--+              +--+--+--+--+
       |     |                    |     |                    |     |
    ===+=====+===              ===|=====|===              ===|=====|===
       |=====|                    ||   ||                    ||   ||
       =======                    =======                    =======
     cruzamento                   (verifica so dir)        (verifica so esq)
```

## Condicoes de Sensor Ultrassonico

```
+-----------------+--------------------------------------------------------------+
|    CONDICAO     |                     SIGNIFICADO                              |
+-----------------+--------------------------------------------------------------+
|    dist X       |  Verdadeiro se distancia <= X centimetros                    |
|                 |  (obstaculo proximo)                                         |
+-----------------+--------------------------------------------------------------+
|    distm X      |  Verdadeiro se distancia >= X centimetros                    |
|                 |  (caminho livre)                                             |
+-----------------+--------------------------------------------------------------+
```

### Exemplos de Condicoes Ultrassonicas:

| Condicao | Significado | Uso tipico |
|----------|-------------|------------|
| `dist 10` | Obstaculo a 10cm ou menos | Emergencia! Recuar imediatamente |
| `dist 20` | Obstaculo a 20cm ou menos | Alerta, preparar para desviar |
| `dist 30` | Obstaculo a 30cm ou menos | Reduzir velocidade |
| `distm 50` | Caminho livre de pelo menos 50cm | Pode acelerar |
| `distm 100` | Caminho livre de pelo menos 1 metro | Via livre total |

### Diagrama Visual das Condicoes Ultrassonicas:

```
                    SENSOR
                      |
                      v
    +------------------------------------------------------------+
    |                                                            |
    |   0cm   10cm   20cm   30cm   40cm   50cm   ...   300cm     |
    |    +-----+-----+-----+-----+-----+----------------+        |
    |                                                            |
    |    <---- dist 20 ---->                                     |
    |    (verdadeiro se <=20cm)                                  |
    |                                                            |
    |                        <------ distm 30 ----------------->|
    |                        (verdadeiro se >=30cm)              |
    |                                                            |
    +------------------------------------------------------------+
```

---

# 13. Calibracao dos Motores

A calibracao compensa diferencas entre os dois motores, fazendo o robo andar reto.

## Por que Calibrar?

```
+----------------------------------------------------------------------------+
|                      PROBLEMA: MOTORES DESIGUAIS                           |
+----------------------------------------------------------------------------+
|                                                                            |
|   Cada motor tem caracteristicas ligeiramente diferentes:                  |
|   - Atrito interno                                                         |
|   - Eficiencia                                                             |
|   - Desgaste                                                               |
|                                                                            |
|   SEM CALIBRACAO:                      COM CALIBRACAO:                     |
|                                                                            |
|       /                                        |                           |
|      /                                         |                           |
|     /    Robo desvia para                      |  Robo anda reto!          |
|    /     um lado                               |                           |
|   o                                            o                           |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Comandos de Calibracao

| Comando | Descricao |
|---------|-----------|
| `atrasar_mote <valor>` | Reduz forca do motor ESQUERDO |
| `atrasar_motd <valor>` | Reduz forca do motor DIREITO |

### Valores Tipicos:
- Valor entre 10 e 100
- Comece com 30 e ajuste conforme necessario
- A calibracao e salva na EEPROM (memoria permanente)

## Procedimento de Calibracao Passo a Passo

```
+----------------------------------------------------------------------------+
|                     PROCEDIMENTO DE CALIBRACAO                             |
+----------------------------------------------------------------------------+
|                                                                            |
|  PASSO 1: Teste inicial                                                    |
|  -----------------------------------------------------------------         |
|  Coloque o robo em linha reta e execute:                                   |
|                                                                            |
|     pf 3000                                                                |
|                                                                            |
|  Observe para qual lado o robo desvia.                                     |
|                                                                            |
|                                                                            |
|  PASSO 2: Identifique o motor mais forte                                   |
|  -----------------------------------------------------------------         |
|                                                                            |
|     / Desvia para DIREITA?                                                 |
|       -> Motor ESQUERDO e mais forte                                       |
|       -> Use: atrasar_mote                                                 |
|                                                                            |
|     \ Desvia para ESQUERDA?                                                |
|       -> Motor DIREITO e mais forte                                        |
|       -> Use: atrasar_motd                                                 |
|                                                                            |
|                                                                            |
|  PASSO 3: Aplique correcao                                                 |
|  -----------------------------------------------------------------         |
|  Comece com valor 30:                                                      |
|                                                                            |
|     atrasar_mote 30    # Se desvia para direita                            |
|     ou                                                                     |
|     atrasar_motd 30    # Se desvia para esquerda                           |
|                                                                            |
|                                                                            |
|  PASSO 4: Teste novamente                                                  |
|  -----------------------------------------------------------------         |
|                                                                            |
|     pf 3000                                                                |
|                                                                            |
|  Ainda desvia? Aumente o valor. Desvia para o outro lado? Diminua.         |
|                                                                            |
|                                                                            |
|  PASSO 5: Calibracao salva automaticamente!                                |
|  -----------------------------------------------------------------         |
|  A calibracao e armazenada na EEPROM e persiste mesmo apos desligar.       |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Como a Calibracao Funciona Internamente

```
+----------------------------------------------------------------------------+
|                    MECANICA DA CALIBRACAO                                  |
+----------------------------------------------------------------------------+
|                                                                            |
|   O firmware armazena duas variaveis: DIF1 e DIF2                          |
|                                                                            |
|   - Se atrasar_mote foi usado:                                             |
|     DIF1 = valor_calibracao                                                |
|     DIF2 = 0                                                               |
|                                                                            |
|   - Se atrasar_motd foi usado:                                             |
|     DIF1 = 0                                                               |
|     DIF2 = valor_calibracao                                                |
|                                                                            |
|   Ao mover os motores:                                                     |
|     Motor 1 recebe: PWM - DIF1                                             |
|     Motor 2 recebe: PWM - DIF2                                             |
|                                                                            |
|   Exemplo com atrasar_mote 30 e PWM 750:                                   |
|     Motor 1 (esq): 750 - 30 = 720  (mais lento)                            |
|     Motor 2 (dir): 750 - 0  = 750  (normal)                                |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Armazenamento na EEPROM

A calibracao usa os enderecos 97, 98 e 99 da EEPROM:

| Endereco | Conteudo | Descricao |
|----------|----------|-----------|
| 97 | 'M' | Marcador de calibracao valida |
| 98 | 0 ou 1 | 0 = esquerdo atrasado, 1 = direito atrasado |
| 99 | valor | Valor da diferenca (0-200) |

---

# 14. Configuracao de Velocidades

O MostraBot permite ajustar a velocidade de tres contextos diferentes.

## Tres Velocidades Independentes

```
+----------------------------------------------------------------------------+
|                    SISTEMA DE VELOCIDADES                                  |
+----------------------------------------------------------------------------+
|                                                                            |
|   +-----------------+  +-----------------+  +-----------------+            |
|   |    pwmPadrao    |  |    pwmLinha     |  |    pwmUltra     |            |
|   |   (Movimento)   |  |  (Seguir Linha) |  | (Ultrassonico)  |            |
|   +--------+--------+  +--------+--------+  +--------+--------+            |
|            |                    |                    |                     |
|            v                    v                    v                     |
|     Comandos PF/PT        seguir-linha        seguir-ultra-*               |
|     PD/PE                                                                  |
|                                                                            |
|   VALOR PADRAO: 750       VALOR PADRAO: 750    VALOR PADRAO: 750           |
|   (para todos)                                                             |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Comandos de Velocidade Temporaria

Estes comandos mudam a velocidade **apenas durante a sessao atual**. Ao reiniciar o robo, volta ao padrao.

| Comando | Descricao | Afeta |
|---------|-----------|-------|
| `define_vel <valor>` | Define velocidade geral | PF, PT, PD, PE |
| `define_vel_linha <valor>` | Define velocidade da linha | seguir-linha |
| `define_vel_ultra <valor>` | Define velocidade do ultra | seguir-ultra-* |

### Exemplos:
```
define_vel 900           # Movimento mais rapido
pf 1000                  # Agora anda mais rapido

define_vel 500           # Movimento mais lento
pf 1000                  # Agora anda mais devagar

define_vel_linha 600     # Linha mais lenta (mais preciso)
define_vel_ultra 800     # Ultra mais rapido
```

## Limites de Velocidade (PWM)

```
+----------------------------------------------------------------------------+
|                       ESCALA DE VELOCIDADE PWM                             |
+----------------------------------------------------------------------------+
|                                                                            |
|    280         500         750         900        1023                     |
|     |           |           |           |           |                      |
|     +-----------+-----------+-----------+-----------+                      |
|     v           v           v           v           v                      |
|   MINIMO     LENTO       MEDIO       RAPIDO      MAXIMO                    |
|                         (padrao)                                           |
|                                                                            |
|   PWM_MINIMO = 280    (abaixo disso o motor nao gira)                      |
|   PWM_MAX = 1023      (potencia maxima)                                    |
|                                                                            |
|   Valores fora dessa faixa sao automaticamente ajustados!                  |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 15. Velocidades Personalizadas (EEPROM)

Para salvar velocidades permanentemente (que persistem apos reiniciar), use os comandos de personalizacao.

## Comandos de Velocidade Permanente

| Comando | Descricao | Armazena em |
|---------|-----------|-------------|
| `define_personalizado <valor>` | Salva velocidade geral | EEPROM |
| `define_personalizado_linha <valor>` | Salva velocidade de linha | EEPROM |
| `define_personalizado_ultra <valor>` | Salva velocidade do ultra | EEPROM |
| `reset_personalizado` | Restaura TODOS os padroes | EEPROM |

### Exemplos:
```
define_personalizado 800          # Salva velocidade geral
define_personalizado_linha 600    # Salva velocidade da linha
define_personalizado_ultra 700    # Salva velocidade do ultra
```

## Comparacao: Temporario vs Permanente

```
+----------------------------------------------------------------------------+
|              TEMPORARIO vs PERMANENTE                                      |
+----------------------------------------------------------------------------+
|                                                                            |
|   TEMPORARIO (define_vel)           |   PERMANENTE (define_personalizado)  |
|   ---------------------------------  ------------------------------------- |
|                                     |                                      |
|   [OK] Efeito imediato              |   [OK] Efeito imediato               |
|   [X]  Perdido ao reiniciar         |   [OK] Salvo na EEPROM               |
|   [OK] Bom para testes              |   [OK] Persiste apos reiniciar       |
|   [OK] Nao desgasta EEPROM          |   [!]  Limitar escritas (vida util)  |
|                                     |                                      |
|   USE PARA:                         |   USE PARA:                          |
|   - Experimentar velocidades        |   - Configuracao final do robo       |
|   - Ajustes durante uma aula        |   - Apos encontrar valores ideais    |
|                                     |                                      |
+----------------------------------------------------------------------------+
```

## Armazenamento na EEPROM

| Endereco | Conteudo |
|----------|----------|
| 100 | 'P' = Marcador de personalizado valido |
| 101-102 | Velocidade padrao (2 bytes) |
| 103-104 | Velocidade linha (2 bytes) |
| 105-106 | Velocidade ultra (2 bytes) |

## Restaurando Padroes

Para voltar todas as velocidades aos valores de fabrica:

```
reset_personalizado
```

Isso restaura:
- pwmPadrao = 750
- pwmLinha = 750
- pwmUltra = 750

---

# 16. Protecoes do Firmware

O MostraBot v5.2 Beta implementa **12 protecoes** para garantir operacao segura.

## Visao Geral das Protecoes

```
+----------------------------------------------------------------------------+
|                    12 PROTECOES DO FIRMWARE                                |
+----------------------------------------------------------------------------+
|                                                                            |
|   1. Back EMF Protection       |   7. Median Filter                        |
|      Protege contra tensao     |      Filtra ruido do ultrassom            |
|      reversa dos motores       |      (5 amostras, mediana)                |
|                                |                                           |
|   2. Kickstart System          |   8. EEPROM Validation                    |
|      Pulso inicial para        |      Valida dados antes de usar           |
|      vencer inercia            |      (ranges, marcadores)                 |
|                                |                                           |
|   3. PWM Clamping              |   9. Shoot-Through Prevention             |
|      Limita PWM entre          |      Todos pinos LOW antes                |
|      280 e 1023                |      de mudar direcao                     |
|                                |                                           |
|   4. Watchdog Feed             |  10. Stack Overflow Prevention            |
|      yield() evita reset       |      MAX_REPITAS = 3                      |
|      por timeout               |      MAX_COMANDOS = 60                    |
|                                |                                           |
|   5. Boot Pin Protection       |  11. Default Safe Values                  |
|      D3/D8 iniciam como        |      Valores padrao seguros               |
|      OUTPUT LOW                |      para todos parametros                |
|                                |                                           |
|   6. HTTP Timeout              |  12. Obstacle Detection Hysteresis        |
|      2 segundos maximo         |      3 leituras consecutivas              |
|      para requisicoes          |      antes de reagir                      |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Detalhamento das Protecoes

### 1. Back EMF Protection (Forca Contra-Eletromotriz)

```
+----------------------------------------------------------------------------+
|                         BACK EMF PROTECTION                                |
+----------------------------------------------------------------------------+
|                                                                            |
|   PROBLEMA: Quando um motor para, ele gera tensao reversa                  |
|             que pode danificar o circuito.                                 |
|                                                                            |
|   SOLUCAO:                                                                 |
|   - Atraso obrigatorio de 100ms apos parar motores                         |
|   - Todos os 4 pinos setados LOW antes de mudar direcao                    |
|                                                                            |
|   CODIGO:                                                                  |
|   #define ATRASO_MOVIMENTO 100                                             |
|                                                                            |
|   void pararMotoresDireto() {                                              |
|     digitalWrite(MOT1_A, LOW);                                             |
|     digitalWrite(MOT1_B, LOW);                                             |
|     digitalWrite(MOT2_A, LOW);                                             |
|     digitalWrite(MOT2_B, LOW);                                             |
|     delay(ATRASO_MOVIMENTO);  // 100ms de espera                           |
|     yield();                                                               |
|   }                                                                        |
|                                                                            |
+----------------------------------------------------------------------------+
```

### 2. Kickstart System (Anti-Inercia)

```
+----------------------------------------------------------------------------+
|                          KICKSTART SYSTEM                                  |
+----------------------------------------------------------------------------+
|                                                                            |
|   PROBLEMA: Motores precisam de mais energia para comecar a girar          |
|             (inercia, atrito estatico).                                    |
|                                                                            |
|   SOLUCAO:                                                                 |
|   - Pulso inicial de PWM 900 por 40ms                                      |
|   - So aplica se movimento > 40ms                                          |
|   - Depois reduz para PWM normal                                           |
|                                                                            |
|   CONSTANTES:                                                              |
|   #define PWM_KICKSTART    900                                             |
|   #define TEMPO_KICKSTART  40                                              |
|   #define KICKSTART_ATIVO  true                                            |
|                                                                            |
|   DIAGRAMA:                                                                |
|                                                                            |
|   PWM |                                                                    |
|  900 -+--*========*                                                        |
|       |           |                                                        |
|  750 -+           *================================*                       |
|       |                                            |                       |
|       +--------------------------------------------+---------> tempo       |
|          40ms       movimento normal                                       |
|         (boost)                                                            |
|                                                                            |
+----------------------------------------------------------------------------+
```

### 3. PWM Clamping (Limitacao de PWM)

```
+----------------------------------------------------------------------------+
|                           PWM CLAMPING                                     |
+----------------------------------------------------------------------------+
|                                                                            |
|   PROBLEMA: PWM muito baixo = motor trava                                  |
|             PWM muito alto = sobrecorrente                                 |
|                                                                            |
|   SOLUCAO: Funcao clampPWM() em TODOS os analogWrite()                     |
|                                                                            |
|   int clampPWM(int valor) {                                                |
|     if (valor < PWM_MINIMO) return PWM_MINIMO;  // 280                     |
|     if (valor > PWM_MAX) return PWM_MAX;        // 1023                    |
|     return valor;                                                          |
|   }                                                                        |
|                                                                            |
|   Uso: analogWrite(MOT1_A, clampPWM(pwm + DIF1));                          |
|                                                                            |
+----------------------------------------------------------------------------+
```

### 4. Watchdog Feed

```
+----------------------------------------------------------------------------+
|                          WATCHDOG FEED                                     |
+----------------------------------------------------------------------------+
|                                                                            |
|   PROBLEMA: O ESP8266 tem um watchdog que reinicia o chip                  |
|             se o codigo "travar" por muito tempo.                          |
|                                                                            |
|   SOLUCAO: Chamar yield() regularmente                                     |
|   - 26+ chamadas distribuidas no codigo                                    |
|   - Em loops de sensor                                                     |
|   - Em movimentos                                                          |
|   - Em comunicacao WiFi                                                    |
|                                                                            |
|   yield(); // "Estou vivo, nao reinicie!"                                  |
|                                                                            |
+----------------------------------------------------------------------------+
```

### 7. Median Filter (Filtro de Mediana)

```
+----------------------------------------------------------------------------+
|                          MEDIAN FILTER                                     |
+----------------------------------------------------------------------------+
|                                                                            |
|   PROBLEMA: Sensor ultrassonico pode dar leituras erraticas                |
|             (ruido, interferencia).                                        |
|                                                                            |
|   SOLUCAO: Mediana de multiplas amostras                                   |
|                                                                            |
|   COMO FUNCIONA:                                                           |
|   1. Coleta 5-7 leituras                                                   |
|   2. Ordena do menor para o maior                                          |
|   3. Retorna o valor do meio (mediana)                                     |
|                                                                            |
|   EXEMPLO:                                                                 |
|   Leituras: 25, 150, 28, 27, 999, 26, 24                                   |
|   Ordenado: 24, 25, 26, 27, 28, 150, 999                                   |
|   Mediana: 27 (valor central)                                              |
|                                                                            |
|   Os valores absurdos (150, 999) sao ignorados!                            |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 17. Tabela de Referencia Rapida

## Todos os Comandos em Uma Pagina

```
+----------------------------------------------------------------------------+
|                    REFERENCIA RAPIDA DE COMANDOS                           |
+----------------------------------------------------------------------------+
|                                                                            |
|  MOVIMENTO                          |  SENSORES                            |
|  ---------------------------------  |  ---------------------------------   |
|  pf <ms>    Para frente             |  sensor linha    Ativa IR            |
|  pt <ms>    Para tras               |  sensor ultra    Ativa ultrassom     |
|  pd <ms>    Gira direita            |  sensor off      Desativa sensores   |
|  pe <ms>    Gira esquerda           |                                      |
|                                     |                                      |
|  LEDS                               |  SEGUIR LINHA                        |
|  ---------------------------------  |  ---------------------------------   |
|  ld <ms>    LED direito             |  seguir-linha <ms>                   |
|  le <ms>    LED esquerdo            |                                      |
|  lde <ms>   Ambos LEDs              |                                      |
|                                     |                                      |
|  GARRA                              |  SEGUIR ULTRA                        |
|  ---------------------------------  |  ---------------------------------   |
|  bc         Baixar/Fechar garra     |  seguir-ultra-agil <ms>              |
|  sc         Subir/Abrir garra       |  seguir-ultra-inteligente <ms>       |
|  servo1 <angulo>                    |  seguir-ultra-memoria <ms>           |
|  servo2 <angulo>                    |  seguir-ultra-hibrido <ms>           |
|                                     |  seguir-ultra <ms>  (=agil)          |
|  SOM                                |  parar-ultra                         |
|  ---------------------------------  |                                      |
|  bu <ms>    Buzzer/Bipe             |                                      |
|                                     |                                      |
|  CALIBRACAO                         |  VELOCIDADES                         |
|  ---------------------------------  |  ---------------------------------   |
|  atrasar_mote <valor>               |  define_vel <valor>                  |
|  atrasar_motd <valor>               |  define_vel_linha <valor>            |
|                                     |  define_vel_ultra <valor>            |
|                                     |  define_personalizado <valor>        |
|  CONTROLE                           |  define_personalizado_linha <valor>  |
|  ---------------------------------  |  define_personalizado_ultra <valor>  |
|  repita <N>                         |  reset_personalizado                 |
|  fim repita                         |                                      |
|  se <condicao>                      |                                      |
|  fim se                             |                                      |
|                                     |                                      |
|  CONDICOES LINHA                    |  CONDICOES ULTRA                     |
|  ---------------------------------  |  ---------------------------------   |
|  sensord     Direito na linha       |  dist X     Distancia <= X cm        |
|  nsensord    Direito no branco      |  distm X    Distancia >= X cm        |
|  sensore     Esquerdo na linha      |                                      |
|  nsensore    Esquerdo no branco     |                                      |
|  linhaok     Ambos no branco        |                                      |
|  perdeu      Ambos na linha         |                                      |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Valores Padrao

| Parametro | Valor Padrao | Descricao |
|-----------|--------------|-----------|
| PWM_PADRAO_DEFAULT | 750 | Velocidade padrao de movimento |
| PWM_LINHA_DEFAULT | 750 | Velocidade padrao seguidor de linha |
| PWM_ULTRA_DEFAULT | 750 | Velocidade padrao navegacao ultra |
| PWM_MINIMO | 280 | Velocidade minima permitida |
| PWM_MAX | 1023 | Velocidade maxima permitida |
| PASSO_PADRAO | 120ms | Duracao padrao de movimento |
| LED padrao | 500ms | Duracao padrao de LED |
| Buzzer padrao | 800ms | Duracao padrao de bipe |
| DIST_OBSTACULO | 14cm | Distancia para detectar obstaculo |
| TEMPO_GIRO_90 | 220ms | Tempo para girar 90 graus |
| TEMPO_GIRO_180 | 440ms | Tempo para girar 180 graus |

---

# 18. Exemplos Praticos Completos

## Exemplo 1: Primeiro Programa (Iniciante)

**Objetivo:** Fazer o robo andar em quadrado e piscar LEDs em cada canto.

```
pf 1000
ld 300
pd 220
pf 1000
le 300
pd 220
pf 1000
ld 300
pd 220
pf 1000
le 300
pd 220
bu 1000
```

## Exemplo 2: Seguidor de Linha Basico

**Objetivo:** Seguir uma linha preta por 30 segundos.

```
sensor linha
repita 300
  seguir-linha 100
fim repita
sensor off
bu 500
lde 1000
```

## Exemplo 3: Seguidor de Linha com Deteccao

**Objetivo:** Seguir linha e sinalizar quando detectar cruzamento.

```
sensor linha
repita 200
  seguir-linha 50
  se perdeu
    bu 200
    lde 200
  fim se
fim repita
sensor off
```

## Exemplo 4: Explorador Ultrassonico Agil

**Objetivo:** Explorar ambiente em modo agil para criancas.

```
sensor ultra
repita 30
  seguir-ultra-agil 2000
fim repita
parar-ultra
sensor off
lde 500
bu 300
bu 300
bu 300
```

## Exemplo 5: Demonstracao Inteligente

**Objetivo:** Mostrar capacidade cognitiva do robo.

```
sensor ultra
repita 10
  seguir-ultra-inteligente 5000
fim repita
parar-ultra
sensor off
bu 500
```

## Exemplo 6: Coletor de Objetos

**Objetivo:** Ir ate um objeto, pegar e voltar.

```
sc
pf 1500
bc
pt 1500
sc
bu 500
```

## Exemplo 7: Programa Completo com Calibracao

**Objetivo:** Programa completo demonstrando varias funcionalidades.

```
# Definir velocidade mais lenta
define_vel 600

# Fazer um quadrado
repita 4
  pf 1000
  ld 200
  pd 220
fim repita

# Bipe de conclusao
bu 500

# Pegar objeto
pf 500
bc
pt 500

# Restaurar velocidade
define_vel 750

# Andar rapido
pf 2000

# Soltar objeto
sc

# Finalizacao
lde 1000
bu 300
bu 300
```

---

# 19. Resolucao de Problemas

## Problema: Robo nao conecta ao WiFi

```
+----------------------------------------------------------------------------+
|  SINTOMA: LED pisca 10 vezes rapido (sem os 3 bipes)                       |
+----------------------------------------------------------------------------+
|  SOLUCOES:                                                                 |
|  1. Verifique se o WiFi esta funcionando                                   |
|  2. Verifique se esta no alcance do roteador                               |
|  3. Verifique se a senha WiFi esta correta no firmware                     |
|  4. Reinicie o roteador e o robo                                           |
+----------------------------------------------------------------------------+
```

## Problema: Robo nao responde aos comandos

```
+----------------------------------------------------------------------------+
|  SINTOMA: Comandos enviados mas robo nao executa                           |
+----------------------------------------------------------------------------+
|  SOLUCOES:                                                                 |
|  1. Verifique se o heartbeat esta pulsando (1x a cada 3s)                  |
|  2. Verifique se esta logado no robo correto na PJE                        |
|  3. Aguarde e tente novamente (pode haver delay de rede)                   |
|  4. Reinicie o robo                                                        |
+----------------------------------------------------------------------------+
```

## Problema: Robo desvia para um lado

```
+----------------------------------------------------------------------------+
|  SINTOMA: Ao mandar andar reto, robo desvia para esquerda ou direita       |
+----------------------------------------------------------------------------+
|  SOLUCAO: Calibracao dos motores                                           |
|                                                                            |
|  Se desvia para DIREITA: atrasar_mote 30                                   |
|  Se desvia para ESQUERDA: atrasar_motd 30                                  |
|                                                                            |
|  Ajuste o valor (10-100) ate andar reto.                                   |
+----------------------------------------------------------------------------+
```

## Problema: LEDs nao acendem

```
+----------------------------------------------------------------------------+
|  SINTOMA: Comandos LD/LE/LDE nao funcionam                                 |
+----------------------------------------------------------------------------+
|  SOLUCAO:                                                                  |
|  1. Certifique-se que sensor esta OFF                                      |
|     sensor off                                                             |
|  2. Depois use os comandos de LED                                          |
|     ld 500                                                                 |
|                                                                            |
|  Os pinos D3/D8 sao compartilhados com sensores!                           |
+----------------------------------------------------------------------------+
```

## Problema: Sensor de linha nao funciona

```
+----------------------------------------------------------------------------+
|  SINTOMA: Robo nao segue a linha ou comportamento erratico                 |
+----------------------------------------------------------------------------+
|  SOLUCOES:                                                                 |
|  1. Verifique se ativou o sensor: sensor linha                             |
|  2. Verifique conexao fisica dos sensores IR                               |
|  3. Verifique contraste: linha preta em fundo branco claro                 |
|  4. Ajuste altura dos sensores (1-2cm do chao)                             |
|  5. Evite luz solar direta                                                 |
+----------------------------------------------------------------------------+
```

## Problema: Sensor ultrassonico nao detecta obstaculos

```
+----------------------------------------------------------------------------+
|  SINTOMA: Robo bate nos obstaculos em modo ultra                           |
+----------------------------------------------------------------------------+
|  SOLUCOES:                                                                 |
|  1. Verifique se ativou o sensor: sensor ultra                             |
|  2. Verifique conexao do HC-SR04 (TRIG=D3, ECHO=D8)                        |
|  3. Superficies muito lisas/anguladas podem nao refletir                   |
|  4. Use modo inteligente para melhor deteccao                              |
|  5. Reduza velocidade: define_vel_ultra 500                                |
+----------------------------------------------------------------------------+
```

## Problema: Garra nao funciona

```
+----------------------------------------------------------------------------+
|  SINTOMA: Comandos BC/SC nao movem a garra                                 |
+----------------------------------------------------------------------------+
|  SOLUCOES:                                                                 |
|  1. Verifique conexao dos servos (Servo1=D6, Servo2=D7)                    |
|  2. Verifique alimentacao (servos precisam de corrente)                    |
|  3. Teste com servo1 90 e servo2 90                                        |
|  4. Se nao mover, pode ser problema mecanico/eletrico                      |
+----------------------------------------------------------------------------+
```

## Problema: Robo reinicia sozinho

```
+----------------------------------------------------------------------------+
|  SINTOMA: Robo reinicia durante operacao (3 bipes novamente)               |
+----------------------------------------------------------------------------+
|  CAUSAS POSSIVEIS:                                                         |
|  1. Bateria fraca - recarregue                                             |
|  2. Sobrecarga - reduza velocidade                                         |
|  3. Curto-circuito - verifique conexoes                                    |
|  4. Sensores conectados durante boot - reconecte apos 3 bipes              |
+----------------------------------------------------------------------------+
```

---

# 20. Glossario

| Termo | Definicao |
|-------|-----------|
| **Back EMF** | Forca contra-eletromotriz gerada pelos motores ao parar |
| **Boot** | Processo de inicializacao quando o robo liga |
| **Buzzer** | Componente que emite sons/bipes |
| **EEPROM** | Memoria nao-volatil que mantem dados apos desligar |
| **ESP8266** | Microcontrolador com WiFi usado no Wemos D1 Mini |
| **GPIO** | General Purpose Input/Output - pinos programaveis |
| **HC-SR04** | Modelo do sensor ultrassonico |
| **Heartbeat** | Pulso periodico do LED indicando que robo esta operacional |
| **IR** | Infravermelho - tecnologia dos sensores de linha |
| **Kickstart** | Pulso inicial de alta potencia para vencer inercia |
| **LED** | Light Emitting Diode - os "olhos" do robo |
| **Mediana** | Valor central de uma serie ordenada de numeros |
| **PJE** | Plataforma Jabuti Edu - sistema de controle online |
| **PWM** | Pulse Width Modulation - controle de potencia dos motores |
| **Servo** | Motor de posicionamento preciso (usado na garra) |
| **Ultrassonico** | Sensor que mede distancia por ondas sonoras |
| **Watchdog** | Sistema que reinicia o chip se travar |
| **WiFi** | Conexao sem fio para internet |
| **Wemos D1 Mini** | Placa de desenvolvimento usada no MostraBot |
| **yield()** | Funcao que "alimenta" o watchdog |

---

# Informacoes de Contato

**EJR Robotica Educacional**
- Email: eloirjr@gmail.com
- Plataforma: pje.ejrrobotica.com.br

---

*Versao do Documento: 2.0*
*Versao do Firmware: MostraBot 5em1 v5.2 Beta*
*Data: Fevereiro 2026*

---

**Esta apostila foi elaborada para auxiliar professores no uso do MostraBot em sala de aula. Qualquer duvida, entre em contato com a EJR Robotica Educacional.**
