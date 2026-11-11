# Sistemas binario, decimal y hexadecimal

[Índice de la semana](README.md) · [Siguiente: operaciones de bits](05_Operaciones_Bits.md)

## Un mismo valor, distintas escrituras

Un sistema de numeración posicional asigna a cada cifra un peso según su posición. En decimal se usan diez símbolos; en binario, dos; en hexadecimal, dieciséis.

$$
V=\sum_{i=0}^{n-1}d_i\,b^i
$$

$b$ es la base y $d_i$ es la cifra de la posición $i$, contada desde la derecha empezando en cero. Por ejemplo, $205=2\times10^2+0\times10^1+5\times10^0$.

| Base | Símbolos | Escritura habitual en C/C++ |
|---|---|---|
| Decimal | 0–9 | `200` |
| Binaria | 0 y 1 | `0b11001000` en C++ de Arduino |
| Hexadecimal | 0–9 y A–F | `0xC8` |

## De binario a decimal

Los pesos de un byte son:

| Posición | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|---|---|
| Peso | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| `10000001` | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |

Se suman los pesos de las posiciones con 1: $128+1=129$. Los ceros iniciales ayudan a ver el ancho, pero no cambian el valor.

## Hexadecimal y grupos de cuatro bits

Cada dígito hexadecimal representa un **nibble**, un grupo de cuatro bits. Un byte requiere dos dígitos hexadecimales.

| Hex | Binario | Hex | Binario | Hex | Binario | Hex | Binario |
|---|---|---|---|---|---|---|---|
| 0 | 0000 | 4 | 0100 | 8 | 1000 | C | 1100 |
| 1 | 0001 | 5 | 0101 | 9 | 1001 | D | 1101 |
| 2 | 0010 | 6 | 0110 | A | 1010 | E | 1110 |
| 3 | 0011 | 7 | 0111 | B | 1011 | F | 1111 |

Para `0xC8`, se convierte C en `1100` y 8 en `1000`. Después:

$$
0xC8=12\times16+8=200=11001000_2.
$$

## De decimal a otra base

Para convertir 200 a hexadecimal, se divide entre 16: $200=12\times16+8$. El cociente 12 se representa con C y el resto con 8: **C8**.

Para obtener binario se puede dividir repetidamente entre 2 y leer los restos desde el último hasta el primero. También se pueden seleccionar pesos: $200-128=72$, $72-64=8$, $8-8=0$. Quedan encendidos los bits 7, 6 y 3: `11001000`.

| Decimal | Hexadecimal | Binario de ocho bits |
|---|---|---|
| 8 | `0x08` | `00001000` |
| 32 | `0x20` | `00100000` |
| 129 | `0x81` | `10000001` |
| 200 | `0xC8` | `11001000` |
| 255 | `0xFF` | `11111111` |

Con $n$ bits sin signo hay $2^n$ combinaciones y el rango es $0$ a $2^n-1$. Ocho bits representan de 0 a 255. La interpretación con signo se estudia en [Tipos de datos](../Semana_02_ESP32/04_Tipos_Datos.md).

## Comprobación

**¿`0x20` y 20 son iguales?** No: `0x20` vale $2\times16=32$. El prefijo cambia la base, no el almacenamiento físico del número.

**Base de clase:** R, p. 7; K03; I11, p. 7; I12, p. 1; fotografía I-F06. Véase [Fuentes](../FUENTES.md).
