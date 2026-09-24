# Laboratorio 3: Cálculo del índice pletismogrófico quirúrgico (SPI)

# 1. Introducción

La fotopletismografía (PPG) es una técnica óptica no invasiva que permite registrar las variaciones del volumen sanguíneo periférico asociadas con cada ciclo cardiaco. La señal obtenida presenta una forma de onda pulsátil en la cual es posible identificar características como los máximos sistólicos, los valles, la amplitud de cada pulso y el intervalo temporal entre latidos.

Estas características pueden modificarse como consecuencia de cambios en la actividad cardiovascular y del sistema nervioso autónomo. Por esta razón, la señal PPG puede utilizarse para obtener parámetros relacionados con la respuesta fisiológica de una persona ante diferentes estímulos.

Una de las aplicaciones de la fotopletismografía es el cálculo del **Surgical Pleth Index (SPI)** o **Índice Pletismográfico Quirúrgico**, utilizado principalmente durante anestesia general para evaluar cambios relacionados con el balance entre nocicepción y antinocicepción.

El SPI utiliza dos características derivadas de la onda fotopletismográfica:

- **PPGA (Plethysmographic Pulse Wave Amplitude):** amplitud de la onda de pulso.
- **HBI (Heart Beat Interval):** intervalo temporal entre latidos consecutivos.

El índice presenta valores comprendidos entre 0 y 100. De acuerdo con la guía de laboratorio, valores más elevados se relacionan con una mayor respuesta nociceptiva o de estrés, mientras que durante anestesia general se menciona como referencia un intervalo aproximado entre 20 y 50.

En esta práctica se desarrolló un sistema de adquisición utilizando un sensor **MAX30102**, una tarjeta **ESP32** y **MATLAB**. La señal PPG fue transmitida desde la ESP32 hacia el computador mediante comunicación serial y posteriormente procesada en MATLAB.

Para identificar cada ciclo cardiaco se implementó un algoritmo de detección de máximos y mínimos. A partir de estos puntos se determinaron la frecuencia cardiaca, la amplitud pico-valle de cada pulso (PPGA), el intervalo entre latidos (HBI) y finalmente una estimación del SPI.

Con el propósito de evaluar la respuesta del sistema frente a un estímulo fisiológico, se utilizó el **Cold Pressor Test (CPT)**. En el protocolo experimental realizado se adquirieron **40 segundos continuos de señal PPG**, divididos en dos condiciones:

- **Primeros 20 segundos:** condición normal o de referencia.
- **Últimos 20 segundos:** aplicación del Cold Pressor Test.

De esta manera fue posible observar y comparar el comportamiento de la señal fotopletismográfica y del SPI antes y durante la aplicación del estímulo frío.

---

# 2. Objetivos

## 2.1 Objetivo general

Desarrollar un sistema de adquisición y procesamiento de una señal fotopletismográfica que permita estimar continuamente el índice pletismográfico quirúrgico (SPI) y evaluar su comportamiento en condición normal y durante la aplicación del Cold Pressor Test.

## 2.2 Objetivos específicos

- Adquirir una señal fotopletismográfica mediante el sensor MAX30102 y una ESP32.

- Visualizar en MATLAB la señal PPG obtenida durante la adquisición.

- Implementar un algoritmo para detectar automáticamente los máximos sistólicos y los valles presentes en la señal PPG.

- Calcular la frecuencia cardiaca a partir de los pulsos detectados.

- Determinar la amplitud pico-valle de cada pulso fotopletismográfico (PPGA).

- Calcular el intervalo entre latidos consecutivos (HBI).

- Estimar el índice pletismográfico quirúrgico (SPI) a partir de PPGA y HBI.

- Registrar la evolución temporal del SPI durante una captura continua de 40 segundos.

- Aplicar el Cold Pressor Test durante los últimos 20 segundos de la adquisición para producir una respuesta fisiológica autonómica.

- Comparar el comportamiento del SPI durante la condición normal y durante el Cold Pressor Test.

- Analizar las limitaciones del SPI estimado para representar la nocicepción y el dolor.
\longrightarrow
SPI_{recuperacion}
$$

La temperatura, el procedimiento específico para realizar la exposición al frío y los criterios de seguridad deben establecerse de acuerdo con el protocolo autorizado por el laboratorio y las indicaciones del docente responsable.

# 3. Revisión de literatura

## 3.1 Fotopletismografía (PPG)

