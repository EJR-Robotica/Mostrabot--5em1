# Pinagem do REPTO MOSTRABOT

## Wemos D1 Mini (ESP8266)

```
                    ┌─────────────────┐
                    │     USB         │
                    │    ┌───┐        │
              RST ──┤1   │   │      16├── TX (GPIO1)
               A0 ──┤2   │   │      15├── RX (GPIO3)
      [MOT2_B] D0 ──┤3   │ESP│      14├── D1 [MOT2_A]
               D5 ──┤4   │   │      13├── D2 [LED/BUZZER]
      [MOT1_B] D6 ──┤5   │   │      12├── D3 [SENSOR_D3]
     [SERVO1]  D7 ──┤6   │   │      11├── D4 [MOT2_B]
     [SERVO2]  D8 ──┤7   └───┘      10├── GND
              3V3 ──┤8              9 ├── 5V
                    └─────────────────┘
```

## Tabela de Pinos

| Pino Wemos | GPIO | Função no Robô | Notas |
|------------|------|----------------|-------|
| D0 | GPIO16 | Motor 1 Frente | PWM |
| D1 | GPIO5 | Motor 2 Frente | PWM |
| D2 | GPIO4 | LED Externo / Buzzer | Compartilhado |
| D3 | GPIO0 | Sensor D3 | ⚠️ BOOT PIN |
| D4 | GPIO2 | Motor 2 Ré | PWM, LED interno |
| D5 | GPIO14 | Motor 1 Ré | PWM |
| D6 | GPIO12 | Servo 1 (Garra) | |
| D7 | GPIO13 | Servo 2 (Braço) | |
| D8 | GPIO15 | Sensor D8 | ⚠️ BOOT PIN |

## Pinos Compartilhados D3/D8 (Sensores + LEDs)

### Modo SENSOR_ULTRA (Ultrassônico HC-SR04)
| Pino | Função |
|------|--------|
| D3 | TRIGGER |
| D8 | ECHO |

### Modo SENSOR_LINHA (Infravermelho)
| Pino | Função |
|------|--------|
| D3 | Sensor Direita |
| D8 | Sensor Esquerda |

### Modo LED (Olhos do Robô)
| Pino | Função |
|------|--------|
| D3 | LED Direito |
| D8 | LED Esquerdo |

**Comandos:**
- `ld 500` - Pisca LED direito por 500ms
- `le 300` - Pisca LED esquerdo por 300ms
- `lde 1000` - Pisca ambos LEDs por 1 segundo

> 💡 **Modo automático**: `sensor off` configura D3/D8 como OUTPUT para LEDs

## Esquema de Conexão

### Ponte H (Motores)
```
        MOT1_A (D0) ──┬── Motor Esquerdo
        MOT1_B (D5) ──┘
        
        MOT2_A (D1) ──┬── Motor Direito
        MOT2_B (D4) ──┘
```

### Servos
```
        SERVO1 (D6) ──── Servo Garra
        SERVO2 (D7) ──── Servo Braço
```

### Sensores
```
     ┌─────────────────────────────────┐
     │         MODO ULTRA              │
     │                                 │
     │  D3 ─── TRIG ───┐               │
     │                 │ HC-SR04       │
     │  D8 ─── ECHO ───┘               │
     └─────────────────────────────────┘
     
     ┌─────────────────────────────────┐
     │         MODO LINHA              │
     │                                 │
     │  D3 ─── Sensor IR Direita       │
     │                                 │
     │  D8 ─── Sensor IR Esquerda      │
     └─────────────────────────────────┘
```

## ⚠️ REGRAS CRÍTICAS DE HARDWARE

### Pinos de Boot do ESP8266

Os pinos **D3 (GPIO0)** e **D8 (GPIO15)** são usados durante o boot:

| Pino | GPIO | Nível no Boot | Função |
|------|------|---------------|--------|
| D3 | GPIO0 | HIGH | Modo normal |
| D8 | GPIO15 | LOW | Necessário para boot |

### Regra Operacional OBRIGATÓRIA

1. **Ao energizar**: Sensores devem estar **DESCONECTADOS**
2. **Após Wi-Fi conectar**: Pode conectar sensores
3. **Firmware inicia**: Sempre em **SENSOR OFF**

### Sequência Correta
```
1. Ligar robô (sem sensores conectados)
2. Aguardar LED piscar 2x (Wi-Fi OK)
3. Conectar sensor IR ou Ultrassônico
4. Usar normalmente
```

### O que acontece se não seguir:
- ESP8266 pode não bootar
- Robô trava na inicialização
- Necessário desconectar sensores e reiniciar

## PWM dos Motores

| Valor | Velocidade |
|-------|------------|
| 0 | Parado |
| 500 | ~50% |
| 1000 | ~100% (padrão) |
| 1023 | Máximo |

## Calibração

Se o robô não anda reto, usar comandos:
- `atrasar_mote X` - Corrige para esquerda
- `atrasar_motd X` - Corrige para direita

Valores salvos na EEPROM e persistem após reinício.
