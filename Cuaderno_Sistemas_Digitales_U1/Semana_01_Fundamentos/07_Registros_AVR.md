# DDRB, PORTB y PINB

[Índice de la semana](README.md) · [Siguiente semana: ESP32](../Semana_02_ESP32/README.md)

## Un registro por función

Un **registro de periférico** es una posición especial que configura o refleja hardware. No debe confundirse con los registros `r0`–`r31` de trabajo de la CPU. En el ATmega328P se estudian los puertos B, C y D; cada bit del registro se relaciona con un pin del puerto.

| Registro del puerto B | Uso básico | Significado |
|---|---|---|
| DDRB | Elegir dirección | Bit 1: salida; bit 0: entrada |
| PORTB | Escribir o seleccionar pull-up | Con salida controla nivel; con entrada puede activar pull-up |
| PINB | Leer | Refleja el nivel lógico muestreado del pin |

PB5 significa **bit 5 del puerto B**. En un Arduino Uno clásico corresponde a D13; no significa «terminal físico número 5». La posición física depende del encapsulado.

## Combinaciones de configuración

La tabla supone E/S digital ordinaria, sin una función alternativa tomando el control y con la habilitación global de pull-up sin bloquear.

| DDRBn | PORTBn | Comportamiento |
|---|---|---|
| 0 | 0 | Entrada de alta impedancia, sin pull-up interna |
| 0 | 1 | Entrada con pull-up interna |
| 1 | 0 | Salida en bajo |
| 1 | 1 | Salida en alto |

**Alta impedancia** significa que la entrada carga muy poco el circuito externo. Sin una señal ni resistencia que defina su tensión, puede quedar flotante y variar por ruido; no es una fuente deliberada de números aleatorios.

## Salida: controlar PB5

**Fragmento C para ATmega328P**, con `<avr/io.h>`, dentro de una función:

```c
DDRB |= (1 << PB5);       // PB5 como salida
PORTB |= (1 << PB5);      // nivel alto
PORTB &= ~(1 << PB5);     // nivel bajo
```

Se usan máscaras para conservar los otros bits. En el Uno alimentado a 5 V, los niveles se aproximan a GND y VCC bajo las condiciones eléctricas correspondientes. La corriente y la carga influyen en la tensión real. Para un LED externo se requiere resistencia en serie.

## Entrada: pulsador con pull-up

Si PB0 se conecta a un pulsador que cierra hacia GND:

```c
DDRB &= ~(1 << PB0);      // entrada
PORTB |= (1 << PB0);      // pull-up interna
bool pulsado = (PINB & (1 << PB0)) == 0;
```

En C se incluye también `<stdbool.h>` para `bool`; en C++ ya existe el tipo. **Suelto = alto; presionado = bajo**. La resistencia pull-up mantiene un nivel cuando el botón está abierto y limita la corriente cuando se cierra. El rebote se estudia en la semana 2.

## Precauciones al manipular registros

Leer PINB permite conocer el nivel del pin; leer PORTB obtiene el valor del latch de salida, que tiene otra función. **Precisión técnica, D1:** escribir un 1 en un bit de PINB alterna el latch correspondiente de PORTB. Por eso PINB no debe tratarse como una variable común sobre la cual se escribe para «actualizar la lectura».

Una expresión de lectura-modificación-escritura puede interferir con una ISR que use el mismo puerto. Un compilador puede emitir instrucciones de bit indivisibles en ciertos casos, pero no se debe asumir sin revisar el caso concreto.

**Base de clase:** R, p. 10; K04; I12, pp. 4–5; fotografía I-F07. **Verificación adicional:** D1. Véase [Fuentes](../FUENTES.md).