La fotopletismografía (PPG) es una técnica óptica no invasiva utilizada para detectar variaciones del volumen sanguíneo en los tejidos. La señal obtenida presenta cambios periódicos relacionados con el ciclo cardiaco, debido a que la cantidad de sangre presente en los vasos periféricos varía con cada pulsación.

En una señal PPG es posible identificar diferentes características asociadas con cada ciclo cardiaco. Para el desarrollo de esta práctica se consideraron principalmente los máximos sistólicos, los valles de la señal, la amplitud de cada pulso y el intervalo temporal entre pulsaciones consecutivas.

Estas características permiten obtener dos parámetros necesarios para la estimación del índice pletismográfico quirúrgico:

- **PPGA (Plethysmographic Pulse Wave Amplitude):** amplitud de la onda fotopletismográfica.
- **HBI (Heart Beat Interval):** intervalo temporal entre latidos consecutivos.

La señal PPG también puede presentar modificaciones asociadas con cambios en la perfusión periférica y en la actividad del sistema nervioso autónomo, por lo que resulta útil para estudiar respuestas fisiológicas ante diferentes estímulos.

---

## 3.2 Sistema nervioso autónomo y señal PPG

El sistema nervioso autónomo participa en la regulación de diferentes funciones cardiovasculares, entre ellas la frecuencia cardiaca y el tono vascular periférico.

Ante determinados estímulos puede producirse un aumento de la actividad simpática, acompañado de cambios en la frecuencia cardiaca y vasoconstricción periférica. Estas modificaciones pueden alterar la cantidad de sangre que llega al sitio donde se realiza la medición fotopletismográfica y, por lo tanto, modificar la amplitud de la señal PPG.

De manera simplificada, una respuesta simpática puede representarse como:

$$
\text{Estímulo}
\rightarrow
\text{Activación autonómica}
\rightarrow
\text{Cambios cardiovasculares y vasculares}
\rightarrow
\text{Modificación de la señal PPG}
$$

Esta relación es importante para el cálculo del SPI, debido a que el índice utiliza características de la señal PPG y del intervalo entre latidos para representar cambios relacionados con la respuesta autonómica.

---

## 3.3 Nocicepción y dolor

Para interpretar correctamente el SPI es necesario diferenciar los conceptos de **nocicepción** y **dolor**.

La nocicepción está relacionada con los procesos fisiológicos mediante los cuales el organismo detecta y responde ante estímulos potencialmente dañinos. Estas respuestas pueden producir modificaciones en variables controladas por el sistema nervioso autónomo, como la frecuencia cardiaca y el tono vascular periférico.

El dolor, por otra parte, corresponde a una experiencia consciente y subjetiva. Por esta razón, una modificación de una variable fisiológica no debe interpretarse directamente como una medida de la intensidad del dolor percibido por una persona.

Esta diferencia es especialmente importante en esta práctica, debido a que el SPI fue desarrollado para evaluar cambios relacionados con el balance entre nocicepción y antinocicepción durante anestesia general.

Por lo tanto:

$$
\boxed{\text{SPI} \neq \text{medición directa del dolor}}
$$

El SPI debe interpretarse como un índice derivado de respuestas fisiológicas y no como una escala directa de dolor.

---

## 3.4 Índice Pletismográfico Quirúrgico (SPI)

El **Surgical Pleth Index (SPI)** o **Índice Pletismográfico Quirúrgico** es un índice utilizado para evaluar cambios relacionados con el balance entre nocicepción y antinocicepción durante anestesia general.

El índice utiliza información obtenida principalmente de la onda fotopletismográfica y del intervalo entre latidos.

De acuerdo con la literatura utilizada como fundamento de la práctica, el SPI puede expresarse mediante:

$$
SPI =
100 -
\left(
0.7\,PPGA_{norm}
+
0.3\,HBI_{norm}
\right)
$$

donde:

- $PPGA_{norm}$ = amplitud normalizada de la onda fotopletismográfica.
- $HBI_{norm}$ = intervalo normalizado entre latidos.

La ecuación asigna una ponderación del **70 % a la amplitud fotopletismográfica** y del **30 % al intervalo entre latidos**.

Por esta razón, los cambios en la amplitud de la señal PPG tienen una influencia importante sobre el valor final del índice.

