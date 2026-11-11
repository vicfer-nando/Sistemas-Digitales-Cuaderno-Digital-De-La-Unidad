# Configuración Arduino-ESP32 e interrupciones del timer

[Índice de la semana](README.md) · [Siguiente: ejercicios](05_Formulas_Ejercicios.md)

## ISR y callback

La alarma puede solicitar una **interrupción**. La CPU atiende una **ISR**, rutina de servicio de interrupción, y después reanuda el contexto que interrumpió. Un **callback** es una función que se entrega a otro componente para que la invoque cuando corresponda.

En la API utilizada, el callback del timer se ejecuta **en contexto de ISR**. No todo callback de cualquier biblioteca tiene esa propiedad. En este caso debe ser breve: registrar que ocurrió el aviso y dejar impresiones, comunicaciones y cálculos largos para `loop()`.

## API 2.x frente a 3.x

| Intención | Arduino-ESP32 2.x, estilo anterior | Arduino-ESP32 3.x, utilizado aquí |
|---|---|---|
| Crear/configurar | `timerBegin(0, 80, true)` | `timerBegin(1000000)` |
| Asociar ISR | `timerAttachInterrupt(t, &alTimer, true)` | `timerAttachInterrupt(t, &alTimer)` |
| Configurar alarma | `timerAlarmWrite(t, 500000, true)` | `timerAlarm(t, 500000, true, 0)` |
| Habilitar alarma | `timerAlarmEnable(t)` | Incluido al configurar con `timerAlarm()` |

La columna antigua es una referencia de lectura, no se mezcla con el sketch 3.x. El primer argumento de `timerBegin()` 3.x es la frecuencia de ticks en Hz, **no el número de timer**. Si falla, devuelve `nullptr`. Tras crearse correctamente, el contador arranca; por eso el ejemplo lo detiene durante su preparación.

## Ejemplo: aviso periódico de 0,5 s

**Sketch independiente, ESP32 clásico y Arduino-ESP32 3.x.** Se utiliza una bandera porque interesa saber que hay trabajo pendiente, sin conservar todos los avisos acumulados.

```cpp
#include <Arduino.h>

hw_timer_t* timer = nullptr;
portMUX_TYPE mux = portMUX_INITIALIZER_UNLOCKED;
volatile bool pendiente = false;

void IRAM_ATTR alTimer() {
  portENTER_CRITICAL_ISR(&mux);
  pendiente = true;
  portEXIT_CRITICAL_ISR(&mux);
}

void setup() {
  Serial.begin(115200);
  timer = timerBegin(1000000); // 1 tick = 1 us
  if (timer == nullptr) {
    Serial.println("No se pudo crear el timer");
    return;
  }
  timerStop(timer);
  timerAttachInterrupt(timer, &alTimer);
  timerWrite(timer, 0);
  timerAlarm(timer, 500000, true, 0); // alarma y recarga en 0
  timerStart(timer);
}

void loop() {
  portENTER_CRITICAL(&mux);
  bool aviso = pendiente;
  pendiente = false;
  portEXIT_CRITICAL(&mux);
  if (aviso) {
    Serial.println("Aviso del temporizador");
  }
}
```

La cuenta recorre 500 000 ticks de 1 µs: el intervalo nominal es 0,5 s. El mensaje se imprime cuando `loop()` atiende la bandera; no demuestra experimentalmente que hayan pasado exactamente 0,5 s entre impresiones. Si se alternara un LED una vez por aviso, necesitaría dos avisos para completar encendido y apagado: su ciclo completo sería de 1 s, equivalente a 1 Hz.

## Variables compartidas

`volatile` expresa que el valor puede cambiar fuera del flujo normal. No vuelve indivisible el conjunto «leer y limpiar». Entre ambas instrucciones podría llegar otro aviso y ser borrado por error.

El ejemplo protege la escritura en ISR y la copia/limpieza en `loop()` con el mismo **`portMUX_TYPE`**, mecanismo de sección crítica del entorno ESP32. La sección contiene únicamente el acceso compartido. Esto también contempla la concurrencia entre núcleos; desactivar solo las interrupciones locales no protege frente al otro núcleo.

Aunque la copia y limpieza estén protegidas, varias asignaciones `pendiente=true` antes de la atención se **agrupan en un único aviso**. Para conservar una cantidad se utiliza el [contador de eventos](../Semana_04_Interrupciones/04_Banderas_Eventos.md).

## Dónde ampliar

[ISR, IRAM_ATTR y volatile](../Semana_04_Interrupciones/03_ISR_IRAM_Volatile.md) explica las condiciones de memoria y seguridad. Marcar una función `IRAM_ATTR` no certifica que todo lo que llama sea accesible sin caché.

**Base de clase:** R, pp. 17–19; K07; I32, pp. 3–5. **Complemento técnico:** D5, D7–D8 y D12–D13 para API, acceso compartido e implementación. Véase [Fuentes](../FUENTES.md).
