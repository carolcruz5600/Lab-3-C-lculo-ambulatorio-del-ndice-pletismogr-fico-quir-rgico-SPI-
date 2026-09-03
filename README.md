# Laboratorio 3: Cálculo ambulatorio del índice pletismogrófico quirúrgico (SPI)

# Cálculo ambulatorio del índice pletismográfico quirúrgico (SPI)

## Objetivos

### Objetivo general

Desarrollar un sistema de medición continua del índice pletismográfico quirúrgico (SPI) en condiciones ambulatorias.

### Objetivos específicos

- Reconocer las características fundamentales de la onda de pulso a partir de las cuales se obtiene el SPI.
- Construir un sistema que calcule el SPI en tiempo real y bajo condiciones ambulatorias.
- Validar el funcionamiento del sistema desarrollado mediante un método que induzca una respuesta fisiológica similar a la que produce el dolor agudo.

## Introducción

La monitorización de variables fisiológicas constituye una herramienta fundamental para evaluar de manera objetiva los cambios que experimenta el organismo frente a diferentes estímulos. En particular, durante procedimientos quirúrgicos realizados bajo anestesia general resulta importante determinar el equilibrio existente entre la estimulación nociceptiva y el efecto de los agentes analgésicos.

La nocicepción corresponde al proceso mediante el cual el sistema nervioso detecta y procesa estímulos potencialmente dañinos. Este concepto debe diferenciarse del dolor, ya que este último corresponde a una experiencia sensorial y emocional consciente y subjetiva. Durante la anestesia general, la evaluación directa del dolor resulta limitada debido al estado de inconsciencia del paciente, por lo que se utilizan diferentes variables fisiológicas relacionadas con la actividad del sistema nervioso autónomo para estimar indirectamente el balance entre nocicepción y antinocicepción.

Una de las señales fisiológicas que permite estudiar estos cambios es la señal fotopletismográfica o PPG (*Photoplethysmography*). La fotopletismografía es una técnica óptica no invasiva que permite detectar variaciones en el volumen sanguíneo periférico. Estas variaciones se producen principalmente como consecuencia de los cambios en el volumen arterial asociados con cada ciclo cardíaco.

La señal PPG puede obtenerse mediante un sistema compuesto por una fuente de luz y un fotodetector. Cuando la luz incide sobre el tejido, una parte es absorbida y otra es reflejada o transmitida. La cantidad de radiación detectada cambia debido a las variaciones periódicas del volumen de sangre presentes en el tejido durante cada latido cardíaco.

A partir de la señal PPG pueden extraerse características como la amplitud de cada onda de pulso y el intervalo temporal existente entre pulsaciones consecutivas. Estas variables contienen información relacionada con la actividad cardiovascular y autonómica del individuo.

El índice pletismográfico quirúrgico o SPI (*Surgical Pleth Index*) utiliza características derivadas de la señal fotopletismográfica para proporcionar una medida relacionada con el balance entre nocicepción y antinocicepción. El índice toma valores comprendidos entre 0 y 100, donde valores mayores se asocian con una mayor respuesta fisiológica relacionada con la estimulación nociceptiva.

En esta práctica se desarrolla un sistema para adquirir la señal PPG mediante un sensor óptico de reflectancia y un circuito analógico de acondicionamiento. El circuito permite convertir las pequeñas variaciones ópticas producidas por los cambios en el volumen sanguíneo periférico en una señal eléctrica susceptible de ser digitalizada.

Para ello se utilizan diferentes etapas de acondicionamiento, entre las cuales se encuentran la excitación del emisor óptico, detección mediante un fototransistor, eliminación de la componente continua mediante un filtro pasaaltas, amplificación de la señal pulsátil, filtrado pasabajas y ajuste de la amplitud y del nivel de referencia de la señal.

Posteriormente, la señal acondicionada puede ser adquirida mediante el convertidor analógico-digital de un microcontrolador, como una ESP32, y transmitida hacia MATLAB. Allí pueden implementarse algoritmos para identificar máximos y mínimos de cada onda de pulso, calcular la amplitud pletismográfica, determinar el intervalo entre latidos y finalmente estimar el SPI.

La respuesta del sistema puede evaluarse mediante una condición experimental capaz de producir cambios en la actividad autonómica. Para esta práctica se propone utilizar el *Cold Pressor Test* (CPT) y comparar las características de la señal PPG y del SPI antes, durante y después de la aplicación del estímulo.