El SPI presenta una escala entre 0 y 100. De acuerdo con la guía de laboratorio, los valores elevados indican una mayor respuesta nociceptiva o de estrés. Durante anestesia general, la guía menciona como referencia un intervalo aproximado entre 20 y 50 para una analgesia intraoperatoria adecuada.

---

## 3.5 Amplitud de la onda fotopletismográfica (PPGA)

La **PPGA** representa la amplitud de cada pulso de la señal fotopletismográfica.

En el sistema desarrollado, esta amplitud se obtiene calculando la diferencia entre el máximo sistólico y el valle correspondiente:

$$
PPGA_i = Pico_i - Valle_i
$$

donde:

- $Pico_i$ es el valor máximo detectado para un determinado latido.
- $Valle_i$ es el mínimo asociado con dicho latido.

Gráficamente:

$$
PPGA =\text{Máximo sistólico}-\text{Valle}
$$

La amplitud PPG puede modificarse cuando cambia la perfusión periférica. Por esta razón, las respuestas vasculares producidas por estímulos autonómicos pueden reflejarse en modificaciones de PPGA.

En la ecuación utilizada para estimar el SPI, PPGA presenta una ponderación de 0.70, por lo que constituye la variable con mayor influencia sobre el índice calculado.

---

## 3.6 Intervalo entre latidos (HBI)

El **Heart Beat Interval (HBI)** representa el tiempo transcurrido entre dos pulsaciones consecutivas.

Si los máximos sistólicos son detectados en los tiempos \(t_i\) y \(t_{i+1}\), el HBI puede calcularse como:

$$
HBI_i = t_{i+1} - t_i
$$

Un HBI menor corresponde generalmente a una mayor frecuencia cardiaca, mientras que un HBI mayor corresponde a una frecuencia cardiaca menor.

La relación aproximada entre HBI y frecuencia cardiaca puede expresarse como:

$$
FC \approx \frac{60}{HBI}
$$

donde:

- $FC$ es la frecuencia cardiaca expresada en latidos por minuto.
- $HBI$ se encuentra expresado en segundos.

En el cálculo del SPI utilizado en esta práctica, el HBI normalizado presenta una ponderación de 0.30.

---

## 3.7 Normalización de PPGA y HBI

Antes de calcular el SPI, las variables HBI y PPGA fueron normalizadas en una escala entre 0 y 100.

En el código desarrollado se utilizó una normalización mínimo-máximo:

$$
HBI_{norm}=100\frac{HBI-HBI_{min}}{HBI_{max}-HBI_{min}}
$$

y:

$$
PPGA_{norm}=100\frac{PPGA-PPGA_{min}}{PPGA_{max}-PPGA_{min}}$$

Posteriormente se utilizaron estos valores en:

$$
SPI =
100-
\left(
0.30HBI_{norm}
+
0.70PPGA_{norm}
\right)
$$

Esta implementación permite obtener un índice comprendido aproximadamente entre 0 y 100 para los datos analizados.

Sin embargo, la normalización utilizada depende de los valores mínimos y máximos encontrados dentro de la propia captura. Por esta razón, el resultado obtenido en esta práctica debe considerarse una **estimación experimental del SPI** y no una reproducción exacta del procesamiento utilizado por un monitor clínico comercial.

---

## 3.8 Cold Pressor Test (CPT)

El **Cold Pressor Test (CPT)** es una prueba utilizada para generar una respuesta fisiológica mediante la exposición de una extremidad a un estímulo frío.

El estímulo frío puede producir una respuesta del sistema nervioso autónomo caracterizada por activación simpática y modificaciones cardiovasculares y vasculares periféricas.

De manera simplificada:

$$
\text{Estímulo frío} \rightarrow
\text{Activación simpática} \rightarrow
\text{Respuesta cardiovascular}
$$

Entre los cambios fisiológicos asociados con esta respuesta pueden presentarse modificaciones en:

- frecuencia cardiaca;
- presión arterial;
- resistencia vascular periférica;
- perfusión sanguínea periférica.

Debido a que la señal PPG depende de las variaciones del volumen sanguíneo periférico, estos cambios pueden producir modificaciones en la amplitud de la onda fotopletismográfica.

Por esta razón, el Cold Pressor Test fue utilizado en esta práctica como un estímulo experimental para evaluar si el sistema desarrollado era capaz de detectar cambios en las variables empleadas para calcular el SPI.

En el experimento realizado se utilizó una captura continua de **40 segundos**:

<div align="center">

