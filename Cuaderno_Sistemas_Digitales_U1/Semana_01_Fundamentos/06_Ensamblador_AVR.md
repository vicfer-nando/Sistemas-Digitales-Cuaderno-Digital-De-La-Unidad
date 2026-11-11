# Instrucciones básicas del ensamblador AVR

[Índice de la semana](README.md) · [Siguiente: registros de E/S](07_Registros_AVR.md)

## Del lenguaje C a instrucciones

El **ensamblador** usa nombres simbólicos para las instrucciones que ejecuta una arquitectura. El compilador traduce C/C++ a instrucciones y el ensamblador las codifica en lenguaje máquina. El código de AVR no se puede ejecutar directamente en el ESP32: sus CPU y juegos de instrucciones son distintos.

AVR sigue un enfoque **RISC**, con operaciones relativamente simples y muchas instrucciones de un ciclo. Esto no significa que todas duren un ciclo. La distinción RISC/CISC describe familias y decisiones de diseño; no garantiza por sí sola que una plataforma sea más rápida.

## Instrucciones del curso

| Instrucción | Ejemplo | Significado |
|---|---|---|
| `LDI` | `ldi r16, 0x20` | Carga una constante inmediata |
| `ADD` | `add r16, r17` | Suma los dos registros y deja el resultado en r16 |
| `AND` | `and r16, r17` | AND entre registros |
| `OR` | `or r16, r17` | OR entre registros |
| `EOR` | `eor r16, r17` | XOR entre registros |
| `COM` | `com r16` | Complementa los ocho bits |
| `SBI` | `sbi 0x05, 5` | Establece un bit de E/S permitido |
| `CBI` | `cbi 0x05, 5` | Borra un bit de E/S permitido |
| `IN` / `OUT` | `in r16, 0x03` | Transfiere entre registros de CPU y E/S |
| `RJMP` | `rjmp espera` | Salto relativo a una etiqueta |

**Inmediato** significa que el dato forma parte de la propia instrucción. En AVR, `LDI` admite `r16` a `r31`, no cualquiera de los 32 registros. `SBI` y `CBI` solo admiten direcciones de E/S `0x00` a `0x1F` y bits 0 a 7; no sirven para cualquier dirección de memoria.

## Dos espacios de direcciones

En el ATmega328P, PORTB tiene dirección **`0x05` en E/S** y **`0x25` en el espacio de datos**. Son dos formas de acceder al mismo registro con instrucciones distintas. `SBI` utiliza la dirección de E/S, por lo que `sbi 0x25, 5` no es una sustitución válida.

| Registro | Dirección de E/S | Dirección de datos |
|---|---|---|
| PINB | `0x03` | `0x23` |
| DDRB | `0x04` | `0x24` |
| PORTB | `0x05` | `0x25` |

## Ejemplo de lectura

Este **fragmento AVR para ATmega328P** configura PB5 como salida y lo deja en alto. No contiene tabla de vectores ni inicialización completa de un programa.

```asm
    ldi r16, 0x20    ; r16 = 00100000
    sbi 0x04, 5      ; DDRB5 = 1: salida
    sbi 0x05, 5      ; PORTB5 = 1: nivel alto
espera:
    rjmp espera     ; permanece en este punto
```

Para dejar PB4 en bajo se usa `cbi 0x05, 4`. Esta es una corrección que R destaca en su p. 10: para borrar un bit no corresponde `SBI`.

La expresión C `PORTB |= (1 << 5);` **puede** compilarse como `SBI` si el compilador reconoce el registro y el bit constantes. No se debe generalizar «una línea C = una instrucción»: depende del código, del destino y de las optimizaciones. La secuencia binaria citada por I12 para ese `SBI` es `0x9A2D`; no hace falta memorizarla para entender la operación.

**Base de clase:** R, pp. 9–10; K03–K04; I12, pp. 2–4. **Verificación adicional:** D1, mapa de registros y resumen de instrucciones. Véase [Fuentes](../FUENTES.md).
