# Arranque y estructura del programa

[Índice de la semana](README.md) · [Siguiente: tipos de datos](04_Tipos_Datos.md)

## Qué ocurre antes de setup()

El ESP32 no empieza directamente en `setup()`. De forma simplificada, el arranque normal de la aplicación Arduino sigue esta cadena:

```text
Reset → código de ROM → bootloader en Flash → inicialización de la aplicación
      → FreeRTOS / app_main() → loopTask → setup() → loop() repetidamente
```

La **ROM** contiene código de arranque incorporado al chip. El cargador de segunda etapa selecciona y prepara la aplicación desde Flash. La inicialización coloca los segmentos necesarios en RAM y prepara el entorno. En la configuración Arduino habitual, `app_main()` crea la tarea que invoca `setup()` y después repite `loop()`.

**Precisión técnica, D9–D10:** la cadena depende del modo de arranque y de la configuración. No es un diagrama completo de todas las rutas, como descarga o retorno de sueño profundo. FreeRTOS se menciona para situar el programa; su programación detallada queda fuera de esta unidad.

## EN, BOOT y strapping

Al reset se leen ciertos niveles para decidir opciones de arranque. GPIO0 en bajo participa en la entrada al modo de descarga. El botón **BOOT** suele llevarlo a bajo y **EN** reinicia el chip.

Una carga externa que fuerce un pin de strapping puede impedir el arranque normal. Las fuentes destacan GPIO12: un nivel inadecuado puede seleccionar una tensión de Flash incompatible con el módulo. La práctica inicial es usar un GPIO sin ese conflicto y revisar la placa real.

## setup() y loop()

`setup()` se ejecuta una vez **por arranque**. Allí se configuran pines, puerto serie y periféricos. `loop()` contiene el comportamiento que se repite: leer, decidir y actualizar salidas.

**Sketch didáctico independiente, ESP32 con Arduino:**

```cpp
#include <Arduino.h>

constexpr uint8_t LED = 16;
void setup() {
  pinMode(LED, OUTPUT);
  digitalWrite(LED, LOW);
}
void loop() {
  digitalWrite(LED, HIGH);
  delay(500);
  digitalWrite(LED, LOW);
  delay(500);
}
```

Se conecta un LED externo con resistencia como se explica en [Entradas y salidas](05_Entradas_Salidas.md). Los dos intervalos forman un ciclo completo de aproximadamente un segundo, sin contar el pequeño tiempo de las llamadas.

`delay()` detiene el avance de **esta tarea** durante el intervalo. En ESP32 no implica que toda la CPU ni todas las tareas de FreeRTOS dejen de funcionar. Para atender varios comportamientos desde `loop()`, suele ser mejor consultar `millis()`.

## Alcance y duración de variables

Una variable local automática se crea de nuevo en cada llamada. Una local `static` mantiene el valor entre llamadas. Una global tiene duración de todo el programa. En el ejercicio del material, al incrementar una vez por vuelta una global, una local inicializada en cero y una local estática, en la tercera vuelta resultan **3, 1 y 3**.

Ese comportamiento no depende de que `loop()` sea una función especial: depende de la duración de almacenamiento de las variables.

## Preparación y problemas habituales

El material utiliza Arduino IDE 2.x y el paquete **esp32 by Espressif Systems 3.x**. Para los ejemplos de este cuaderno se selecciona un destino compatible con el ESP32 clásico, como ESP32 Dev Module, comprobando puerto y módulo.

| Síntoma | Revisión inicial |
|---|---|
| No aparece puerto | Cable de datos, conexión y controlador del puente USB-serie |
| No entra en descarga | Secuencia BOOT/EN y cargas en pines de arranque |
| Puerto ocupado | Otra aplicación o monitor serie abierto |
| Texto ilegible | Coincidencia de la velocidad serie, por ejemplo 115200 |
| Brownout | Calidad de alimentación y cable |

Son posibles causas, no diagnósticos garantizados. Un reinicio exige revisar el mensaje concreto.

**Base de clase:** R, p. 13; K06; I21, pp. 4–5. **Verificación adicional:** D9–D10. Véase [Fuentes](../FUENTES.md).
