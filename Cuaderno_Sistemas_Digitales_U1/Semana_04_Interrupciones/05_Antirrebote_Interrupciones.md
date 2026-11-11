# Antirrebote mediante interrupciones

[Índice de la semana](README.md) · [Índice general](../README.md)

## La interrupción también detecta el rebote

Una ISR no distingue por sí sola una pulsación real de las oscilaciones de un contacto. Configurar FALLING puede producir varios avisos al presionar; CHANGE añade también las transiciones de liberación. Se necesita un criterio para decidir qué cambio se acepta.

## Filtro de tiempo muerto del material

I43 utiliza una ventana de **50 ms**: se acepta un flanco si han pasado al menos 50 ms desde el último aceptado. La resta sin signo evita problemas en el retorno del reloj a cero. El valor de 50 ms es una decisión del ejemplo, no una propiedad de todos los botones.

Ese filtro acepta el primer flanco y rechaza los demasiado cercanos. Puede rechazar una segunda pulsación rápida o aceptar ruido aislado. **No demuestra que el nivel haya permanecido estable**. Si el tiempo inicial vale cero, además puede rechazar un primer evento ocurrido en los primeros 50 ms; puede usarse una condición de «aún no hubo evento aceptado» para evitarlo.

El material hace la consulta temporal dentro de la ISR. Aquí se elige una variante que deja las consultas de tiempo, la lectura del pin y la validación en `loop()`. Esta es una **adaptación didáctica propia**, fundamentada en el patrón de estabilidad de la semana 2 y el aviso por interrupción de la semana 4.

## Variante: CHANGE avisa y loop valida estabilidad

Se registra cualquier cambio con una bandera. Al recibirlo, se inicia o reinicia un período de observación. Mientras se valida, `loop()` lee la entrada y reinicia el tiempo si observa otra variación. Tras 30 ms sin cambios observados, acepta el nuevo estado. Solo el estado estable HIGH → LOW cuenta una pulsación.

**Sketch independiente, ESP32 clásico y Arduino-ESP32 3.x:**

```cpp
#include <Arduino.h>

constexpr uint8_t BOTON = 21;
constexpr uint32_t ESTABILIDAD_MS = 30;
portMUX_TYPE mux = portMUX_INITIALIZER_UNLOCKED;
volatile bool cambioPendiente = false;
bool validando = false;
bool candidata = HIGH;
bool estable = HIGH;
uint32_t cambioEn = 0;
uint32_t pulsaciones = 0;

void IRAM_ATTR alCambio() {
  portENTER_CRITICAL_ISR(&mux);
  cambioPendiente = true;
  portEXIT_CRITICAL_ISR(&mux);
}

void setup() {
  Serial.begin(115200);
  pinMode(BOTON, INPUT_PULLUP);
  candidata = digitalRead(BOTON);
  estable = candidata;
  attachInterrupt(digitalPinToInterrupt(BOTON), alCambio, CHANGE);
  // Validar también el nivel inicial, por si cambió durante la preparación.
  validando = true;
  cambioEn = millis();
}

void loop() {
  portENTER_CRITICAL(&mux);
  bool cambio = cambioPendiente;
  cambioPendiente = false;
  portEXIT_CRITICAL(&mux);

  uint32_t ahora = millis();
  if (cambio) {
    validando = true;
    candidata = digitalRead(BOTON);
    cambioEn = ahora;
  }
  if (!validando) {
    return;
  }
  bool lectura = digitalRead(BOTON);
  if (lectura != candidata) {
    candidata = lectura;
    cambioEn = ahora;
  }
  if (static_cast<uint32_t>(ahora - cambioEn) >= ESTABILIDAD_MS) {
    validando = false;
    if (candidata != estable) {
      estable = candidata;
      if (estable == LOW) {
        ++pulsaciones;
        Serial.println(pulsaciones);
      }
    }
  }
}
```

La bandera puede agrupar flancos y eso es aceptable aquí: interesa **reiniciar la validación del estado**, no contar oscilaciones. La liberación también inicia una validación, de modo que una siguiente presión puede reconocerse como nueva transición estable.

## Límites del ejemplo

El superlazo debe volver con frecuencia. Una tarea larga retrasa la validación y puede ocultar cambios intermedios. La ventana empieza cuando `loop()` recibe el aviso, de modo que puede aceptar más tarde que una validación con marcas de tiempo de cada flanco. Los cambios muy rápidos no se conservan individualmente.

Un cambio físico puede coincidir con la lectura: ningún programa de muestreo demuestra continuidad infinita entre observaciones. Este patrón ofrece antirrebote para un pulsador lento, no un instrumento de captura de pulsos. Para un botón común, el [sondeo con estabilidad](../Semana_02_ESP32/06_Pulsadores_Antirrebote.md) suele ser más sencillo y suficiente.

## Errores frecuentes y correcciones

| Error | Por qué falla | Corrección |
|---|---|---|
| `delay(50)` dentro de ISR | Prolonga o bloquea la atención | Validar fuera de la ISR |
| Imprimir en cada ISR | Añade trabajo de duración y dependencias inadecuadas | Guardar aviso e imprimir en loop |
| Confiar en `volatile` para leer y limpiar | No protege la operación compuesta | Usar la misma sección crítica en ambos contextos |
| Contar cada FALLING como una pulsación | El rebote genera varios flancos | Filtrar y detectar cambio de estado estable |
| Esperar en ISR hasta que se suelte | Mantiene ocupado el contexto de interrupción | Procesar presión y liberación por estados |
| Usar bool para no perder ninguna cuenta | Agrupa varios eventos pendientes | Contador o cola según la información requerida |

## Comprobación

**¿Hay que usar 30 ms o 50 ms en todo circuito?** No. Se parte de esos ejemplos de clase y se ajusta según el comportamiento real y la rapidez requerida. Este cuaderno no atribuye resultados de prueba a ninguno de los filtros.

**Base de clase:** R, pp. 14 y 19; K06 y K08; I22, pp. 5–6; I43, pp. 2–4. **Adaptación propia:** validación en loop con CHANGE y protección D8. Véase [Fuentes](../FUENTES.md).