| Intervalo experimental | Condición |
|:---:|:---:|
| 0–20 s | Condición normal |
| 20–40 s | Cold Pressor Test |

</div>

Esto permitió registrar ambas condiciones dentro de una misma adquisición y posteriormente analizar la evolución del SPI antes y durante la aplicación del estímulo.

  # 4. Sensores y Material

Para el desarrollo experimental se utilizaron los siguientes elementos:

<div align="center">

| Elemento | Función |
|:---:|:---:|
| Sensor MAX30102 | Adquisición de la señal fotopletismográfica (PPG) |
| ESP32 | Lectura del sensor y transmisión de los datos |
| MATLAB | Procesamiento de la señal, detección de pulsos y cálculo del SPI |
| Agua fría | Estímulo utilizado durante el Cold Pressor Test |

</div>

> **Nota:** la guía de laboratorio propone originalmente la construcción de un circuito de adquisición de PPG mediante un sensor óptico de reflectancia y una etapa analógica. En esta implementación se utilizó un sensor digital MAX30102 conectado a una ESP32 para realizar la adquisición de la señal fotopletismográfica.

---

# 5. Montaje experimental

## 5.1 Sistema de adquisición

El sistema implementado estuvo compuesto principalmente por un sensor **MAX30102**, una tarjeta **ESP32** y un computador con MATLAB. El dedo del voluntario se colocó sobre el sensor MAX30102 procurando mantener una posición estable durante toda la adquisición.

El MAX30102 realizó la medición óptica y la ESP32 se encargó de transmitir los valores obtenidos hacia el computador mediante comunicación serial.

MATLAB recibió las muestras, almacenó la señal PPG y posteriormente realizó el procesamiento necesario para detectar los pulsos y estimar el SPI.

---

## 5.2 Configuración de la adquisición

La comunicación entre la ESP32 y MATLAB se realizó mediante el puerto serial.

Los principales parámetros utilizados en el código fueron:

<div align="center">

| Parámetro | Valor |
|:---:|:---:|
| Puerto serial | COM5 |
| Velocidad de comunicación | 115200 baud |
| Frecuencia de muestreo configurada | 30 Hz |
| Duración de la captura | 40 s |
| Número esperado de muestras | 1200 |

</div>

El número esperado de muestras se determinó mediante:

$$
N = F_s \times T
$$

donde:

$$
F_s = 30\ \text{Hz}
$$

y:

$$
T = 40\ \text{s}
$$

Por lo tanto:

$$
N = 30 \times 40 = 1200\ \text{muestras}
$$

Durante la captura, MATLAB verificó continuamente la disponibilidad de información en el puerto serial y almacenó cada dato válido recibido desde la ESP32.

---

# 6. Procedimiento experimental

## 6.1 Preparación del sistema

Antes de iniciar la adquisición se realizaron los siguientes pasos:

1. Se conectó el sensor MAX30102 a la ESP32.

2. Se conectó la ESP32 al computador mediante USB.

3. Se verificó la comunicación entre la ESP32 y el computador.

4. Se configuró MATLAB para utilizar el puerto `COM5` a una velocidad de `115200 baud`.

5. Se configuró una frecuencia de muestreo de 30 Hz y una duración total de captura de 40 segundos.

6. Se colocó el dedo del voluntario sobre el sensor MAX30102.

7. Se verificó que la señal fotopletismográfica pudiera observarse correctamente antes de realizar la prueba.

8. Se indicó al voluntario mantener el dedo quieto y evitar cambios en la presión ejercida sobre el sensor.

## 6.2 Adquisición en MATLAB

Al comenzar la ejecución del programa, MATLAB estableció la comunicación con la ESP32 y limpió los datos que pudieran permanecer almacenados previamente en el puerto serial.

Durante la adquisición se mostraron en la ventana de comandos mensajes para indicar el progreso de la captura:

```text
========================================
INICIANDO ADQUISICIÓN PPG
========================================
Conectando con ESP32...

========================================
CAPTURANDO BASELINE
Mantenga el dedo quieto.
No cambie la presión sobre el sensor.
========================================

Tiempo Baseline: 0 / 20 s
Tiempo Baseline: 1 / 20 s
...
Tiempo Baseline: 19 / 20 s
Tiempo Baseline: 20 / 20 s

========================================
INICIANDO COLD PRESSOR TEST
========================================
Continúe con la adquisición.
========================================

Tiempo CPT: 20 / 40 s
Tiempo CPT: 21 / 40 s
...
Tiempo CPT: 39 / 40 s
Tiempo CPT: 40 / 40 s

========================================
ADQUISICIÓN FINALIZADA
========================================
```
## 6.3 Visualización de la señal PPG en tiempo real

