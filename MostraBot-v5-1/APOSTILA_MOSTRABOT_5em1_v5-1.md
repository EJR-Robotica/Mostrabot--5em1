# APOSTILA COMPLETA DO MOSTRABOT 5em1 v5.1

## Guia Pratico para Professores - EJR Robotica Educacional

---

```
################################################################################
#                                                                              #
#                    LEIA ANTES DE USAR O MOSTRABOT!                           #
#                                                                              #
################################################################################
#                                                                              #
#   Esta apostila contem informacoes ESSENCIAIS para o bom funcionamento       #
#   do seu robo. Antes de iniciar as aulas, leia OBRIGATORIAMENTE:             #
#                                                                              #
#   >>> Secao 18: REGRAS DE OURO - As 5 regras fundamentais                    #
#   >>> Secao 19: Pausas Entre Movimentos - Estabilidade do sistema            #
#   >>> Secao 20: Navegacao Inteligente - Uso avancado do ultrassonico         #
#   >>> Secao 21: Padroes de Programacao - Modelos prontos para copiar         #
#                                                                              #
#   Seguindo estas orientacoes, seus programas funcionarao perfeitamente!      #
#                                                                              #
################################################################################
```

---

# INDICE

1. [Introducao ao MostraBot](#1-introducao-ao-mostrabot)
2. [Conhecendo o Hardware](#2-conhecendo-o-hardware)
3. [Sinais Visuais do Robo](#3-sinais-visuais-do-robo)
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
14. [Configuracao de Velocidades (Upgrade 5.2)](#14-configuracao-de-velocidades-upgrade-52)
15. [Tabela de Referencia Rapida](#15-tabela-de-referencia-rapida)
16. [Exemplos Praticos Completos](#16-exemplos-praticos-completos)
17. [Resolucao de Problemas](#17-resolucao-de-problemas)
18. [REGRAS DE OURO - Programacao Estavel](#18-regras-de-ouro---programacao-estavel) **ESSENCIAL**
19. [Pausas Entre Movimentos](#19-pausas-entre-movimentos---o-segredo-da-estabilidade) **ESSENCIAL**
20. [Navegacao Inteligente com Condicoes (SE + DIST)](#20-navegacao-inteligente-com-condicoes-se--dist) **RECOMENDADO**
21. [Padroes de Programacao Recomendados](#21-padroes-de-programacao-recomendados)
22. [Dicas de Estabilidade para Programas Longos](#22-dicas-de-estabilidade-para-programas-longos)
23. [Upgrade 5.2 - Novas Funcoes (Opcional)](#23-upgrade-52---novas-funcoes-opcional)
24. [Glossario](#24-glossario)

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

## Como Funcionam os Modulos da PJE (Visao Rapida)

- **Modulo 1:** controle por **botoes e setas**. Ideal para demonstracao rapida e uso com alunos iniciantes. Nao ha digitacao manual.
- **Modulo 2 e Modulo 3:** **comandos manuais** em um campo de texto. Aqui comeca a programacao por linhas (pf, pt, pd, pe, etc.).
- **Modulo 4:** mesmo campo de texto do Modulo 2, mas com **sensores e estruturas** (`sensor`, `repita`, `se`, `dist`).

**Em resumo:** no **Modulo 1** voce clica; a partir do **Modulo 2** voce digita comandos.

![Layout dos Modulos 1, 2 e 4 da PJE](modulos.png)

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
|  Na pratica: Ligue o robo, espere as 2 piscadas longas, conecte sensores   |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 3. Sinais Visuais do Robo

Esta e uma das secoes mais importantes! O robo "fala" com voce atraves de LEDs. Aprenda a interpretar esses sinais.

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
|  PASSO 2: LED pisca rapido (50ms ON, 200ms OFF) - conectando WiFi          |
|           * o * o * o * o * o * o * o * o * o                              |
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
|  2 vezes      muito rapido                                                 |
|  (lento)      (tentando reconectar)                                        |
|  * * (500ms)                                                               |
|     |                                                                      |
|     v                                                                      |
|  PASSO 4: Configurando DNS (pisca 100ms ON/OFF)                            |
|           |                                                                |
|           v                                                                |
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
| LED pisca rapido (50ms ON/200ms OFF)| Conectando ao WiFi...                |
+-------------------------------------+--------------------------------------+
| LED pisca 2x lento (500ms)          | WiFi conectado com sucesso!          |
+-------------------------------------+--------------------------------------+
| LED pisca medio (100ms ON/OFF)      | Configurando DNS                     |
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
|  O LED vai piscar rapidamente (50ms ON, 200ms OFF).                        |
|  Aguarde ate ele piscar 2 vezes lentamente (sucesso).                      |
|                                                                            |
|                                                                            |
|  PASSO 3: Confirme a conexao                                               |
|  -----------------------------------------------------------------         |
|  SUCESSO: LED pisca 2 vezes lentamente (500ms cada)                        |
|  FALHA: LED pisca muito rapidamente (reinicie e tente novamente)           |
|                                                                            |
|                                                                            |
|  PASSO 4: Conecte os sensores (se necessario)                              |
|  -----------------------------------------------------------------         |
|  SOMENTE APOS o boot completo (2 piscadas lentas), conecte:                |
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

## Layout dos Modulos (M1, M2 e M4)

Use esta leitura rapida do layout para orientar a turma:

**Modulo 1 (botoes e setas)**
- **Botoes verdes:** setas de movimento (`PF`, `PT`, `PD`, `PE`) e acoes rapidas (`LD`, `LE`, `LDE`, `BU`, `BC`, `SC`).
- **Select de tempo:** escolhe o tempo em **milissegundos** (valores prontos).
- **Botoes de acao:** `ENVIAR` (verde), `LIMPAR` (amarelo) e `LIMPAR TUDO` (vermelho).
- **Objetivo:** controle imediato, sem digitacao de comandos.

**Modulo 2 (comandos manuais)**
- **Caixa grande de texto:** voce digita os comandos linha por linha.
- **Teclado azul:** digita o tempo em milissegundos.
- **Botao `ENVIAR` (verde):** envia o programa inteiro.

**Modulo 4 (sensores e logica)**
- **Mesmo formato do Modulo 2**, porem usando sensores e estruturas (`sensor`, `repita`, `se`, `dist`).

**Regra didatica simples:** no **Modulo 1** voce clica. A partir do **Modulo 2**, voce digita.

### Exemplo no Modulo 1 (cliques)
1. Escolha o tempo no **select** (ex.: `1000`).
2. Clique no botao verde **PF** (frente).
3. Clique em **ENVIAR** (botao verde).

No Modulo 1 nao existe digitacao de comandos. Os comandos manuais comecam no Modulo 2.

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

**Pausa silenciosa:** se quiser apenas dar tempo ao robo sem som, use o comando `t <ms>`.

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
|   ===|=====|===              |    ===+=====|===                            |
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

## Comando de Navegacao Ultrassonica

Na versao 5.1, existe **um unico modo** de navegacao ultrassonica:

```
+-----------------------------+----------------------------------------------+
|         COMANDO             |              DESCRICAO                       |
+-----------------------------+----------------------------------------------+
| seguir-ultra                | Navegacao com desvio de obstaculos           |
|                             | Sistema anti-picos com rampa                 |
|                             | Giro alternado para encontrar saida          |
+-----------------------------+----------------------------------------------+
```

## Como Funciona o seguir-ultra

```
+----------------------------------------------------------------------------+
|                     COMPORTAMENTO DO SEGUIR-ULTRA                          |
+----------------------------------------------------------------------------+
|                                                                            |
|  O robo executa o seguinte algoritmo:                                      |
|                                                                            |
|  1. Anda para frente ate detectar obstaculo (<=20cm)                       |
|                                                                            |
|  2. Ao detectar obstaculo (3 leituras consecutivas para confirmar):        |
|     a) Desacelera suavemente (rampa anti-picos)                            |
|     b) Para completamente                                                  |
|     c) Da re curta (120ms)                                                 |
|     d) Gira para encontrar saida (direita/esquerda alternado)              |
|     e) Verifica se caminho esta livre                                      |
|     f) Se livre, acelera suavemente e continua                             |
|                                                                            |
|  3. Se todas direcoes bloqueadas:                                          |
|     -> Re extra (200ms)                                                    |
|     -> Tenta novamente                                                     |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Ativando o Modo Ultrassonico

### Passo 1: Ativar o sensor
```
sensor ultra
```

### Passo 2: Executar a navegacao
```
seguir-ultra <tempo_ms>
```

## Exemplo Completo de Navegacao

```
sensor ultra
repita 30
  seguir-ultra 2000
  t 50
fim repita
sensor off
bu 500
```

**Explicacao:**
1. `sensor ultra` - Ativa o sensor ultrassonico
2. `repita 30` - Repete 30 vezes
3. `seguir-ultra 2000` - Navega por 2 segundos a cada iteracao
4. `t 50` - **IMPORTANTE: pausa de sincronizacao (veja Regras de Ouro)**
5. `fim repita` - Fecha o loop
6. `sensor off` - Desativa o sensor
7. `bu 500` - Bipe de finalizacao

---

## RECOMENDADO: Navegacao Manual com SE + DIST

```
+----------------------------------------------------------------------------+
|                   DICA DO PROFESSOR                                        |
+----------------------------------------------------------------------------+
|                                                                            |
|  Para maior CONTROLE e ESTABILIDADE, recomendamos usar a navegacao         |
|  manual com condicoes SE + DIST ao inves do seguir-ultra automatico.       |
|                                                                            |
|  Vantagens:                                                                |
|  - Maior controle sobre o comportamento                                    |
|  - Feedback visual com LEDs                                                |
|  - Mais didatico (aluno entende a logica)                                  |
|  - Maior estabilidade do sistema                                           |
|                                                                            |
|  >>> Veja a Secao 20 para exemplos completos! <<<                          |
|                                                                            |
+----------------------------------------------------------------------------+
```

### Exemplo de Navegacao Manual (RECOMENDADO):

```
sensor ultra

repita 25
  # Verifica obstaculo
  se dist 20
    t 100
    pt 300
    t 50
    pd 300
    t 50
  fim se

  # Caminho livre, avanca
  se distm 20
    pf 250
  fim se

  t 50
fim repita

sensor off
bu 500
lde 500
```

Este modelo e **mais estavel e didatico** que o seguir-ultra automatico!

### Exemplo Estavel com Pausas (Modelo Pratico)

```
sensor ultra
repita 100
  pf 80
  se dist 30
    t 65
    pt 150
    t 65
    pd 150
    t 65
  fim se
  t 65
fim repita
sensor off
t 500
```

**Dica:** este modelo usa pausas curtas para aumentar a estabilidade em ambientes com obstaculos.

## Dicas para Ambientes de Navegacao

```
+----------------------------------------------------------------------------+
|                    DICAS PARA O AMBIENTE                                   |
+----------------------------------------------------------------------------+
|                                                                            |
|  [OK] Use obstaculos solidos (caixas, paredes)                             |
|  [OK] Superficies perpendiculares ao sensor                                |
|  [OK] Espaco minimo de 50cm entre obstaculos                               |
|  [OK] Area plana e nivelada                                                |
|                                                                            |
|  [X] Evite objetos muito finos (pernas de cadeira)                         |
|  [X] Evite superficies muito anguladas                                     |
|  [X] Evite materiais absorventes (espuma, tecido)                          |
|  [X] Evite ambientes muito apertados                                       |
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

## Armazenamento na EEPROM

A calibracao usa os enderecos 97, 98 e 99 da EEPROM:

| Endereco | Conteudo | Descricao |
|----------|----------|-----------|
| 97 | 'M' | Marcador de calibracao valida |
| 98 | 0 ou 1 | 0 = direito atrasado, 1 = esquerdo atrasado |
| 99 | valor | Valor da diferenca (0-255) |

---

# 14. Configuracao de Velocidades (Upgrade 5.2)

Na versao 5.1, a forma mais previsivel de reduzir a velocidade e usar **tempos menores** e o comando `t` entre movimentos.

A configuracao de velocidades completas (movimento, linha e ultrassom) faz parte do **Upgrade 5.2**, descrito na Secao 23.

---

# 15. Tabela de Referencia Rapida

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
|  bc         Baixar/Fechar garra     |  seguir-ultra <ms>                   |
|  sc         Subir/Abrir garra       |                                      |
|  servo1 <angulo>                    |                                      |
|  servo2 <angulo>                    |                                      |
|                                     |                                      |
|  SOM                                |  VELOCIDADES                         |
|  ---------------------------------  |  ---------------------------------   |
|  bu <ms>    Buzzer/Bipe             |  define_vel <valor>                  |
|                                     |  define_vel_linha <valor>            |
|  CALIBRACAO                         |  define_vel_ultra <valor>            |
|  ---------------------------------  |                                      |
|  atrasar_mote <valor>               |                                      |
|  atrasar_motd <valor>               |                                      |
|                                     |                                      |
|  CONTROLE                           |                                      |
|  ---------------------------------  |                                      |
|  repita <N>                         |                                      |
|  fim repita                         |                                      |
|  se <condicao>                      |                                      |
|  fim se                             |                                      |
|  t <ms>    Pausa/estabilidade       |                                      |
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

**Nota para professores:** a forma mais previsivel de ajustar o comportamento do robo e usando **tempo** nos comandos e pausas `t`. A configuracao de velocidade tambem pode ser usada, mas sempre teste com um comando curto apos ajustar.

## Valores Padrao

| Parametro | Valor Padrao | Descricao |
|-----------|--------------|-----------|
| PWM_PADRAO_DEFAULT | 700 | Velocidade padrao de movimento |
| PWM_LINHA_DEFAULT | 700 | Velocidade padrao seguidor de linha |
| PWM_ULTRA_DEFAULT | 800 | Velocidade padrao navegacao ultra |
| PWM_MINIMO | 200 | Velocidade minima permitida |
| PWM_MAX | 1023 | Velocidade maxima permitida |
| PASSO_PADRAO | 120ms | Duracao padrao de movimento |
| LED padrao | 500ms | Duracao padrao de LED |
| Buzzer padrao | 800ms | Duracao padrao de bipe |
| DIST_OBSTACULO | 20cm | Distancia para detectar obstaculo |
| TEMPO_GIRO | 200ms | Tempo de giro para desvio |

---

# 16. Exemplos Praticos Completos

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

## Exemplo 4: Explorador Ultrassonico

**Objetivo:** Explorar ambiente desviando de obstaculos.

```
sensor ultra
repita 30
  seguir-ultra 2000
fim repita
sensor off
lde 500
bu 300
bu 300
bu 300
```

## Exemplo 5: Coletor de Objetos

**Objetivo:** Ir ate um objeto, pegar e voltar.

```
sc
pf 1500
bc
pt 1500
sc
bu 500
```

## Exemplo 6: Programa Completo com Calibracao

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
define_vel 700

# Andar rapido
pf 2000

# Soltar objeto
sc

# Finalizacao
lde 1000
bu 300
bu 300
```

## Exemplo 7: Navegacao com Condicoes

**Objetivo:** Navegar e reagir a obstaculos proximos.

```
sensor ultra
repita 50
  se dist 15
    bu 100
    pt 200
    pd 200
  fim se
  pf 200
fim repita
sensor off
bu 500
```

---

# 17. Resolucao de Problemas

## Problema: Robo nao conecta ao WiFi

```
+----------------------------------------------------------------------------+
|  SINTOMA: LED pisca muito rapido continuamente                             |
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
|  4. Reduza velocidade: tempos menores ou define_vel_ultra 500              |
|  5. Evite objetos muito finos ou absorventes                               |
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
|  SINTOMA: Robo reinicia durante operacao (2 piscadas novamente)            |
+----------------------------------------------------------------------------+
|  CAUSAS E SOLUCOES:                                                        |
|                                                                            |
|  1. PROGRAMA SEM PAUSAS DE SINCRONIZACAO                                   |
|     -> Adicione t 50 (ou bu 50) entre comandos (veja Secao 18)             |
|     -> Nunca faca mais de 8 comandos sem t 50                              |
|                                                                            |
|  2. LOOP MUITO LONGO SEM RESPIRO                                           |
|     -> Sempre inclua t 50 (ou bu 50) dentro de loops                       |
|     -> Veja exemplos na Secao 21                                           |
|                                                                            |
|  3. INVERSAO BRUSCA DE DIRECAO                                             |
|     -> Use t 100 (ou bu 100) entre pf e pt                                 |
|     -> Veja Secao 19 - Pausas Entre Movimentos                             |
|                                                                            |
|  4. BATERIA FRACA                                                          |
|     -> Recarregue a bateria                                                |
|                                                                            |
|  5. SENSORES CONECTADOS DURANTE BOOT                                       |
|     -> Reconecte sensores somente apos as 2 piscadas lentas                |
|                                                                            |
|  IMPORTANTE: Siga as REGRAS DE OURO (Secao 18) para evitar reinicializacoes|
|                                                                            |
+----------------------------------------------------------------------------+
```

## Problema: Robo trava durante programa longo

```
+----------------------------------------------------------------------------+
|  SINTOMA: Robo para de responder durante execucao de programa              |
+----------------------------------------------------------------------------+
|  SOLUCAO: O programa precisa de "respiros" para o sistema                  |
|                                                                            |
|  1. Adicione bu 50 a cada 5-8 comandos                                     |
|  2. Em loops, sempre inclua bu 50 no final de cada iteracao                |
|  3. Divida programas longos em etapas (veja Secao 22)                      |
|  4. Use os Padroes de Programacao da Secao 21                              |
|                                                                            |
|  EXEMPLO CORRETO:                                                          |
|  repita 30                                                                 |
|    seguir-linha 100                                                        |
|    bu 50                                                                   |
|  fim repita                                                                |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Dica: Controle Estavel do Ultrassonico

```
+----------------------------------------------------------------------------+
|  SITUACAO: Ambiente com muitos obstaculos ou reflexos                      |
+----------------------------------------------------------------------------+
|  RECOMENDACAO: Use navegacao manual com SE + DIST (mais estavel)           |
|                                                                            |
|  Em vez de apenas:                                                         |
|    seguir-ultra 5000                                                       |
|                                                                            |
|  Use navegacao com condicoes:                                              |
|    sensor ultra                                                            |
|    repita 25                                                               |
|      se dist 20                                                            |
|        bu 100                                                              |
|        pt 300                                                              |
|        bu 50                                                               |
|        pd 300                                                              |
|        bu 50                                                               |
|      fim se                                                                |
|      se distm 20                                                           |
|        pf 250                                                              |
|      fim se                                                                |
|      bu 50                                                                 |
|    fim repita                                                              |
|    sensor off                                                              |
|    bu 500                                                                  |
|                                                                            |
|  Veja mais exemplos na Secao 20 - Navegacao Inteligente                    |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 18. REGRAS DE OURO - Programacao Estavel

Esta secao e **FUNDAMENTAL** para o bom funcionamento do robo. Siga estas regras para garantir que seus programas funcionem perfeitamente!

## Por que Seguir as Regras de Ouro?

O MostraBot e um sistema eletronico sofisticado que precisa de **tempo para processar** cada comando. Assim como nos precisamos de pausas para pensar, o robo tambem precisa de pequenos intervalos para executar suas acoes com precisao.

**Dica importante:** se quiser uma pausa silenciosa, use `t 50` no lugar de `bu 50`.

```
+----------------------------------------------------------------------------+
|                    AS 5 REGRAS DE OURO DO MOSTRABOT                        |
+----------------------------------------------------------------------------+
|                                                                            |
|  REGRA 1: Sempre use BU (bipe) ou T (pausa) entre sequencias              |
|  REGRA 2: Nunca faca mais de 10 comandos sem uma pausa                    |
|  REGRA 3: Sempre termine programas com sensor off e bu                    |
|  REGRA 4: Use tempos minimos recomendados para cada comando               |
|  REGRA 5: Em loops longos, inclua um t 50 (ou bu 50) a cada 5-8 repeticoes|
|                                                                            |
+----------------------------------------------------------------------------+
```

## Regra 1: Bipe de Sincronizacao

O comando `bu` (buzzer) nao serve apenas para fazer som - ele tambem da tempo para o robo **sincronizar seus sistemas internos**. Use-o como separador entre blocos de comandos.

### Por que funciona:
O bipe cria uma pequena pausa que permite ao robo:
- Estabilizar os motores apos movimentos
- Processar a proxima sequencia de comandos
- Manter a conexao WiFi ativa

### Exemplo CORRETO:
```
pf 1000
pd 200
bu 50
pf 1000
pd 200
bu 50
pf 1000
pd 200
bu 50
pf 1000
pd 200
bu 500
```

### Exemplo INCORRETO (pode causar comportamento instavel):
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

## Regra 2: Pausas em Sequencias Longas

```
+----------------------------------------------------------------------------+
|                    LIMITE DE COMANDOS CONSECUTIVOS                         |
+----------------------------------------------------------------------------+
|                                                                            |
|   Quantidade de comandos         |   Acao recomendada                      |
|   -------------------------------|----------------------------------------|
|   1 a 5 comandos                 |   Pode executar direto                  |
|   6 a 10 comandos                |   Adicione bu 50 no meio                |
|   Mais de 10 comandos            |   Divida em blocos de 5-8 com bu 50     |
|                                                                            |
|   DICA: O bu 50 e tao curto que quase nao se ouve, mas faz diferenca!     |
|                                                                            |
+----------------------------------------------------------------------------+
```

### Exemplo de Programa Longo BEM ESTRUTURADO:
```
# Bloco 1 - Primeiro lado do percurso
pf 1000
pd 100
pf 500
pe 100
pf 1000
bu 50

# Bloco 2 - Segundo lado do percurso
pt 500
pd 200
pf 800
pe 150
pf 600
bu 50

# Bloco 3 - Finalizacao
pt 300
pd 180
pf 1000
bu 500
```

## Regra 3: Finalizacao Obrigatoria

**TODO programa deve terminar com:**

```
sensor off
bu 300
```

Isso garante que:
1. Os sensores sejam desativados corretamente
2. O robo sinalize que terminou
3. Os pinos D3/D8 fiquem prontos para proxima execucao

### Exemplo COMPLETO:
```
sensor linha
repita 20
  seguir-linha 100
  bu 50
fim repita
sensor off
bu 500
lde 500
```

## Regra 4: Tempos Minimos Recomendados

```
+----------------------------------------------------------------------------+
|                    TEMPOS MINIMOS PARA ESTABILIDADE                        |
+----------------------------------------------------------------------------+
|                                                                            |
|   COMANDO              |  TEMPO MINIMO    |   MOTIVO                       |
|   ---------------------|------------------|--------------------------------|
|   pf, pt               |  100ms           |  Motor precisa acelerar        |
|   pd, pe               |  100ms           |  Giro precisa de impulso       |
|   bu (sincronizacao)   |  50ms            |  Tempo de processamento        |
|   bu (audivel)         |  200ms           |  Para ouvir o som              |
|   ld, le, lde          |  100ms           |  LED precisa acender           |
|   seguir-linha         |  50ms            |  Leitura do sensor             |
|   seguir-ultra         |  100ms           |  Leitura do ultrassom          |
|                                                                            |
|   IMPORTANTE: Tempos muito curtos podem causar comportamento imprevisivel! |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Regra 5: Loops Longos com Respiro

Quando usar `repita` com muitas repeticoes, inclua um `t 50` (pausa silenciosa) a cada 5-8 iteracoes para dar "respiro" ao sistema. Se preferir feedback sonoro, use `bu 50`.

### Exemplo com Loop Longo:
```
sensor linha
repita 50
  seguir-linha 100
  t 50
fim repita
sensor off
bu 500
```

### NUNCA faca isso (pode travar):
```
sensor linha
repita 100
  seguir-linha 50
fim repita
sensor off
```

### SEMPRE faca isso:
```
sensor linha
repita 100
  seguir-linha 50
  t 50
fim repita
sensor off
bu 500
```

---

# 19. Pausas Entre Movimentos - O Segredo da Estabilidade

## Entendendo o Sistema de Movimento

O MostraBot usa motores eletricos que precisam de tempo para:
1. **Acelerar** - sair do repouso para a velocidade desejada
2. **Manter** - executar o movimento
3. **Desacelerar** - parar de forma suave
4. **Estabilizar** - preparar para o proximo movimento

```
+----------------------------------------------------------------------------+
|                    CICLO DE MOVIMENTO DO MOTOR                             |
+----------------------------------------------------------------------------+
|                                                                            |
|   Velocidade                                                               |
|       ^                                                                    |
|       |        .-------.                                                   |
|       |       /         \                                                  |
|       |      /           \                                                 |
|       |     /             \                                                |
|       |    /               \                                               |
|       +---/--------+--------\---+---> Tempo                                |
|           ^        ^         ^  ^                                          |
|           |        |         |  |                                          |
|        Acelera   Mantem   Freia  Estabiliza                                |
|                                                                            |
|   O tempo de ESTABILIZACAO e crucial para o proximo movimento!             |
|                                                                            |
+----------------------------------------------------------------------------+
```

## A Pausa de Estabilizacao

Quando o robo muda de direcao (por exemplo, de frente para tras, ou de frente para giro), os motores precisam de um momento para se preparar. Isso e normal em qualquer robo!

### Comando de Pausa Silenciosa: `t`

O comando `t` cria uma **pausa controlada** entre movimentos, ajudando a estabilizar o robo sem emitir som.

```
t <tempo_em_milissegundos>
```

Exemplos simples:
```
pf 1000
t 80
pd 200
t 80
pf 1000
```

Se quiser feedback sonoro, voce pode usar `bu 50` no lugar do `t 50`.

### Regra Pratica: Pausa de Transicao

```
+----------------------------------------------------------------------------+
|                    QUANDO USAR PAUSA DE TRANSICAO                          |
+----------------------------------------------------------------------------+
|                                                                            |
|   TRANSICAO                    |   PAUSA RECOMENDADA                       |
|   -----------------------------|------------------------------------------|
|   Frente -> Tras (pf -> pt)    |   t 100 (ou bu 100) entre comandos       |
|   Tras -> Frente (pt -> pf)    |   t 100 (ou bu 100) entre comandos       |
|   Frente -> Giro (pf -> pd/pe) |   t 50  (opcional, recomendado)          |
|   Giro -> Frente (pd/pe -> pf) |   t 50  (opcional, recomendado)          |
|   Giro Esq -> Giro Dir         |   t 100 (ou bu 100) entre comandos       |
|   Qualquer -> Parado           |   Automatico (nao precisa)               |
|                                                                            |
+----------------------------------------------------------------------------+
```

### Exemplo de Inversao de Direcao:

**CORRETO:**
```
pf 1000
t 100
pt 1000
t 100
pf 1000
```

**INCORRETO (pode causar picos de corrente):**
```
pf 1000
pt 1000
pf 1000
```

## Por que a Pausa e Importante?

```
+----------------------------------------------------------------------------+
|                    O QUE ACONTECE SEM PAUSA                                |
+----------------------------------------------------------------------------+
|                                                                            |
|   Quando voce inverte a direcao sem pausa:                                 |
|                                                                            |
|   1. Motor ainda esta girando em uma direcao                               |
|   2. Comando de inversao chega                                             |
|   3. Motor tenta inverter INSTANTANEAMENTE                                 |
|   4. Isso gera estresse no sistema                                         |
|                                                                            |
|   Com a pausa (bu 100):                                                    |
|                                                                            |
|   1. Motor para naturalmente                                               |
|   2. Sistema processa o som                                                |
|   3. Motor recebe novo comando                                             |
|   4. Movimento suave e preciso                                             |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 20. Navegacao Inteligente com Condicoes (SE + DIST)

O comando `seguir-ultra` executa uma logica automatica de navegacao. Porem, para **maior controle e precisao**, voce pode criar sua propria logica de navegacao usando os comandos `SE` com condicoes de distancia!

## Vantagens da Navegacao Manual

```
+----------------------------------------------------------------------------+
|            POR QUE USAR NAVEGACAO MANUAL COM SE + DIST?                    |
+----------------------------------------------------------------------------+
|                                                                            |
|   1. CONTROLE TOTAL sobre como o robo reage                                |
|   2. FEEDBACK VISUAL com LEDs a cada deteccao                              |
|   3. SONS PERSONALIZADOS para diferentes distancias                        |
|   4. MAIOR ESTABILIDADE do sistema                                         |
|   5. APRENDIZADO mais profundo de programacao                              |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Condicoes de Distancia Disponiveis

| Condicao | Significado | Exemplo de Uso |
|----------|-------------|----------------|
| `dist 10` | Distancia <= 10cm | Emergencia! Muito perto |
| `dist 15` | Distancia <= 15cm | Obstaculo proximo |
| `dist 20` | Distancia <= 20cm | Alerta, preparar desvio |
| `dist 30` | Distancia <= 30cm | Obstaculos a frente |
| `distm 30` | Distancia >= 30cm | Caminho razoavelmente livre |
| `distm 50` | Distancia >= 50cm | Caminho livre |
| `distm 100` | Distancia >= 100cm | Via completamente livre |

## Modelo Basico de Navegacao Manual

Este modelo e **ALTAMENTE RECOMENDADO** para uso em sala de aula por ser mais estavel e didatico:

```
sensor ultra
repita 30

  # Verifica se tem obstaculo muito perto (emergencia)
  se dist 15
    bu 100
    pt 300
    bu 50
    pd 300
    bu 50
  fim se

  # Se caminho livre, avanca um pouco
  se distm 20
    pf 200
  fim se

  bu 50

fim repita
sensor off
bu 500
lde 500
```

## Modelo Avancado com Multiplas Distancias

```
sensor ultra
repita 40

  # EMERGENCIA - muito perto (menos de 10cm)
  se dist 10
    bu 200
    pt 400
    bu 100
    pd 400
    bu 100
    ld 300
  fim se

  # ALERTA - obstaculo proximo (10-20cm)
  se dist 20
    bu 100
    pt 200
    bu 50
    pd 200
    bu 50
  fim se

  # ATENCAO - obstaculo detectado (20-30cm)
  se dist 30
    pf 100
    bu 50
  fim se

  # LIVRE - caminho aberto (mais de 30cm)
  se distm 30
    pf 300
    bu 50
  fim se

fim repita
sensor off
bu 300
bu 300
bu 300
lde 1000
```

## Modelo com Escolha de Direcao

Este modelo tenta girar para um lado, e se nao resolver, tenta o outro:

```
sensor ultra
repita 25

  # Obstaculo detectado
  se dist 20
    bu 100
    pt 200
    bu 50

    # Tenta girar para direita
    pd 200
    bu 50

    # Se ainda tem obstaculo, tenta esquerda
    se dist 20
      pe 400
      bu 50
    fim se

  fim se

  # Caminho livre, avanca
  se distm 20
    pf 250
  fim se

  bu 50

fim repita
sensor off
bu 500
```

## Modelo para Exploracao Segura (RECOMENDADO PARA INICIANTES)

Este e o modelo mais seguro e estavel para criancas:

```
sensor ultra
repita 20

  # Passo 1: Verifica se tem obstaculo
  se dist 25
    # Para, da re, gira
    bu 100
    pt 300
    bu 100
    pd 350
    bu 100
  fim se

  # Passo 2: Se livre, da um passinho
  se distm 25
    pf 300
    bu 50
  fim se

fim repita

# Finalizacao obrigatoria
sensor off
bu 500
lde 500
```

## Tabela de Tempos Recomendados para Navegacao

```
+----------------------------------------------------------------------------+
|              TEMPOS RECOMENDADOS PARA NAVEGACAO MANUAL                     |
+----------------------------------------------------------------------------+
|                                                                            |
|   ACAO                    |   TEMPO      |   OBSERVACAO                    |
|   ------------------------|--------------|-------------------------------- |
|   Re de emergencia        |   300-400ms  |   Afasta do obstaculo           |
|   Re de alerta            |   150-200ms  |   Recuo moderado                |
|   Giro de desvio          |   200-350ms  |   ~90 a 160 graus               |
|   Avanco livre            |   200-400ms  |   Passos seguros                |
|   Bipe de sincronizacao   |   50ms       |   Entre cada acao               |
|   Bipe de alerta          |   100-200ms  |   Ao detectar obstaculo         |
|                                                                            |
|   IMPORTANTE: Sempre inclua bu 50 entre acoes para estabilidade!           |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 21. Padroes de Programacao Recomendados

Esta secao apresenta **modelos prontos** que voce pode copiar e adaptar. Todos seguem as Regras de Ouro e garantem funcionamento estavel.

## Padrao 1: Programa Basico de Movimento

```
# ============================================
# PADRAO BASICO - Movimento com Feedback
# ============================================

# Movimento inicial
pf 1000
bu 50

# Giro
pd 200
bu 50

# Mais movimento
pf 1000
bu 50

# Finalizacao
bu 500
lde 500
```

## Padrao 2: Quadrado Perfeito

```
# ============================================
# PADRAO QUADRADO - Com pausas de estabilidade
# ============================================

repita 4
  pf 1000
  bu 50
  pd 220
  bu 50
fim repita

bu 500
lde 500
```

## Padrao 3: Seguidor de Linha Estavel

```
# ============================================
# PADRAO SEGUIDOR DE LINHA - Versao Estavel
# ============================================

sensor linha

repita 30
  seguir-linha 100
  bu 50
fim repita

sensor off
bu 500
lde 500
```

## Padrao 4: Seguidor de Linha com Deteccao

```
# ============================================
# PADRAO SEGUIDOR COM DETECCAO DE EVENTOS
# ============================================

sensor linha

repita 40
  seguir-linha 80

  # Detectou cruzamento?
  se perdeu
    bu 200
    lde 300
  fim se

  bu 50
fim repita

sensor off
bu 500
```

## Padrao 5: Navegacao Ultrassonica Segura

```
# ============================================
# PADRAO NAVEGACAO ULTRA - Versao Segura
# ============================================

sensor ultra

repita 25

  se dist 20
    bu 100
    pt 300
    bu 50
    pd 300
    bu 50
  fim se

  se distm 20
    pf 250
  fim se

  bu 50

fim repita

sensor off
bu 500
lde 500
```

## Padrao 6: Coletor de Objetos

```
# ============================================
# PADRAO COLETOR - Pegar e Transportar
# ============================================

# Abre garra
sc
bu 100

# Aproxima do objeto
pf 1000
bu 100

# Fecha garra (pega)
bc
bu 200

# Transporta
pt 500
bu 50
pd 400
bu 50
pf 800
bu 100

# Solta objeto
sc
bu 200

# Recua
pt 500
bu 50

# Fecha garra
bc

# Finalizacao
bu 500
lde 500
```

## Padrao 7: Programa Longo (Mais de 20 comandos)

```
# ============================================
# PADRAO PROGRAMA LONGO - Com blocos de respiro
# ============================================

# === BLOCO 1 ===
pf 1000
pd 200
pf 500
pe 100
pf 800
bu 50

# === BLOCO 2 ===
pt 300
pd 350
pf 1000
pe 200
pf 600
bu 50

# === BLOCO 3 ===
pd 180
pf 500
pt 200
pe 90
pf 400
bu 50

# === BLOCO 4 ===
pf 1000
pd 400
pt 500
pe 200
pf 800
bu 50

# === FINALIZACAO ===
bu 500
lde 1000
```

## Padrao 8: Demonstracao Completa (Todos os Recursos)

```
# ============================================
# PADRAO DEMONSTRACAO - Mostra todos recursos
# ============================================

# 1. Saudacao inicial
bu 300
lde 500
bu 100

# 2. Movimento basico
pf 1000
bu 50
pt 500
bu 100

# 3. Giros
pd 400
bu 50
pe 400
bu 100

# 4. LEDs alternados
ld 300
le 300
ld 300
le 300
bu 100

# 5. Garra
sc
bu 200
bc
bu 200
sc
bu 100

# 6. Seguidor de linha (se sensor conectado)
sensor linha
repita 10
  seguir-linha 100
  bu 50
fim repita
sensor off
bu 100

# 7. Ultrassonico (se sensor conectado)
sensor ultra
repita 10
  se dist 25
    bu 100
    pt 200
    bu 50
    pd 200
    bu 50
  fim se
  se distm 25
    pf 200
  fim se
  bu 50
fim repita
sensor off
bu 100

# 8. Finalizacao festiva
repita 3
  bu 200
  lde 200
fim repita
bu 500
```

---

# 22. Dicas de Estabilidade para Programas Longos

## O Desafio dos Programas Longos

Programas com muitos comandos ou loops extensos exigem cuidados especiais. O robo precisa manter a conexao WiFi ativa enquanto executa, e isso requer pequenas pausas estrategicas.

## Regra do "Respiro"

```
+----------------------------------------------------------------------------+
|                    REGRA DO RESPIRO                                        |
+----------------------------------------------------------------------------+
|                                                                            |
|   A cada 5-8 comandos, inclua um "respiro" para o sistema:                 |
|                                                                            |
|   RESPIRO CURTO:  bu 50   (quase inaudivel, so para processamento)         |
|   RESPIRO MEDIO:  bu 100  (bipe curto, bom separador)                      |
|   RESPIRO LONGO:  bu 200  (bipe audivel, marca etapas)                     |
|                                                                            |
+----------------------------------------------------------------------------+
```

## Estrutura Recomendada para Loops

### Loop CURTO (ate 10 repeticoes):
```
repita 10
  # comandos
  bu 50
fim repita
```

### Loop MEDIO (10 a 30 repeticoes):
```
repita 25
  # comandos
  bu 50
fim repita
bu 100
```

### Loop LONGO (mais de 30 repeticoes):
```
repita 50
  # comandos
  bu 50

  # A cada ~10 iteracoes internas, o bu 50 ja ajuda
fim repita
bu 200
```

## Divisao em Etapas

Para programas muito longos, divida em "etapas" claramente marcadas:

```
# ========== ETAPA 1: POSICIONAMENTO ==========
pf 1000
pd 200
bu 100

# ========== ETAPA 2: COLETA ==========
sc
pf 500
bc
bu 100

# ========== ETAPA 3: TRANSPORTE ==========
pt 500
pd 400
pf 1000
bu 100

# ========== ETAPA 4: ENTREGA ==========
sc
pt 300
bc
bu 100

# ========== FINALIZACAO ==========
bu 500
lde 500
```

## Checklist de Estabilidade

Antes de executar seu programa, verifique:

```
+----------------------------------------------------------------------------+
|                    CHECKLIST DE ESTABILIDADE                               |
+----------------------------------------------------------------------------+
|                                                                            |
|  [ ] Programa termina com "sensor off" (se usou sensores)?                 |
|  [ ] Programa termina com "bu" para sinalizar fim?                         |
|  [ ] Tem t 50 (ou bu 50) entre sequencias de movimento?                    |
|  [ ] Inversoes de direcao tem t 100 (ou bu 100) entre elas?                |
|  [ ] Loops longos tem t 50 (ou bu 50) dentro deles?                        |
|  [ ] Nao ha mais de 8-10 comandos sem pausa?                               |
|  [ ] Tempos de movimento sao >= 100ms?                                     |
|                                                                            |
+----------------------------------------------------------------------------+
```

---

# 23. Upgrade 5.2 - Novas Funcoes (Opcional)

A versao 5.1 foi feita para **simplicidade e confiabilidade** em sala de aula. Para escolas que desejam **mais automacao e recursos avancados**, existe a **versao 5.2**, com modos prontos para navegacao em labirintos e maior variedade de comandos.

## O que muda na versao 5.2?

- **Modos ultrassonicos avancados** (mais autonomia em labirintos)
- **Comandos prontos** para exploracao rapida
- **Mais protecoes internas** para estabilidade
- **Opcoes extras de velocidade personalizada**

## Exemplos de Comandos Disponiveis no Upgrade 5.2

```
seguir-ultra-agil <ms>
seguir-ultra-inteligente <ms>
seguir-ultra-memoria <ms>
seguir-ultra-hibrido <ms>
parar-ultra
define_personalizado <valor>
define_personalizado_linha <valor>
define_personalizado_ultra <valor>
reset_personalizado
```

**Recomendacao para professores:** se o foco for **labirintos avancados** e **navegacao autonoma mais inteligente**, vale solicitar o upgrade 5.2.

---

# 24. Glossario

| Termo | Definicao |
|-------|-----------|
| **Boot** | Processo de inicializacao quando o robo liga |
| **Buzzer** | Componente que emite sons/bipes. Tambem usado para sincronizacao do sistema |
| **Ciclo de Respiro** | Pausa necessaria para o robo processar comandos (ex: t 50 ou bu 50) |
| **EEPROM** | Memoria nao-volatil que mantem dados apos desligar |
| **ESP8266** | Microcontrolador com WiFi usado no Wemos D1 Mini |
| **Estabilizacao** | Tempo que o motor precisa para se preparar para novo movimento |
| **GPIO** | General Purpose Input/Output - pinos programaveis |
| **HC-SR04** | Modelo do sensor ultrassonico |
| **Heartbeat** | Pulso periodico do LED indicando que robo esta operacional |
| **IR** | Infravermelho - tecnologia dos sensores de linha |
| **LED** | Light Emitting Diode - os "olhos" do robo |
| **Pausa de Transicao** | Tempo entre mudancas de direcao (recomendado: t 100 ou bu 100) |
| **PJE** | Plataforma Jabuti Edu - sistema de controle online |
| **PWM** | Pulse Width Modulation - controle de potencia dos motores |
| **Rampa** | Sistema de aceleracao/desaceleracao suave |
| **Regras de Ouro** | Conjunto de boas praticas essenciais para programacao estavel |
| **Respiro** | Pausa curta (t 50 ou bu 50) entre comandos para estabilidade |
| **t (Tempo)** | Comando de pausa silenciosa em milissegundos |
| **Servo** | Motor de posicionamento preciso (usado na garra) |
| **Sincronizacao** | Momento em que o robo processa internamente os comandos |
| **Ultrassonico** | Sensor que mede distancia por ondas sonoras |
| **WiFi** | Conexao sem fio para internet |
| **Wemos D1 Mini** | Placa de desenvolvimento usada no MostraBot |

---

# Informacoes de Contato

**EJR Robotica Educacional**
- WhatsApp: 51 99292 1288
- Email: eloirjr@gmail.com
- Plataforma: pje.ejrrobotica.com.br

---

*Versao do Documento: 2.0 (Atualizada com Guia de Boas Praticas)*
*Versao do Firmware: MostraBot 5em1 v5.1*
*Data: Fevereiro 2026*

---

```
+----------------------------------------------------------------------------+
|                         LEITURA OBRIGATORIA!                               |
+----------------------------------------------------------------------------+
|                                                                            |
|   Antes de usar o MostraBot em sala de aula, leia atentamente:             |
|                                                                            |
|   - Secao 18: REGRAS DE OURO - Programacao Estavel                         |
|   - Secao 19: Pausas Entre Movimentos                                      |
|   - Secao 20: Navegacao Inteligente com Condicoes                          |
|                                                                            |
|   Estas secoes contem informacoes ESSENCIAIS para o bom funcionamento      |
|   do robo e garantem a melhor experiencia de aprendizado!                  |
|                                                                            |
+----------------------------------------------------------------------------+
```

**Esta apostila foi elaborada para auxiliar professores no uso do MostraBot em sala de aula. Qualquer duvida, entre em contato com a EJR Robotica Educacional.**