# Marco teórico

## Nocicepción y dolor

Aunque los conceptos de dolor y nocicepción se encuentran relacionados, no representan exactamente el mismo fenómeno.

La nocicepción corresponde al procesamiento neural de estímulos potencialmente dañinos. Los nociceptores detectan estímulos mecánicos, térmicos o químicos y generan señales que son transmitidas hacia el sistema nervioso central.

El dolor, por otra parte, constituye una experiencia consciente y subjetiva asociada con componentes sensoriales y emocionales. Por esta razón, la presencia de actividad nociceptiva no necesariamente implica una percepción consciente de dolor.

Esta diferencia resulta especialmente importante durante la anestesia general. Un paciente anestesiado puede no experimentar conscientemente dolor y, sin embargo, su organismo puede presentar respuestas autonómicas ante determinados estímulos quirúrgicos.

Entre estas respuestas pueden encontrarse modificaciones en:

- Frecuencia cardíaca.
- Presión arterial.
- Tono vascular periférico.
- Amplitud de la señal fotopletismográfica.
- Intervalo entre latidos.

El análisis de estas variables permite obtener información indirecta sobre la respuesta fisiológica asociada con la nocicepción.


## Sistema nervioso autónomo

El sistema nervioso autónomo participa en la regulación involuntaria de diferentes funciones fisiológicas, entre ellas la frecuencia cardíaca, presión arterial, respiración y tono vascular.

Está constituido principalmente por los sistemas simpático y parasimpático.

La actividad simpática está asociada con respuestas fisiológicas frente a situaciones que requieren una adaptación rápida del organismo. Un incremento de la actividad simpática puede producir cambios como:

- Incremento de la frecuencia cardíaca.
- Modificaciones de la presión arterial.
- Vasoconstricción periférica.
- Cambios en el volumen sanguíneo periférico.

La vasoconstricción periférica resulta especialmente importante para esta práctica porque modifica la cantidad de sangre presente en los vasos sanguíneos de las extremidades.

Debido a que la fotopletismografía detecta cambios en el volumen sanguíneo periférico, una modificación del tono vascular puede observarse como un cambio en la amplitud de la señal PPG.

De manera simplificada:

**Aumento de actividad simpática → vasoconstricción periférica → modificación del volumen sanguíneo periférico → cambio en la amplitud PPG.**

De forma simultánea pueden producirse modificaciones en la frecuencia cardíaca y, por tanto, en el intervalo temporal existente entre pulsaciones consecutivas.

Estas dos características, amplitud de pulso e intervalo entre latidos, son precisamente las variables utilizadas para obtener el SPI.


## Fotopletismografía

La fotopletismografía, conocida como PPG por sus siglas en inglés (*Photoplethysmography*), es una técnica óptica no invasiva utilizada para detectar variaciones en el volumen sanguíneo presente en un tejido.

Un sistema PPG básico está compuesto por:

**Fuente de luz → tejido → fotodetector**

La fuente de luz ilumina una región del cuerpo, mientras que el fotodetector registra las modificaciones producidas en la intensidad luminosa debido a las propiedades ópticas del tejido y a los cambios del volumen sanguíneo.

Existen principalmente dos configuraciones para realizar una medición fotopletismográfica:

### PPG por transmisión

El emisor y el detector se encuentran ubicados en lados opuestos del tejido.

La luz atraviesa el tejido antes de llegar al detector.

Esta configuración es utilizada, por ejemplo, en muchos sensores colocados en el dedo.

### PPG por reflectancia

El emisor y el detector se encuentran ubicados aproximadamente sobre la misma superficie.

La luz emitida penetra en el tejido y una parte regresa hacia el fotodetector debido a fenómenos de dispersión y reflexión.

El circuito desarrollado en esta práctica utiliza este principio de funcionamiento mediante un sensor TCRT1000 o un TCST110 adaptado para funcionar como sensor de reflectancia.


## Componentes AC y DC de la señal PPG

La señal obtenida mediante fotopletismografía puede considerarse formada por dos componentes principales: una componente continua o DC y una componente alterna o AC.

La señal total puede representarse mediante:

$$
PPG(t)=PPG_{DC}+PPG_{AC}(t)
$$

La componente $PPG_{DC}$ representa el nivel aproximadamente constante de la señal y está relacionada con factores como:

