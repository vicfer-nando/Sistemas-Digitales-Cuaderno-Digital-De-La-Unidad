# Tipos de datos, signo y tiempo

[Índice de la semana](README.md) · [Siguiente: entradas y salidas](05_Entradas_Salidas.md)

## El tipo define cómo interpretar los bits

Dos variables de ocho bits tienen la misma cantidad de combinaciones, pero pueden interpretarlas de manera distinta. En `uint8_t` todas representan números no negativos; en `int8_t`, usando complemento a dos, una parte representa negativos.

$$
\text{Sin signo: }0\le x\le2^n-1
$$

$$
\text{Con signo, complemento a dos: }-2^{n-1}\le x\le2^{n-1}-1.
$$

| Tipo | Bits | Rango |
|---|---|---|
| `uint8_t` | 8 | 0 a 255 |
| `int8_t` | 8 | −128 a 127 |
| `uint16_t` | 16 | 0 a 65 535 |
| `int16_t` | 16 | −32 768 a 32 767 |
| `uint32_t` | 32 | 0 a 4 294 967 295 |
| `int32_t` | 32 | −2 147 483 648 a 2 147 483 647 |

El patrón `11001100` representa 204 sin signo. Interpretado como complemento a dos de ocho bits representa $204-256=-52$. Esto describe la representación; no recomienda conversiones fuera de rango como técnica portable.

## Un int no mide igual en todas las placas

| Tipo | AVR del Uno, bytes | ESP32 clásico con toolchain Arduino, bytes |
|---|---|---|
| `char` | 1 | 1 |
| `short` | 2 | 2 |
| `int` | 2 | 4 |
| `long` | 4 | 4 |
| `float` | 4 | 4 |
| `double` | 4 en la configuración habitual del core AVR | 8 |
| Puntero ordinario a datos | 2 | 4 |

Son propiedades de esos entornos, no reglas universales del lenguaje. Se comprueban con `sizeof`. El signo de `char` puede depender del compilador; para expresar signo de manera clara se prefieren `int8_t` y `uint8_t`.

## Desbordamiento y conversiones

En aritmética sin signo el resultado se reduce módulo $2^n$. En el ejercicio del material:

$$
200+100=300,\qquad300\bmod256=44.
$$

```cpp
uint8_t a = 200;
uint8_t b = 100;
uint8_t resultado = static_cast<uint8_t>(a + b); // 44
```

La suma de estos operandos pequeños normalmente se evalúa como `int`; la reducción aparece al convertirla a `uint8_t`. El **desbordamiento aritmético de un entero con signo** no debe usarse para dar la vuelta: tiene comportamiento indefinido en C/C++. La conversión fuera de rango a un tipo con signo es otro asunto y depende de la versión del lenguaje y del entorno.

## Promociones y constantes

Para `uint8_t a = 0xCC`, `~a` se evalúa normalmente después de promover a `int`: no invierte solo ocho bits. `static_cast<uint8_t>(~a)` da `0x33`, o `00110011` como byte. La impresión en binario puede omitir los ceros iniciales; no ha cambiado el ancho del tipo.

`constexpr uint8_t LED = 16;` declara una constante con tipo que puede evaluarse al compilar. Un `#define` hace sustitución textual. `static` conserva una local entre llamadas; **`volatile` no conserva un valor entre arranques ni vuelve atómica una operación**. Su papel en las ISR se desarrolla en la semana 4.

## Tiempo sin bloquear

**Fragmento para Arduino, dentro de loop(), con `anterior` declarado globalmente como `uint32_t anterior = 0;`:**

```cpp
uint32_t ahora = millis();
if (static_cast<uint32_t>(ahora - anterior) >= 500U) {
  anterior = ahora;
  // Ejecutar una acción breve cada 500 ms como mínimo.
}
```

La resta sin signo permite atravesar el retorno de `millis()` a cero, aproximadamente cada $2^{32}\,\mathrm{ms}=49{,}71$ días, siempre que se consulte con suficiente frecuencia para no perder una vuelta completa y se usen intervalos adecuados. En un modelo de ocho bits, $4-250\equiv10\pmod{256}$.

Asignar `millis()` a `int` reduce el rango útil: en AVR se supera el máximo positivo cerca de 32,8 s y en ESP32 cerca de 24,9 días. Para estas diferencias temporales se utiliza un tipo sin signo de 32 bits.

**Base de clase:** R, pp. 6, 13 y 15; K06; I22, pp. 1–4. Los cálculos son desarrollos propios de esos ejemplos. Véase [Fuentes](../FUENTES.md).
