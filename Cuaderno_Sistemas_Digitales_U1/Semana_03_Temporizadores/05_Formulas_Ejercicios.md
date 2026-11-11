# Formulario y ejercicios resueltos

[Índice de la semana](README.md) · [Siguiente semana: interrupciones externas](../Semana_04_Interrupciones/README.md)

## Formulario con unidades

Se supone conteo ascendente desde cero, alarma positiva y recarga en cero para repetición. $f_e$ es la frecuencia de entrada en Hz; $N$ es el divisor sin unidades; $f_t$ es la frecuencia de ticks en Hz; $A$ es la cantidad de ticks hasta la alarma; $n$ es el ancho del contador.

| Magnitud | Fórmula | Unidad del resultado |
|---|---|---|
| Período de señal | $T=1/f$ | s |
| Frecuencia de señal | $f=1/T$ | Hz |
| Frecuencia de ticks | $f_t=f_e/N$ | ticks/s, Hz |
| Prescaler | $N=f_e/f_t$ | Sin unidad |
| Resolución | $\Delta t=1/f_t$ | s/tick |
| Ticks para un intervalo | $A=t f_t$ | ticks |
| Tiempo de alarma | $t=A/f_t=A\Delta t$ | s |
| Frecuencia a partir de alarma y tiempo | $f_t=A/t$ | Hz |
| Frecuencia de avisos periódicos | $f_a=1/t=f_t/A$ | avisos/s, Hz |
| Valor máximo | $C_{max}=2^n-1$ | Cuenta |
| Tiempo hasta desbordar desde cero | $t_d=2^n/f_t$ | s |

Para un contador inicial $C_0$ y alarma $A>C_0$, sin vuelta intermedia, el tiempo es $(A-C_0)/f_t$. Si se recarga en $R$, el período posterior es $(A-R)/f_t$.

## 1. Aviso cada medio segundo

**Procedencia:** K07, p. 1; I31, p. 3; R, p. 16.

**Datos:** $f_e=80\,\mathrm{MHz}$, $f_t=1\,\mathrm{MHz}$ y $t=0{,}5\,\mathrm{s}$.

**Fórmulas:** $N=f_e/f_t$, $\Delta t=1/f_t$, $A=t f_t$.

**Sustitución:**

$$
N=\frac{80\,000\,000}{1\,000\,000}=80,\quad
\Delta t=\frac{1}{1\,000\,000}=1\,\mathrm{\mu s}.
$$

$$
A=0{,}5\times1\,000\,000=500\,000\ \text{ticks}.
$$

**Resultado:** alarma de **500 000 ticks**, con divisor 80. **Comprobación:** $500\,000\times1\,\mathrm{\mu s}=500\,\mathrm{ms}$; $f_a=1/0{,}5=2\,\mathrm{Hz}$.

## 2. Alarma de 2500 ticks a 10 kHz

**Procedencia:** I31, p. 3; fotografía I-F09; aclaración en K07, p. 2 y R, p. 16.

**Datos:** $f_t=10\,000\,\mathrm{Hz}$, $A=2500$, $f_e=80\,\mathrm{MHz}$.

**Fórmulas:** $\Delta t=1/f_t$, $t=A/f_t$ y $N=f_e/f_t$.

**Sustitución:**

$$
\Delta t=\frac{1}{10\,000}=0{,}0001\,\mathrm{s}
=100\,\mathrm{\mu s}=0{,}1\,\mathrm{ms}.
$$

$$
t=\frac{2500}{10\,000}=0{,}25\,\mathrm{s},\quad
N=\frac{80\,000\,000}{10\,000}=8000.
$$

**Resultado:** **250 ms**, divisor 8000 y cuatro avisos por segundo si se repite. Para obtener 0,5 s a esa misma frecuencia se necesitan **5000 ticks**, no 2500.

## 3. Frecuencia de 20 kHz y alarma de 5000

**Procedencia:** K07, p. 1; R, p. 21.

**Datos:** $f_e=80\,\mathrm{MHz}$, $f_t=20\,000\,\mathrm{Hz}$, $A=5000$.

