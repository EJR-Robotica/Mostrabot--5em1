# Arquitetura do Firmware MostraBot

**Versão:** 1.0
**Data:** 2026-01-16
**Projeto:** REPTO MOSTRABOT - Plataforma Jabuti Edu Nuvem (PJE)

---

## 1. Visao Geral

O MostraBot e um robo educacional controlado remotamente via WiFi, parte da plataforma Jabuti Edu Nuvem (PJE). O firmware roda em um ESP8266 (Wemos D1 Mini) e se comunica com um servidor PHP para receber comandos.

### 1.1 Diagrama de Contexto

```
┌─────────────┐     HTTP/WiFi     ┌─────────────────┐
│  MostraBot  │ ←───────────────→ │   PJE Nuvem     │
│  (ESP8266)  │    polling        │ (Servidor PHP)  │
└─────────────┘                   └─────────────────┘
       │
       ▼
┌─────────────────────────────────────────┐
│  Hardware: Motores, Servos, Sensores    │
└─────────────────────────────────────────┘
```

---

## 2. Drivers Arquiteturais

| ID | Driver | Descricao | Impacto |
|----|--------|-----------|---------|
| DR-1 | Confiabilidade | Funcionar consistentemente em demonstracoes | Watchdog, reconexao WiFi |
| DR-2 | Manutenibilidade | Codigo facil de entender e modificar | Modularizacao, constantes |
| DR-3 | Extensibilidade | Adicionar novos comandos facilmente | Parser extensivel |
| DR-4 | Limitacao HW | Pinos D3/D8 compartilhados (boot + sensores) | Modo SENSOR_OFF no boot |
| DR-5 | Configurabilidade | Multiplos robos com configs diferentes | Placeholders {{VAR}} |

---

## 3. Arquitetura de Alto Nivel

### 3.1 Padrao: Monolito Modular Embarcado

Arquivo unico `.ino` com secoes bem separadas logicamente.

```
┌─────────────────────────────────────────────────────────────────┐
│                     FIRMWARE MOSTRABOT                          │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │
│  │   CONFIG    │  │    SETUP    │  │          LOOP           │  │
│  │ (constantes)│  │ (init HW)   │  │  (heartbeat + polling)  │  │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘  │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                      MODULOS LOGICOS                        ││
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────┐  ││
│  │  │ Motores  │ │ Sensores │ │  Servos  │ │ Comunicacao    │  ││
│  │  │ (PWM)    │ │(IR/Ultra)│ │ (Garra)  │ │ (WiFi/HTTP)    │  ││
│  │  └──────────┘ └──────────┘ └──────────┘ └────────────────┘  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    EXECUTOR DE COMANDOS                     ││
│  │  ┌─────────┐ ┌─────────────┐ ┌───────────────────────────┐  ││
│  │  │ Parser  │→│Interpretador│→│ Executor (repita/se/cmd)  │  ││
│  │  └─────────┘ └─────────────┘ └───────────────────────────┘  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                      PERSISTENCIA                           ││
│  │              EEPROM (Calibracao de Motores)                 ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

### 3.2 Justificativa

- **Arquivo unico:** Facilita compartilhamento e compilacao no Arduino IDE
- **Modulos logicos:** Funcoes agrupadas por responsabilidade
- **Placeholders:** Sistema PJE substitui antes de compilar

---

## 4. Stack Tecnologico

| Camada | Tecnologia | Justificativa |
|--------|------------|---------------|
| Hardware | ESP8266 (Wemos D1 Mini) | Baixo custo, WiFi integrado |
| Framework | Arduino Core for ESP8266 | Comunidade ampla, educacional |
| Comunicacao | HTTP Polling | Simples, compativel com PHP |
| Persistencia | EEPROM | Calibracao persiste entre boots |
| Servo | Servo.h | Biblioteca padrao Arduino |
| Ultrassom | Ultrasonic.h | Filtro de mediana implementado |

---

## 5. Componentes do Sistema

### 5.1 Modulo de Configuracao

**Arquivo:** Topo do `.ino` (linhas 1-80)

```cpp
// Placeholders substituidos pela PJE
const char* WIFI_SSID     = "{{WIFI_SSID}}";
const char* WIFI_PASSWORD = "{{WIFI_PASSWORD}}";
const char* PJE_HOST      = "{{PJE_HOST}}";
const char* EQUIP_NOME    = "{{EQUIP_NOME}}";
const char* EQUIP_SENHA   = "{{EQUIP_SENHA}}";