Durante la adquisición, MATLAB permitió visualizar en tiempo real la señal PPG recibida desde la ESP32. Para facilitar su observación se utilizó una ventana móvil de aproximadamente 5 segundos.

La visualización en tiempo real permitió comprobar que el sensor se encontraba adquiriendo una señal pulsátil y verificar que el dedo permaneciera correctamente ubicado sobre el sensor durante el experimento.

### Señal PPG durante la adquisición

<div align="center">

<img width="612.58" height="400" alt="image" src="https://github.com/user-attachments/assets/2292da1c-2600-498f-a921-b9ade727c5b1" />

**Figura 1.** Señal fotopletismográfica visualizada en tiempo real durante la adquisición.

</div>

La figura muestra la señal fotopletismográfica (PPG) registrada durante la condición basal (Baseline) y visualizada en tiempo real. Se presenta un segmento comprendido aproximadamente entre los 35 y 40 segundos de adquisición. El eje horizontal representa el tiempo en segundos, mientras que el eje vertical corresponde al valor invertido de la señal infrarroja (IR) obtenida mediante el sensor.
En este intervalo se observa una señal periódica y pulsátil, caracterizada por máximos y mínimos repetitivos asociados con las variaciones del volumen sanguíneo periférico producidas por cada ciclo cardíaco. La morfología de los pulsos se mantiene relativamente estable durante la mayor parte del segmento, aunque se presentan pequeñas variaciones en su amplitud.
La presencia de ciclos claramente identificables indica que el sensor logra registrar las oscilaciones de la perfusión sanguínea periférica, permitiendo posteriormente detectar picos y valles y obtener parámetros derivados de la PPG, como la frecuencia cardíaca y las variables necesarias para el análisis del Índice Pletismogrófico Quirúrgico (SPI).

---

## 6.4 Señal PPG después de la adquisición

Una vez terminada la captura, se eliminaron los primeros 2 segundos de la señal con el propósito de evitar que el periodo inicial de estabilización afectara el procesamiento posterior.

La señal utilizada para el análisis tuvo, por lo tanto, una duración aproximada de:

$$
T_{analizado}=40-2=38\ \text{s}
$$

Posteriormente, la señal fue invertida para facilitar la identificación de los máximos sistólicos mediante el algoritmo implementado.

### Señal PPG procesada

<div align="center">

<img width="612.58" height="400" alt="image" src="https://github.com/user-attachments/assets/eac9b387-40f7-498e-8c07-24df5a145701" />

**Figura 2.** Segmento de la señal PPG utilizado para verificar visualmente la morfología de los pulsos.

</div>

La figura presenta la señal fotopletismográfica (PPG) correspondiente a la condición basal, mostrando un intervalo de aproximadamente 5 a 10 segundos de la adquisición. El eje horizontal representa el tiempo en segundos, mientras que el eje vertical corresponde al valor invertido de la señal infrarroja (IR) registrada por el sensor. Se observa un comportamiento pulsátil y periódico, con máximos y mínimos sucesivos que representan las variaciones del volumen sanguíneo periférico asociadas con cada ciclo cardíaco. Durante este segmento se distinguen aproximadamente ocho ciclos pulsátiles, aunque existen variaciones tanto en la amplitud como en la morfología de los pulsos. En comparación con una señal ideal completamente uniforme, se aprecia una variabilidad moderada en la amplitud, especialmente entre los primeros pulsos y los registrados posteriormente. Sin embargo, la periodicidad general se conserva y los ciclos individuales permanecen claramente identificables, lo que permite utilizar esta señal basal como referencia para la posterior comparación con la señal obtenida durante el Cold Pressor Test y para el cálculo del índice SPI.

## 6.5 Detección de máximos y mínimos mediante MMPD

Para identificar cada pulso de la señal PPG se implementó un algoritmo basado en la detección de máximos y mínimos.

El procedimiento analiza el comportamiento ascendente y descendente de la señal para localizar los máximos sistólicos y los valles asociados con cada pulso.

También se estableció una distancia mínima entre máximos consecutivos para evitar que pequeñas fluctuaciones de la señal fueran interpretadas como nuevos latidos.

### Resultado de la detección

<div align="center">