**Fórmulas:** $\Delta t=1/f_t$, $t=A/f_t$, $N=f_e/f_t$.

**Sustitución:** $\Delta t=1/20\,000=0{,}00005\,\mathrm{s}=50\,\mathrm{\mu s}$; $t=5000/20\,000=0{,}25\,\mathrm{s}$; $N=80\,000\,000/20\,000=4000$.

**Resultado:** **250 ms** y divisor **4000**. Comprobación: $5000\times50\,\mathrm{\mu s}=250\,000\,\mathrm{\mu s}$. La frecuencia de avisos es 4 Hz, no 20 kHz.

## 4. Un aviso cada 250 ms a 1 MHz

**Procedencia:** R, p. 21; K07, p. 2.

**Datos:** $t=250\,\mathrm{ms}=0{,}25\,\mathrm{s}$ y $f_t=1\,000\,000\,\mathrm{Hz}$.

**Fórmula:** $A=t f_t$.

**Sustitución:** $A=0{,}25\times1\,000\,000=250\,000$.

**Resultado:** **250 000 ticks**. Comprobación: cada tick dura 1 µs, así que la suma de los ticks dura 250 ms.

## 5. Tres ejercicios breves de clase

**Procedencia común:** I31, p. 3; K07, p. 2; R, p. 21.

| Ejercicio y datos | Fórmula y sustitución | Resultado |
|---|---|---|
| A: $f_t=1\,\mathrm{MHz}$, $A=2000$ | $t=2000/1\,000\,000$ | $0{,}002\,\mathrm{s}=2\,\mathrm{ms}$ |
| B: $t=10\,\mathrm{ms}$, $f_t=1\,\mathrm{MHz}$ | $A=0{,}010\times1\,000\,000$ | 10 000 ticks |
| C: $f_t=40\,000\,\mathrm{Hz}$, $A=40\,000$ | $t=40\,000/40\,000$ | 1 s |
| C, divisor con $f_e=80\,\mathrm{MHz}$ | $N=80\,000\,000/40\,000$ | 2000 |

## 6. Encontrar la frecuencia por dos caminos

**Procedencia:** despejes desarrollados en R, pp. 20–21.

**Datos A:** un tick dura 50 µs. $f_t=1/0{,}00005=20\,000\,\mathrm{Hz}$.

**Datos B:** 5000 ticks deben durar 0,25 s. $f_t=A/t=5000/0{,}25=20\,000\,\mathrm{Hz}$.

**Resultado:** ambos datos son coherentes. Si cada 5000 ticks hay un aviso, la frecuencia del aviso es 4 Hz.

## 7. Período de una señal de 60 Hz

**Procedencia:** R, p. 15; K07, p. 2.

**Datos → fórmula → sustitución:** $f=60\,\mathrm{Hz}$; $T=1/f$; $T=1/60=0{,}016666\ldots\,\mathrm{s}$.

**Resultado:** aproximadamente **16,67 ms**. Con ticks de 1 µs no existe una cantidad entera de ticks que represente exactamente ese período: redondear a 16 667 ticks produce 16,667 ms. La cuantización es distinta de un error de conversión de unidades.

## 8. Desbordamiento de un contador de 16 bits

**Procedencia:** comparación de límites en I32, p. 2; desarrollo aritmético propio.

**Datos:** $n=16$, $f_t=1\,\mathrm{MHz}$, inicio en cero sin recarga.

**Fórmula:** $t_d=2^n/f_t$.

**Sustitución:** $t_d=65\,536/1\,000\,000=0{,}065536\,\mathrm{s}$.

**Resultado:** **65,536 ms**. El máximo es 65 535, pero el retorno a cero requiere **65 536 ticks**. La alarma puede fijarse antes de ese límite.

## Cómo revisar una respuesta

Convierte primero a segundos y Hz; comprueba que el divisor sea realizable y que la alarma sea entera y quepa. Después reconstruye el tiempo con $A\Delta t$. Si el producto de segundos por ticks/segundo no entrega ticks, se mezclaron unidades. Para redondeo, declara el intervalo efectivamente obtenido.

[Fuentes y localizadores de los ejercicios](../FUENTES.md).