// Constantes de hardware
#define MOT1_A      D0
#define PWM_PADRAO  1000
// ...
```

**Responsabilidade:** Centralizar configuracoes e constantes

---

### 5.2 Modulo de Motores

**Funcoes:**
| Funcao | Descricao |
|--------|-----------|
| `pararMotores()` | Para todos os motores |
| `moverFrente(pwm)` | Move para frente |
| `moverTras(pwm)` | Move para tras |
| `moverEsquerda(pwm)` | Gira para esquerda |
| `moverDireita(pwm)` | Gira para direita |
| `clampPWM(valor)` | Limita PWM entre 0-1023 |

**Pinos:**
- `MOT1_A` (D0) - Motor 1 Frente
- `MOT1_B` (D5) - Motor 1 Re
- `MOT2_A` (D1) - Motor 2 Frente
- `MOT2_B` (D4) - Motor 2 Re

---

### 5.3 Modulo de Sensores

**Estados:**
```cpp
enum SensorModo {
  SENSOR_OFF,    // Padrao no boot (seguro)
  SENSOR_LINHA,  // Infravermelho seguidor de linha
  SENSOR_ULTRA   // Ultrassonico HC-SR04
};
```

**Funcoes:**
| Funcao | Descricao |
|--------|-----------|
| `setSensorModo(modo)` | Alterna modo do sensor |
| `lerUltrassomFiltrado()` | Leitura com filtro mediana (5 amostras) |
| `getDistanciaCache()` | Leitura com cache (120ms) |
| `avaliarCondicao(cond)` | Avalia condicoes `se` |

**Pinos Compartilhados (D3/D8):**

| Modo | D3 | D8 |
|------|----|----|
| ULTRA | TRIGGER | ECHO |
| LINHA | Sensor Direita | Sensor Esquerda |

> **ATENCAO:** D8 (GPIO15) e pino de BOOT. Sensores devem ser conectados APOS o WiFi conectar.

---

### 5.4 Modulo de Servos

**Funcoes:**
| Comando | Acao |
|---------|------|
| `bc` | Baixar garra (10 graus) |
| `sc` | Subir garra (170 graus) |
| `servo1 X` | Servo 1 para angulo X |
| `servo2 X` | Servo 2 para angulo X |

**Pinos:**
- `SERVO1_PIN` (D6) - Garra
- `SERVO2_PIN` (D7) - Braco

---

### 5.5 Modulo de Comunicacao

**Funcoes:**
| Funcao | Descricao |
|--------|-----------|
| `conectarWiFi()` | Conecta na rede WiFi |
| `buscarComandosPJE()` | Faz polling no servidor |
| `atualizarHeartbeat()` | Pisca LED de status |

**Protocolo:**
```
GET /executa_equipamento.php?nome_equipamento=XXX&senha_equipamento=YYY
```

**Resposta:**
```
[cabecalho HTTP]#
pf 500
pe 200
bu 800
#
```

---

### 5.6 Modulo Executor de Comandos

**Funcoes:**
| Funcao | Descricao |
|--------|-----------|
| `executaComando(cmd, valor)` | Executa comando individual |
| `executarPrograma(corpo)` | Executa programa completo |

**Estruturas de Controle:**
- `repita N` ... `fim repita` - Loop (ate 3 niveis aninhados)
- `se COND` ... `fim se` - Condicional

**Condicoes Suportadas:**

| Condicao | Sensor | Descricao |
|----------|--------|-----------|
| `dist X` | ULTRA | Distancia <= X cm |
| `distm X` | ULTRA | Distancia >= X cm |
| `sensord` | LINHA | Sensor direita na linha |
| `nsensord` | LINHA | Sensor direita no branco |
| `sensore` | LINHA | Sensor esquerda na linha |
| `nsensore` | LINHA | Sensor esquerda no branco |
| `linhaok` | LINHA | Ambos no branco |
| `perdeu` | LINHA | Ambos na linha |

---

### 5.7 Modulo de Persistencia (EEPROM)

**Layout:**
| Endereco | Campo | Tipo | Descricao |
|----------|-------|------|-----------|
| 97 | MARKER | char | 'M' = calibrado |
| 98 | SINAL | byte | 0=positivo, 1=negativo |
| 99 | VALOR | byte | Valor absoluto |

**Comandos de Calibracao:**
- `atrasar_mote X` - Corrige desvio para esquerda
- `atrasar_motd X` - Corrige desvio para direita

---

## 6. Lista de Comandos PJE

| Comando | Parametro | Descricao |
|---------|-----------|-----------|
| `pf` | tempo_ms | Para frente |
| `pt` | tempo_ms | Para tras |
| `pe` | tempo_ms | Para esquerda |
| `pd` | tempo_ms | Para direita |
| `bu` | tempo_ms | Buzzer (2000Hz) |
| `bc` | - | Baixar garra |
| `sc` | - | Subir garra |
| `servo1` | angulo | Servo 1 (0-180) |
| `servo2` | angulo | Servo 2 (0-180) |
| `sensor` | linha/ultra/off | Mudar modo sensor |
| `seguir-linha` | - | Modo autonomo linha |
| `seguir-ultra` | - | Modo autonomo ultrassom |
| `ld` | ms | Pisca LED direito (D3) |
| `le` | ms | Pisca LED esquerdo (D8) |
| `lde` | ms | Pisca ambos LEDs (D3+D8) |
| `atrasar_mote` | valor | Calibrar motor esquerdo |
| `atrasar_motd` | valor | Calibrar motor direito |
| `repita` | N | Iniciar loop |
| `fim` | repita | Finalizar loop |
| `se` | condicao | Iniciar condicional |
| `fim` | se | Finalizar condicional |

---

## 7. Estrutura de Arquivos

```
MostrabotFirware5em1/
├── src/
│   └── firmware_template.ino    # <-- FIRMWARE OFICIAL
├── legacy/
│   └── mostrabot_ultrassom_linha.ino  # Versao antiga (referencia)
├── docs/
│   ├── architecture.md          # Este documento
│   └── pinagem.md               # Documentacao de pinos
└── .bmad-core/                  # Configuracao BMad
```

---

## 8. Fluxo de Build

```
┌──────────────────┐
│ firmware_template│
│      .ino        │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Sistema PJE     │
│  Substitui:      │
│  {{WIFI_SSID}}   │
│  {{EQUIP_NOME}}  │
│  etc.            │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Arduino IDE /   │
│  PlatformIO      │
│  Compila         │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Upload para     │
│  ESP8266         │
└──────────────────┘
```

---

## 9. Melhorias Futuras

| Prioridade | Melhoria | Justificativa |
|------------|----------|---------------|
| Media | Reconexao WiFi automatica | Robustez em demonstracoes |
| Media | Comando `parar`/`stop` | Parar motores imediatamente |
| Media | Timeout em autonomos | Seguranca (evitar robo fugir) |
| Baixa | OTA Updates | Atualizar sem cabo USB |
| Baixa | WebSocket | Menor latencia que polling |
| Baixa | Modo AP config | Config WiFi sem recompilar |

---

## 10. Regras Operacionais Criticas

### 10.1 Sequencia de Boot

```
1. Ligar robo (SEM sensores conectados)
2. Aguardar LED piscar 2x (WiFi OK)
3. Conectar sensor IR ou Ultrassonico
4. Usar normalmente
```

### 10.2 Por que isso e necessario?

O pino D8 (GPIO15) precisa estar em LOW durante o boot do ESP8266. Se o sensor estiver conectado, pode impedir o boot.

---

## Historico de Alteracoes

| Data | Versao | Descricao |
|------|--------|-----------|
| 2026-01-16 | 1.0 | Documento inicial - Analise e unificacao |