<img width="612.58" height="400" alt="image" src="https://github.com/user-attachments/assets/02327ff9-30a4-46bb-bd82-171e9cde872f" />

**Figura 3.** Detección de máximos y mínimos de la señal PPG mediante el algoritmo implementado.

</div>

La figura muestra la detección de puntos característicos de la señal PPG basal mediante el método MMPD (Mountains-Moving Peak Detection) en el intervalo comprendido aproximadamente entre 5 y 10 segundos. La señal PPG se representa mediante la línea azul, mientras que los triángulos rojos indican los picos sistólicos y los triángulos verdes corresponden a los valles detectados.
Se observa que el algoritmo identifica de forma adecuada los máximos y mínimos principales de cada ciclo pulsátil. En el segmento mostrado se distinguen aproximadamente ocho picos sistólicos, cada uno asociado a un pulso cardíaco, junto con sus respectivos valles. La detección sigue correctamente las variaciones de amplitud presentes en la señal sin confundir las pequeñas irregularidades de la morfología con nuevos pulsos.
La correcta localización de picos y valles es fundamental para el análisis posterior, ya que permite determinar la amplitud de cada pulso PPG a partir de la diferencia entre el pico sistólico y su valle correspondiente. Estas amplitudes pueden emplearse para obtener el valor representativo de la condición basal y compararlo con el registrado durante el Cold Pressor Test, constituyendo la base para el cálculo y análisis del índice SPI.

Como resultado del procesamiento se obtuvieron:

<div align="center">

|      Parámetro     |  Resultado |
| :----------------: | :--------: |
| Latidos detectados |   **65**   |
| Parejas pico-valle |   **65**   |
|  Tiempo analizado  | **38.0 s** |

</div>

---

# 7. Cálculo de la frecuencia cardiaca

Una vez detectados los máximos correspondientes a los latidos, se calculó la frecuencia cardiaca a partir del número de pulsaciones encontradas durante el tiempo analizado.

El programa utiliza:

$$
FC=
\frac{N_{latidos}}
{T_{analizado}}
\times60
$$

donde:

* $N_{latidos}$ es el número de máximos detectados.
* $T_{analizado}$ corresponde al tiempo de señal utilizado.

Para la captura realizada se obtuvieron:

$$
N_{latidos}=65
$$

$$
T_{analizado}=38.0\ \text{s}
$$

Por lo tanto, MATLAB reportó una frecuencia cardiaca de:

$$
\boxed{FC=102.7\ \text{bpm}}
$$

El resultado corresponde al valor calculado para la captura completa, incluyendo tanto la condición normal como el periodo correspondiente al Cold Pressor Test.

---

# 8. Cálculo de la amplitud PPG

Después de detectar los máximos y los valles, se calculó la amplitud de cada pulso fotopletismográfico.

Para cada pareja pico-valle se utilizó:

$$
PPGA_i=Pico_i-Valle_i
$$

donde $PPGA_i$ representa la amplitud correspondiente al pulso $i$.

El procesamiento permitió obtener un total de:

$$
\boxed{65\ \text{parejas pico-valle}}
$$

La amplitud promedio calculada para toda la captura fue:

$$
\boxed{PPGA_{promedio}=1637.662}
$$

### Amplitud PPG en función del tiempo

<div align="center">

<img width="612.58" height="400" alt="image" src="https://github.com/user-attachments/assets/b9b7fa24-fd0c-4d5e-927f-042a63394b5f" />

**Figura 4.** Amplitud pico-valle de la señal PPG para los latidos detectados durante la captura.

</div>

La figura presenta la amplitud de la señal PPG latido a latido durante la condición Baseline, calculada como la diferencia entre el pico sistólico y el valle correspondiente detectados en cada ciclo cardíaco. El eje horizontal representa el tiempo en segundos, mientras que el eje vertical indica la amplitud pico-valle de cada latido.
Se observa que la amplitud PPG presenta una variabilidad considerable durante el registro basal, con valores que oscilan aproximadamente entre 800 y 2400 unidades. Durante los primeros segundos se evidencia un incremento progresivo de la amplitud, alcanzando sus valores máximos alrededor de los 10 s. Posteriormente se presenta una disminución, especialmente entre aproximadamente 16 y 23 s, seguida de una recuperación parcial entre los 26 y 34 s.
Estas variaciones muestran que, incluso durante la condición basal, la amplitud de la señal PPG no permanece completamente constante, debido a la variabilidad fisiológica y a posibles cambios en las condiciones de medición. Por esta razón, resulta conveniente utilizar un parámetro representativo de múltiples latidos, como la mediana de las amplitudes pico-valle, en lugar de depender de un único pulso.
El valor obtenido durante esta etapa constituye la referencia basal de la amplitud PPG, necesaria para compararla posteriormente con la amplitud registrada durante el Cold Pressor Test y determinar los cambios en la perfusión periférica utilizados para el cálculo del índice SPI..

