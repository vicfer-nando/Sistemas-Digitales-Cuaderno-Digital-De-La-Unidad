# Operaciones lógicas, máscaras y desplazamientos

[Índice de la semana](README.md) · [Siguiente: ensamblador AVR](06_Ensamblador_AVR.md)

## Lógica de un bit

Las operaciones bit a bit actúan en cada posición por separado. AND produce 1 si ambos operandos tienen 1; OR si al menos uno lo tiene; XOR si son distintos; NOT invierte el bit.

| A | B | AND | OR | XOR | NOT A |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 | 1 |
| 0 | 1 | 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 |

En C/C++ los operadores son `&`, `|`, `^` y `~`. No deben confundirse con `&&`, `||` y `!`, que trabajan con condiciones lógicas.

```text
  11001010   0xCA
& 00001111   0x0F
----------
  00001010   0x0A
```

## Crear y aplicar una máscara

Una **máscara** selecciona las posiciones relevantes. `1 << 5` desplaza el 1 cinco posiciones hacia la izquierda: `00100000`, o `0x20`. El bit menos significativo se numera **0**.

| Intención | Expresión conceptual | Efecto |
|---|---|---|
| Poner n en 1 | `reg \|= mascara;` | OR conserva los demás bits |
| Poner n en 0 | `reg &= ~mascara;` | AND borra solo lo seleccionado |
| Alternar n | `reg ^= mascara;` | XOR invierte lo seleccionado |
| Consultar n | `(reg & mascara) != 0` | Devuelve una condición sin modificar |

**Fragmento C++ para ESP32, dentro de una función:**

```cpp
uint32_t reg = 0xAA;
constexpr uint32_t mascara = 1UL << 5;
reg |= mascara;
reg &= ~mascara;
reg ^= mascara;
bool activo = (reg & mascara) != 0;
```

Estas cuatro operaciones se ejecutan en secuencia; `reg` es una variable didáctica. No es un registro GPIO real. Para un registro compartido con una ISR, leer, modificar y escribir puede exigir protección; se retoma en [W1TS y W1TC](../Semana_02_ESP32/02_Arquitectura_GPIO.md).

## Desplazamientos y ancho

En enteros **sin signo**, `x >> n` desplaza hacia la derecha y rellena con ceros. Equivale a dividir por $2^n$ descartando la parte fraccionaria. El desplazamiento a la izquierda multiplica por $2^n$ cuando el resultado cabe en el ancho considerado.

El tipo del operando importa. En ESP32, `1UL << n` emplea un entero sin signo de 32 bits; se requiere $0\le n<32$. Para GPIO32 y superiores no se desplaza 32 posiciones en ese registro: se utiliza el banco adecuado. Los desplazamientos negativos o iguales al ancho del tipo son inválidos.

Al aplicar `~` a un `uint8_t`, C++ primero lo promueve normalmente a `int`. Para expresar una inversión limitada a ocho bits se convierte el resultado:

```cpp
uint8_t a = 0xCC;
uint8_t invertido = static_cast<uint8_t>(~a); // 0x33
```

## Ejercicios del material

Cada fila comienza de nuevo con `reg = 0xAA = 10101010`; no se encadenan.

| Operación | Cálculo de ocho bits | Resultado |
|---|---|---|
| `reg \|= (1 << 0)` | `10101010 OR 00000001` | `10101011 = 0xAB` |
| `reg &= ~(1 << 7)` | `10101010 AND 01111111` | `00101010 = 0x2A` |
| `reg ^= 0x0F` | `10101010 XOR 00001111` | `10100101 = 0xA5` |
| `reg & (1 << 3)` | `10101010 AND 00001000` | `0x08`, condición verdadera |

`reg = (1 << 2)` reemplaza todo el contenido por `00000100`. Si se quería establecer solo el bit 2 conservando lo demás, corresponde `|=`.

**Base de clase:** R, pp. 7–8; K03; I12, pp. 1–3; I22, pp. 1–3; fotografía I-F06. Véase [Fuentes](../FUENTES.md).