- Tejidos.
- Sangre no pulsátil.
- Posición del sensor.
- Intensidad luminosa.
- Geometría entre emisor, dedo y detector.
- Condiciones ópticas externas.

La componente $PPG_{AC}$ corresponde principalmente a las variaciones pulsátiles relacionadas con los cambios del volumen arterial producidos durante cada ciclo cardíaco.

Generalmente, la amplitud de la componente pulsátil es considerablemente menor que la componente continua:

$$
PPG_{AC} \ll PPG_{DC}
$$

Esto significa que la información correspondiente a las variaciones del volumen sanguíneo producidas por cada pulsación puede encontrarse superpuesta sobre un nivel continuo de amplitud considerablemente mayor.

Por esta razón, el circuito de acondicionamiento debe eliminar o reducir la componente DC y posteriormente amplificar la componente pulsátil de interés.

En el circuito implementado, esta función se realiza principalmente mediante el filtro pasaaltas formado por el condensador de $4.7\ \mu F$ y la resistencia de $47\ k\Omega$. Posteriormente, la componente pulsátil es amplificada mediante el LM358.


## Onda de pulso

Cada ciclo cardíaco genera una variación del volumen sanguíneo periférico que puede observarse en la señal PPG como una onda de pulso.

De cada pulsación pueden extraerse diferentes características. Para esta práctica resultan especialmente importantes:

- Máximo de la onda.
- Mínimo de la onda.
- Amplitud de la onda.
- Instante de aparición de cada pulso.
- Intervalo entre pulsaciones consecutivas.

Estas características permiten obtener las variables necesarias para el cálculo experimental del índice pletismográfico quirúrgico.


## Amplitud de la onda pletismográfica

La amplitud de cada pulso puede calcularse mediante la diferencia entre un máximo y el mínimo correspondiente de la señal.

Esta variable se denomina **PPGA** (*Plethysmographic Pulse Wave Amplitude*) y puede calcularse como:

$$
PPGA = V_{max}-V_{min}
$$

donde:

- $PPGA$: amplitud de la onda pletismográfica.
- $V_{max}$: valor máximo de la pulsación.
- $V_{min}$: valor mínimo asociado a la pulsación.

Por ejemplo, si se obtiene:

$$
V_{max}=2.10\ V
$$

y:

$$
V_{min}=1.70\ V
$$

entonces:

$$
PPGA=2.10-1.70
$$

por lo tanto:

$$
\boxed{PPGA=0.40\ V}
$$

Esta variable permite cuantificar las modificaciones de amplitud que experimenta la señal PPG entre diferentes condiciones fisiológicas.

Durante el procesamiento realizado en MATLAB, los máximos y mínimos de cada pulsación pueden identificarse automáticamente para calcular el PPGA correspondiente a cada latido.


## Intervalo entre latidos

Otra característica fundamental de la señal corresponde al intervalo temporal existente entre dos pulsaciones consecutivas.

Si los máximos de dos pulsaciones consecutivas aparecen en los tiempos $t_n$ y $t_{n+1}$, el intervalo entre latidos puede calcularse mediante:

$$
HBI_n=t_{n+1}-t_n
$$

donde $HBI$ corresponde al *Heart Beat Interval*.

Por ejemplo, si:

$$
t_n=10.20\ s
$$

y:

$$
t_{n+1}=11.00\ s
$$

entonces:

$$
HBI=11.00-10.20
$$

$$
\boxed{HBI=0.80\ s}
$$

A partir del intervalo entre latidos también puede calcularse la frecuencia cardíaca:

$$
HR=\frac{60}{HBI}
$$

Para el ejemplo anterior:

$$
HR=\frac{60}{0.80}
$$

por lo tanto:

$$
\boxed{HR=75\ bpm}
$$

De esta manera, una reducción del intervalo entre latidos representa un incremento de la frecuencia cardíaca, mientras que un aumento del intervalo representa una disminución de esta.


## Índice pletismográfico quirúrgico (SPI)

El **SPI** (*Surgical Pleth Index*) es un índice empleado para obtener información relacionada con la respuesta fisiológica asociada con la estimulación nociceptiva.

El cálculo del índice utiliza principalmente dos características derivadas de la señal pletismográfica:

- Amplitud normalizada de la onda PPG ($PPGA_{norm}$).
- Intervalo normalizado entre latidos ($HBI_{norm}$).

La expresión matemática reportada para el índice es:

$$
\boxed{
SPI=100-\left(0.7PPGA_{norm}+0.3HBI_{norm}\right)
}
$$

donde:

- $PPGA_{norm}$: amplitud normalizada de la onda pletismográfica.
- $HBI_{norm}$: intervalo entre latidos normalizado.

La expresión asigna una ponderación del **70 %** a la amplitud pletismográfica y del **30 %** al intervalo entre latidos.

Por tanto, dentro de la expresión matemática del SPI, la amplitud de la señal PPG presenta una contribución mayor que el intervalo entre pulsaciones.

El índice se expresa dentro de una escala comprendida entre:

$$
0\leq SPI\leq100
$$

Los valores más altos del SPI se relacionan con una mayor respuesta nociceptiva.

De acuerdo con la guía de laboratorio, el rango objetivo utilizado como referencia para una analgesia intraoperatoria adecuada durante anestesia general suele encontrarse entre 20 y 50. Asimismo, se indica que deben evitarse incrementos del SPI superiores a 10 unidades.


## Normalización de las variables del SPI

Para utilizar la ecuación del SPI no se introducen directamente la amplitud de la señal en voltios y el intervalo entre latidos en segundos.

Primero deben obtenerse las variables normalizadas:

$$
PPGA_{norm}
$$

y:

$$
HBI_{norm}
$$

De manera general, el proceso puede representarse como:

$$
PPGA \longrightarrow PPGA_{norm}
$$

$$
HBI \longrightarrow HBI_{norm}
$$

para posteriormente calcular:

$$
SPI=100-\left(0.7PPGA_{norm}+0.3HBI_{norm}\right)
$$

La normalización permite expresar las variables dentro de una escala adecuada para combinarlas en el cálculo del índice.

Es importante señalar que el algoritmo comercial del SPI utiliza procedimientos específicos para realizar esta normalización. Por esta razón, si durante la práctica se implementa en MATLAB un procedimiento experimental de normalización, los resultados obtenidos deben considerarse una **estimación experimental del SPI basada en la ecuación publicada**, y no necesariamente una reproducción exacta del algoritmo utilizado por un monitor clínico comercial.


# Cold Pressor Test (CPT)

El *Cold Pressor Test* (CPT) es una técnica experimental utilizada para producir una respuesta fisiológica mediante la exposición controlada al frío.

El estímulo térmico puede generar una respuesta del sistema nervioso autónomo y producir modificaciones cardiovasculares observables en variables como:

- Frecuencia cardíaca.
- Presión arterial.
- Tono vascular periférico.
- Amplitud de la señal fotopletismográfica.
- Intervalo entre pulsaciones.

En el contexto de esta práctica, el CPT se utiliza para generar una modificación fisiológica que pueda ser registrada mediante el sistema de adquisición PPG y posteriormente evaluada mediante las variables empleadas para calcular el SPI.

La adquisición planteada tiene una duración total de:

$$
T=120\ s
$$

correspondiente a dos minutos.

El registro se divide en tres periodos:

| Intervalo | Duración | Condición |
|---|---:|---|
| $0-40\ s$ | 40 s | Condición inicial |
| $40-80\ s$ | 40 s | Aplicación del CPT |
| $80-120\ s$ | 40 s | Recuperación |

Durante los primeros 40 segundos se registra la señal PPG del voluntario en condiciones iniciales.

Cuando transcurren 40 segundos se inicia la maniobra CPT y se mantiene durante los siguientes 40 segundos mientras continúa la adquisición.

Al alcanzar los 80 segundos finaliza la maniobra y el voluntario regresa a las condiciones iniciales. La adquisición continúa durante otros 40 segundos hasta completar los 120 segundos.

Esto permite comparar tres condiciones experimentales:

$$
PPG_{reposo}
\longrightarrow
PPG_{CPT}
\longrightarrow
PPG_{recuperacion}
$$

y, posteriormente:

$$
SPI_{reposo}
\longrightarrow
SPI_{CPT}
\longrightarrow
SPI_{recuperacion}
$$

La temperatura, el procedimiento específico para realizar la exposición al frío y los criterios de seguridad deben establecerse de acuerdo con el protocolo autorizado por el laboratorio y las indicaciones del docente responsable.