Por lo tanto, para interpretar las Figuras 4 y 5 se considera:

<div align="center">

| Tiempo en gráfica procesada | Condición experimental |
| :-------------------------: | :--------------------: |
|    0–18 s aproximadamente   |    Condición normal    |
|   18–38 s aproximadamente   |    Cold Pressor Test   |

</div>

Después del inicio aproximado del CPT se observa una reducción importante de PPGA, llegando en varios pulsos a valores cercanos a 1000–1500 unidades.

Este comportamiento resulta relevante para el cálculo del SPI, debido a que PPGA representa la variable con mayor ponderación dentro de la ecuación utilizada.

---

# 9. Cálculo del intervalo entre latidos (HBI)

El intervalo entre latidos o **Heart Beat Interval (HBI)** se calculó utilizando la diferencia temporal entre dos máximos consecutivos:

$$
HBI_i=t_{i+1}-t_i
$$

donde $t_i$ representa el tiempo correspondiente al máximo del latido $i$.

Para la captura completa, MATLAB obtuvo:

$$
\boxed{HBI_{promedio}=0.581\ \text{s}}
$$

Este parámetro fue utilizado posteriormente, junto con PPGA, para estimar el SPI.

---

# 10. Cálculo del SPI

Para calcular el índice pletismográfico quirúrgico se utilizaron las variables HBI y PPGA.

Primero se realizó una normalización mínimo-máximo de ambas variables en una escala entre 0 y 100.

### Normalización del HBI

$$
HBI_{norm}=100\left(\frac{HBI-HBI_{min}}{HBI_{max}-HBI_{min}}\right)
$$

### Normalización del PPGA

$$
PPGA_{norm}=100\left(\frac{PPGA-PPGA_{min}}{PPGA_{max}-PPGA_{min}}\right)
$$

Posteriormente se calculó el SPI mediante:

$$
SPI=100-\left(0.30HBI_{norm}+0.70PPGA_{norm}\right)
$$

La amplitud PPG presenta una ponderación del 70 %, mientras que el intervalo entre latidos presenta una ponderación del 30 %.

Para la captura completa se obtuvieron los siguientes resultados:

<div align="center">

|            Parámetro           |   Resultado  |
| :----------------------------: | :----------: |
|   Latidos utilizados para SPI  |    **64**    |
|          HBI promedio          |  **0.581 s** |
| PPGA promedio utilizado en SPI | **1650.344** |
|       SPI promedio total       |   **53.06**  |
|           SPI mínimo           |   **11.12**  |
|           SPI máximo           |   **93.90**  |

</div>

Por lo tanto, para toda la adquisición:

$$
\boxed{SPI_{promedio}=53.06}
$$

con un intervalo observado entre:

$$
\boxed{11.12\leq SPI\leq93.90}
$$

---

# 11. Resultados del SPI durante condición normal y Cold Pressor Test

### Evolución temporal del SPI

![SPI durante condición normal y CPT](Figure_5.png)

**Figura 5.** Evolución temporal del SPI estimado durante la condición normal y la aplicación del Cold Pressor Test.

La Figura 5 constituye el principal resultado del experimento, ya que permite observar la evolución del SPI dentro de una misma captura que contiene las dos condiciones experimentales.

La adquisición original fue realizada de la siguiente manera:

$$
\underbrace{0-20\ \text{s}}_{\text{Normal}}
\quad\Big|\quad
\underbrace{20-40\ \text{s}}_{\text{CPT}}
$$

Sin embargo, como durante el procesamiento se eliminaron los primeros 2 segundos, en la Figura 5 la transición debe interpretarse aproximadamente como:

$$
\underbrace{0-18\ \text{s}}_{\text{Normal}}
\quad\Big|\quad
\underbrace{18-38\ \text{s}}_{\text{CPT}}
$$

---

## 11.1 Condición normal

