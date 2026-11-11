# Apoyo: electrónica básica y almacenamiento de un bit

[Índice de la semana](README.md)

Este apartado recoge conceptos breves presentes en las primeras fuentes y necesarios para interpretar un circuito digital. No sustituye una unidad de electrónica analógica.

## Transistor y niveles digitales

Un **transistor** permite controlar corriente mediante una señal. En circuitos digitales se utilizan redes de transistores para construir funciones lógicas y almacenamiento. La comparación de clase con un interruptor ayuda a imaginar dos estados, pero la relación entre transistor y salida depende del circuito.

No es universal afirmar «transistor en corte = 0» y «en saturación = 1». Por ejemplo, en un montaje inversor con resistencia hacia alimentación, un transistor que conduce puede llevar la salida a bajo. En tecnologías CMOS se emplean transistores MOS con otra descripción de regiones de operación.

El **bit** es la información abstracta, 0 o 1. En un GPIO se representa mediante rangos de voltaje; en una memoria puede corresponder a un estado eléctrico de una celda. No todos los bits están almacenados con el mismo mecanismo.

## Resistencia, capacitor e inductor

| Componente | Idea útil en esta unidad | Ejemplo |
|---|---|---|
| Resistencia | Relaciona tensión y corriente; $V=IR$ | Limitar corriente del LED y definir una entrada con pull-up |
| Capacitor | Almacena energía en un campo eléctrico | Ayudar a estabilizar alimentación mediante desacoplo |
| Inductor | Almacena energía en un campo magnético | Bobinados y circuitos de potencia |

Las primeras notas mencionan estos componentes como contexto. Los circuitos del cuaderno solo calculan resistencias para LED y polarización de entradas; no se proponen valores de filtros RC ni montajes de potencia sin un diseño específico.

## Flip-flop y registro

Un **flip-flop** es un elemento secuencial capaz de conservar un bit. Una señal de reloj puede determinar cuándo se actualiza su estado. Varias celdas de almacenamiento forman registros de varios bits, aunque el circuito real depende de la implementación.

Esto permite entender por qué un registro de ocho bits conserva un patrón como `00100000` entre escrituras. La salida del programa no desaparece por terminar una instrucción: el hardware retiene el estado hasta que se modifica o reinicia.

El registro **de trabajo** de la CPU guarda operandos; el registro **de periférico** configura hardware; una celda **de SRAM** almacena datos. Comparten la idea de conservar información, pero tienen funciones y circuitos diferentes.

## Comprobación

**¿Un GPIO alto alimenta de forma segura cualquier carga?** No. Un nivel lógico no implica capacidad de potencia ilimitada. El ejemplo de LED necesita resistencia y una carga como un motor necesita una etapa adecuada.

**Base de clase:** K01, p. 1 y manuscritos; I12, p. 1; fotografías I-F01, I-F02 e I-F04. Las distinciones sobre representación del bit y transistor son precisiones conceptuales para evitar generalizaciones. Véase [Fuentes](../FUENTES.md).