Durante la primera etapa de la gráfica, correspondiente aproximadamente al intervalo entre 0 y 18 segundos, el SPI presenta variaciones considerables.

Inicialmente se observan valores aproximadamente entre 40 y 70. Posteriormente el índice disminuye y alcanza valores cercanos al mínimo registrado.

El mínimo de toda la adquisición fue:

$$
\boxed{SPI_{min}=11.12}
$$

Durante una parte importante de la condición normal se observan valores aproximadamente entre 10 y 50.

A partir de la inspección visual de los puntos de la Figura 5, el valor medio durante esta condición puede estimarse aproximadamente en:

$$
\boxed{SPI_{normal}\approx35}
$$

Este valor corresponde a una **estimación visual de la gráfica** y no a un promedio calculado directamente por MATLAB.

---

## 11.2 Cold Pressor Test

El Cold Pressor Test comenzó experimentalmente en el segundo 20 de adquisición. Debido a la eliminación de los primeros 2 segundos, su inicio corresponde aproximadamente al segundo 18 de la gráfica procesada.

Después de esta transición se observa un incremento importante del SPI.

Al comienzo del periodo correspondiente al CPT aparecen valores aproximadamente entre 60 y 70, seguidos de un aumento que lleva al índice por encima de 80.

Durante esta condición se alcanzó el máximo registrado durante toda la captura:

$$
\boxed{SPI_{max}=93.90}
$$

Posteriormente se observa una disminución temporal del índice y nuevas variaciones durante el resto de la prueba. Hacia el final del registro vuelve a presentarse un incremento considerable, alcanzando nuevamente valores cercanos a 90.

A partir de la inspección visual de la Figura 5, el valor medio durante el CPT puede estimarse aproximadamente en:

$$
\boxed{SPI_{CPT}\approx68}
$$

Nuevamente, este valor corresponde a una estimación visual y no a un promedio calculado directamente a partir del vector de SPI.

---

## 11.3 Comparación entre ambas condiciones

A partir de la gráfica se observa una diferencia entre las dos condiciones experimentales.

<div align="center">

|     Condición     | Intervalo aproximado en la gráfica | SPI estimado visualmente |
| :---------------: | :--------------------------------: | :----------------------: |
|       Normal      |               0–18 s               |         **≈ 35**         |
| Cold Pressor Test |               18–38 s              |         **≈ 68**         |

</div>

La diferencia aproximada entre ambas condiciones es:

$$
\Delta SPI=
SPI_{CPT}-SPI_{normal}
$$

$$
\Delta SPI\approx68-35
$$

$$
\boxed{\Delta SPI\approx33}
$$

Por lo tanto, a partir de la inspección visual de la gráfica se observa un incremento aproximado de **33 unidades de SPI** durante el periodo correspondiente al Cold Pressor Test.

Debe diferenciarse este resultado del promedio total calculado directamente por MATLAB:

$$
\boxed{SPI_{promedio,total}=53.06}
$$

El valor de 53.06 corresponde a toda la captura analizada, mientras que los valores aproximados de 35 y 68 corresponden a estimaciones visuales utilizadas para comparar las dos etapas del experimento.

---

# 12. Relación entre PPGA y SPI durante el CPT

Al comparar las Figuras 4 y 5 se observa que alrededor del periodo correspondiente al inicio del Cold Pressor Test se presenta una reducción de la amplitud PPG acompañada de un aumento del SPI.

Esta relación es coherente con la ecuación implementada:

$$
SPI=
100-
\left(
0.30HBI_{norm}
+
0.70PPGA_{norm}
\right)
$$

Debido a que PPGA presenta una ponderación de 0.70, una reducción de su valor normalizado contribuye al aumento del SPI:

$$
\downarrow PPGA
$$

$$
\downarrow PPGA_{norm}
$$

$$
\uparrow SPI
$$

Los resultados experimentales muestran precisamente una modificación de estas variables durante el periodo correspondiente al CPT.

De manera general, el comportamiento observado puede representarse como:

$$
\text{Cold Pressor Test}
\rightarrow
\text{respuesta autonómica}
\rightarrow
\text{cambios cardiovasculares y vasculares}
\rightarrow
\text{modificación de PPGA y HBI}
\rightarrow
\text{modificación del SPI}
$$

Los resultados obtenidos muestran que el sistema desarrollado fue sensible al cambio de condición experimental, observándose valores de SPI mayores durante una parte importante del Cold Pressor Test en comparación con la condición normal.

