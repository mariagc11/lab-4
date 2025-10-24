# lab-4 SEÑALES ELECTROMIOGRAFICAS (EMG)

# INTRODUCCIÓN 
El estudio de las señales electromiográficas (EMG) permite analizar la actividad eléctrica generada por los músculos durante la contracción, proporcionando información fundamental sobre su funcionamiento y estado de fatiga. Estas señales se registran mediante electrodos de superficie ubicados sobre la piel, que capturan los potenciales eléctricos producidos por las fibras musculares. A través del procesamiento digital de estas señales, es posible aplicar filtros y técnicas de análisis espectral, como la Transformada Rápida de Fourier (FFT), para observar la variación en la frecuencia mediana a medida que el músculo se fatiga. Este análisis resulta esencial en el campo de la ingeniería biomédica, ya que permite comprender el comportamiento fisiológico del músculo, desarrollar herramientas de diagnóstico no invasivas y mejorar el diseño de sistemas de monitoreo y rehabilitación muscular.

# MARCO TEORICO 

1. Electromiografía (EMG):
La electromiografía es una técnica que permite registrar la actividad eléctrica producida por los músculos durante su contracción. Esta señal refleja el funcionamiento neuromuscular y es utilizada para estudiar la respuesta fisiológica del músculo ante distintos niveles de esfuerzo o fatiga.


3. Tipos de EMG:
Existen dos tipos principales de electromiografía: la EMG de superficie, que utiliza electrodos colocados sobre la piel y es un método no invasivo, y la EMG intramuscular, que emplea agujas insertadas directamente en el músculo para obtener señales más específicas.


4. Configuración del registro:
La señal EMG se obtiene mediante dos electrodos activos y un electrodo de referencia o tierra. La diferencia de potencial entre los electrodos activos representa la actividad muscular. La calidad del registro depende de la correcta ubicación, la distancia entre electrodos (w) y la profundidad de las fibras musculares (d).


5. Adquisición de la señal:
Para garantizar un registro adecuado, los electrodos deben colocarse sobre la piel limpia y seca, utilizando gel conductor para mejorar el contacto. Durante la adquisición, se deben controlar las condiciones de movimiento y evitar interferencias electromagnéticas que puedan alterar la señal.


6. Procesamiento digital de la señal:
El procesamiento digital es esencial para limpiar y analizar la señal EMG. Se aplican filtros pasa altas para eliminar el ruido de baja frecuencia y filtros pasa bajas para suprimir interferencias de alta frecuencia. Luego, se realiza un aventamiento, que consiste en dividir la señal en intervalos de tiempo utilizando ventanas como las de Hamming o Hanning.


7. Análisis espectral (Transformada de Fourier – FFT):
La Transformada Rápida de Fourier permite convertir la señal del dominio del tiempo al dominio de la frecuencia. Esto facilita el análisis del contenido espectral de la señal EMG y permite calcular la frecuencia mediana, un indicador útil para estudiar el comportamiento muscular.


8. Fatiga muscular:
La fatiga muscular se caracteriza por una disminución progresiva en la capacidad del músculo para mantener la contracción. En términos de la señal EMG, se observa como una reducción en la frecuencia mediana del espectro, lo que refleja una menor velocidad de conducción de las fibras musculares.


9. Importancia en ingeniería biomédica:
El análisis de señales EMG es fundamental en la ingeniería biomédica, ya que permite evaluar el rendimiento muscular, identificar signos de fatiga y desarrollar sistemas de rehabilitación y diagnóstico basados en el monitoreo de la actividad eléctrica muscular.

10:En señales biológicas como EMG, se busca capturar la energía global de cada ventana y observar cómo cambia la distribución de frecuencias en el tiempo (por ejemplo, para estudiar fatiga).
En estos casos, la ventana Hann:

Reduce mejor la fuga espectral (leakage) entre bandas.

Tiene una forma más “natural” para analizar energía muscular (sin introducir tanto sesgo en la amplitud real).

Suprime los bordes de manera más suave, lo que ayuda cuando se aplican muchas FFT consecutivas .

# METODOLOGÍA 

Instrumentación y prepación del sujeto de prueba 

* Se selecciona como músculo de análisis los trapecios que se encuentran ubicados en la espalda alta, hacia las escapulas en el lado interno.
  
* EL sujeto de prueba permaneció snetado de forma erguida. 

* La piel fue limpiada con alcohol para eliminar grasa y residuos que se encontraban allí, garantizando buena conductividad.

* Se utilizaron tres electrodos de superficie:
  
  * Dos electrodos activos colocados longitudinalmente sobre el vientre de los trapecios, separados aproximadamente 2cm. En nuestro caso boton rojo y amarillo.
    
  * Un electrodo de tierra colocado en el hueso de la columna donde ahi zona ósea no activa, este seria el boton verde.

* Se verifica la correcta conexión al sistema de adquisición como lo es el modulo EMG y la DAQ, al software de procesamiento en Python. 

AL proceso se le realiza una toma fotografica para visualizar los pasos anteriores como se encuentra a continuación:

![Imagen de WhatsApp 2025-10-14 a las 16 56 44_dd302a35](https://github.com/user-attachments/assets/b636bb70-461b-48c4-9d52-3cf43fdf248f)

<img width="739" height="1600" alt="image" src="https://github.com/user-attachments/assets/304c3602-09a2-434d-85df-395a7ac3d590" />

<img width="1170" height="627" alt="image" src="https://github.com/user-attachments/assets/d43738a0-a9a0-4eff-9076-272a420d5bbe" />

# ADQUISICIÓN DE LA SEÑAL 
Para la adquisicón de la señal se realiza una interfaz de adquisión directamente en Python implementando el codigo correspondiente que se encuentra adjunto a este informe. En la siguiente imagen se muestra la interfaz:

<img width="785" height="632" alt="image" src="https://github.com/user-attachments/assets/178e3f6e-2bd5-4368-b1b3-a83dc24a19dd" />

La frecuencia de muestreo fue de 1000 Hz, garantizando una resolución temporal suficiente para capturar los componentes de la señal electromiográfica. Durante la prueba, el sujeto realizo una contracción sostenida del gastrocnemio al ponerse repetidamente en puntas, manteniendo la posición hasta la aparición de una sensación de fatiga. 

La señal fue registrada de manera continua y en tiempo real, obteniéndo un archivo excel de la señal cruda, mostrando el comportamiento inicial y el posterior filtrado digital.

A partir de la adquisión de la señal se procede a implementar un codgio en Python donde se realizan los correspondientes analisis. Por lo cual a continuación se realiza la expñicación del codigo implementado:

# EXPLICACIÓN DE CODIGO 
Empezamos montando el codigo normal para la adquisicion de la señal EMG en pyton donde agregamos 7 partes importantes.

# Definicion de filtro Butterworth
```
# Parámetros del filtro
fs = 1000  # Frecuencia de muestreo en Hz
lowcut = 20   # Frecuencia de corte baja
highcut = 450 # Frecuencia de corte alta

# Frecuencia de Nyquist
nyquist = 0.5 * fs
low = lowcut / nyquist
high = highcut / nyquist

# Diseño del filtro Butterworth de cuarto orden
from scipy.signal import butter, filtfilt

b, a = butter(4, [low, high], btype='band')

```
En esta sección se diseña el filtro digital Butterworth que limpiará la señal electromiográfica.

* Se fijan las frecuencias de corte en 20 y 450 Hz, que abarcan el rango útil de las señales EMG.

* Se calcula la frecuencia de Nyquist, que equivale a la mitad de la frecuencia de muestreo (fs/2), y se normalizan las frecuencias de corte.

* Con butter(4, [low, high], btype='band') se construye un filtro pasa banda de 4º orden, caracterizado por tener una respuesta suave y sin oscilaciones abruptas.

El objetivo de este filtro es eliminar tanto el ruido de baja frecuencia (movimiento o interferencias de línea base) como el ruido de alta frecuencia (eléctrico o térmico), conservando solo las componentes musculares útiles.

# Aplicacion del filtro 
```
# Aplicación del filtro a la señal EMG cruda
import numpy as np

emg_raw = np.loadtxt('emg_data.csv', delimiter=',')  # Lectura de la señal cruda

# Eliminamos el componente DC (media)
emg_raw = emg_raw - np.mean(emg_raw)

# Aplicamos el filtro pasa banda Butterworth
emg_bp = filtfilt(b, a, emg_raw)

```
Esta parte aplica el filtro diseñado sobre la señal EMG obtenida en formato .csv.
Primero, se resta la media de la señal para centrarla en cero, eliminando el componente DC.
Luego, se usa la función filtfilt() para aplicar el filtro en ambas direcciones (hacia adelante y hacia atrás), lo que evita el desfase temporal que produciría un filtrado simple.

El resultado es emg_bp, una señal limpia, libre de interferencias, que conserva únicamente las frecuencias fisiológicas del músculo.

# Transformada Rapida de fourier FFT

```
# Aplicación del filtro a la señal EMG cruda
import numpy as np

emg_raw = np.loadtxt('emg_data.csv', delimiter=',')  # Lectura de la señal cruda

# Eliminamos el componente DC (media)
emg_raw = emg_raw - np.mean(emg_raw)

# Aplicamos el filtro pasa banda Butterworth
emg_bp = filtfilt(b, a, emg_raw)

```
Aquí se calcula la Transformada Rápida de Fourier (FFT), que convierte la señal del dominio del tiempo al dominio de la frecuencia.
La FFT permite observar en qué frecuencias se concentra la energía de la señal EMG.

El vector f contiene las frecuencias, y mag las amplitudes correspondientes.
La gráfica resultante muestra los picos de energía que reflejan la activación muscular, permitiendo identificar si la energía se desplaza hacia frecuencias bajas (síntoma de fatiga). 

# Ventaneo 
```
# Definición de parámetros de ventana
win_sec = 0.5  # Duración de ventana (s)
overlap = 0.5  # Porcentaje de solapamiento
step = int(fs * win_sec * (1 - overlap))  # Paso de ventana
win_size = int(fs * win_sec)

# Ventana Hamming
from scipy.signal import get_window
window = get_window('hamming', win_size)

# División de la señal en ventanas solapadas
segments = []
for start in range(0, len(emg_bp) - win_size, step):
    segment = emg_bp[start:start + win_size] * window
    segments.append(segment)
segments = np.array(segments)
```
El ventaneo consiste en dividir la señal en segmentos cortos para analizar cómo varían sus características a lo largo del tiempo.

* Cada ventana tiene una duración de 0.5 segundos.

* Se usa un 50% de solapamiento entre ventanas para no perder información.

* Se aplica una ventana Hamming, que suaviza los bordes de cada segmento y evita fugas espectrales al hacer la FFT.

El resultado es una lista de ventanas (segments), cada una lista para ser analizada individualmente.
 

# Implementacion de las medidas estadisticas 
```
means = []
stds = []
cvs = []

for seg in segments:
    mean_val = np.mean(np.abs(seg))
    std_val = np.std(seg)
    cv_val = std_val / mean_val if mean_val != 0 else 0

    means.append(mean_val)
    stds.append(std_val)
    cvs.append(cv_val)

```
En cada ventana se calculan las medidas estadísticas básicas de la señal EMG:

* Media: representa la amplitud media del esfuerzo muscular.

* Desviación estándar: indica la variabilidad o irregularidad del esfuerzo.

* Coeficiente de variación (CV): evalúa la relación entre variabilidad y magnitud.

Cuando el músculo entra en fatiga, suele observarse:

* Disminución de la media (menor activación).

* Aumento del CV (mayor inestabilidad eléctrica).

# Deteccion de fatiga
```
# Cálculo de frecuencia mediana en cada ventana
from scipy.signal import welch

fmeds = []
for seg in segments:
    f_vals, Pxx = welch(seg, fs=fs, nperseg=win_size)
    cum_energy = np.cumsum(Pxx)
    fmed = f_vals[np.where(cum_energy >= cum_energy[-1] / 2)[0][0]]
    fmeds.append(fmed)

# Tendencia de la frecuencia mediana
from scipy.stats import linregress

x = np.arange(len(fmeds))
slope, intercept, r, p_value, std_err = linregress(x, fmeds)

print("Pendiente:", slope)
print("p-valor:", p_value)

```
Aquí se realiza la detección de fatiga muscular mediante el análisis de la frecuencia mediana (f_med).

* Para cada ventana, se calcula el espectro de potencia con el método de Welch, y luego se obtiene la frecuencia donde la energía acumulada alcanza el 50%.

* Posteriormente, se aplica una regresión lineal a la serie temporal de f_med para observar su tendencia.

* Si la pendiente (slope) es negativa y el p-valor < 0.05, significa que la disminución es estadísticamente significativa, indicando fatiga muscular.
# Graficas 
```
import pandas as pd
import os

# Crear carpeta de resultados
os.makedirs('resultados', exist_ok=True)

# Guardar medidas estadísticas
df = pd.DataFrame({
    'Ventana': np.arange(len(means)),
    'Media': means,
    'DesvEst': stds,
    'CV': cvs,
    'Frecuencia_Mediana': fmeds
})
df.to_excel('resultados/analisis_emg.xlsx', index=False)

# Guardar gráficas
plt.figure()
plt.plot(fmeds)
plt.title('Frecuencia Mediana')
plt.savefig('resultados/fmed.png')

```
Finalmente, todos los resultados se exportan automáticamente a una carpeta llamada resultados:

* Los valores numéricos (media, desviación estándar, CV y f_med) se guardan en un archivo Excel (.xlsx) para análisis posterior.

* Las gráficas se almacenan en formato PNG con nombres identificativos.
  
# Exportacion de resultados 

por ultímo se exportan los resultados a una carpeta propia donde se guardan en exel los datos de la señal y calculos estadisticos y en png las graficas

# ANÁLISIS DE SEALES ELECTROMIOGRÁFICAS (EMG)

Este repositorio contiene el desarrollo y los resultados del análisis electromiográfico realizado a partir de un archivo CSV de señal EMG.  
El código implementa **filtrado digital**, **ventaneo**, **análisis espectral**, **detección de fatiga** y **evaluación estadística** entre ventanas según los criterios del laboratorio.  

# DESCRIPCIÓN GENERAL DEL PROCESO 

El script realiza los siguientes pasos:
1. **Lectura del archivo EMG (.csv)** y cálculo de la frecuencia de muestreo.
2. **Filtrado digital** con filtros Butterworth pasa-altas y pasa-bajas (20–450 Hz).
3. **Cálculo de la FFT global** para observar la distribución espectral total.
4. **Segmentación de la señal** en ventanas largas (30 s) y cortas (0.5 s con 50 % de solape).
5. **Cálculo de la frecuencia mediana (f_med)** en cada ventana y generación del **mapa espectral**.
6. **Análisis estadístico de fatiga** (pendiente, prueba t de Welch y cambio porcentual).
7. **Cálculo de parámetros estadísticos** (media, desviación estándar, SNR, FFR).
8. **Evaluación de significancia espectral** por bandas y visualización de resultados.

---

## 🧩 Resultados y explicación de las figuras


### 1️⃣ EMG cruda vs filtrada
![Imagen de WhatsApp 2025-10-22 a las 19 22 26_d9533141](https://github.com/user-attachments/assets/5e9ba250-8c78-41b1-b3d6-be60b2337375)
En esta gráfica se compara la señal **EMG cruda** (azul) con la **EMG filtrada** (naranja) tras aplicar filtros pasa-altas y pasa-bajas de 20 a 450 Hz.  
El filtrado elimina el componente DC y el ruido de baja frecuencia proveniente de movimiento o interferencia eléctrica, permitiendo conservar únicamente la actividad eléctrica muscular relevante.  
Se observa una reducción notable en la amplitud y mayor limpieza en la señal filtrada, evidenciando que los filtros fueron aplicados correctamente y que la energía útil de la contracción muscular se mantiene dentro de la banda esperada.

---

### 2️⃣ FFT global de la señal EMG filtrada
<img width="1212" height="394" alt="image" src="https://github.com/user-attachments/assets/e6af6321-90f9-411d-a708-beff9c5d46d3" />

Esta figura muestra el **análisis en frecuencia de toda la señal EMG** mediante la Transformada Rápida de Fourier (FFT).  
Se observa un pico principal alrededor de los **10 Hz**, indicando que gran parte de la energía se concentra en ese rango, lo cual es característico de señales musculares activas.  
Además, el espectro decae hacia frecuencias altas, lo que confirma que el contenido útil se encuentra principalmente entre 0 y 100 Hz.

---

### 3️⃣ Frecuencia mediana (*f_med*) por ventana de 30 s
<img width="1202" height="491" alt="image" src="https://github.com/user-attachments/assets/712def89-4aee-44f6-844d-9a8799674ec3" />

Aquí se representa la evolución de la **frecuencia mediana** (*f_med*) a lo largo del tiempo, calculada en ventanas de 30 s sin solape.  
Los valores se mantienen alrededor de **40 a 140 Hz**, con variaciones entre las siete ventanas analizadas.  
La estabilidad de *f_med* sugiere una contracción muscular mantenida y con signos de fatiga durante el registro.

---

### 4️⃣ Consola de resultados del ventaneo largo
<img width="1200" height="401" alt="image" src="https://github.com/user-attachments/assets/3d3301bb-5299-4e57-b3d2-3ee519e7b96b" />

En este bloque se observan los datos generados por el programa para cada ventana de 30 s, indicando las posiciones de las muestras, el tiempo correspondiente y la *f_med* calculada.  
Se registraron **siete ventanas**, todas con frecuencias medianas próximas entre los 70 y 110 Hz, lo cual respalda la consistencia espectral del músculo a lo largo del ensayo.

---

### 5️⃣ Mapa espectral por ventanas (30 s, Hamming)
<img width="1109" height="417" alt="image" src="https://github.com/user-attachments/assets/060c5bc3-e521-4682-911f-835b7603a9c0" />

El mapa espectral representa la **distribución de potencia en función del tiempo y la frecuencia** para las ventanas largas.  
Los colores cálidos (rojo y amarillo) muestran las zonas de mayor intensidad de señal, ubicadas entre 20 y 400 Hz, mientras que los tonos fríos (azul) representan menor potencia.  
Se observa una densidad espectral estable a lo largo del tiempo, sin desplazamientos hacia frecuencias altas, lo cual indica que la actividad muscular se mantuvo constante y **se evidencia la fatiga en mayor parte al final de la señal** durante el periodo analizado.

---

### 6️⃣ Resultados numéricos de detección de fatiga
<img width="1113" height="252" alt="image" src="https://github.com/user-attachments/assets/43932125-97cf-4a49-8a43-0aee4cdc4d3f" />

Este recuadro presenta los resultados del **análisis estadístico de fatiga**.  
La pendiente de la *f_med* (-0.04579 Hz/s) es prácticamente nula y no significativa (p = 0.2241), con una caída porcentual del 7.51 %, con un umbral de 5% 
Esto significa que se detecta una reducción relevante de la frecuencia mediana, por lo que el músculo ** presentó signos de fatiga electromiográfica** a lo largo del registro.

---

### 7️⃣ Gráfica de *f_med* y decisión de fatiga
<img width="1206" height="382" alt="image" src="https://github.com/user-attachments/assets/10a38dff-d207-411e-a397-8547d169b30c" />

En esta figura se visualiza la evolución de la *f_med* junto con la línea de tendencia (naranja discontinua) y las zonas de comparación entre el bloque inicial y final.  
La tendencia casi horizontal y los valores similares al inicio y al final reafirman que **la potencia espectral del músculo se mantuvo estable**, sin desplazamiento hacia frecuencias más bajas.  
Por lo tanto, el sistema detecta correctamente que **no hubo fatiga muscular** durante el ensayo.

---

### 8️⃣ Señal segmentada en 728 ventanas de 0.5 s
![Imagen de WhatsApp 2025-10-22 a las 19 24 20_c2079646](https://github.com/user-attachments/assets/17990348-8aa0-4f82-a964-3093e3e390af)

Aquí se muestra la **señal completa segmentada en 728 ventanas cortas de 0.5 s con un 50 % de solape**.  
Cada color representa una ventana distinta utilizada para el análisis estadístico y espectral de corto plazo.  
Este ventaneo mejora la resolución temporal del análisis, permitiendo observar variaciones locales en la contracción muscular y preparar los datos para calcular parámetros como la media, desviación estándar y SNR en cada segmento.
<img width="956" height="58" alt="image" src="https://github.com/user-attachments/assets/b1851277-0eb8-4dbe-8d4c-daee98523562" />


---

### 9️⃣ Parámetros calculados por ventana corta
<img width="1202" height="589" alt="image" src="https://github.com/user-attachments/assets/7379221d-d295-44a5-a771-5712a6849a9d" />

Esta figura muestra la evolución de varios **parámetros estadísticos** (media, desviación estándar, coeficiente de variación, SNR y FFR) calculados en cada una de las 728 ventanas.  
Los valores se mantienen estables, sin picos anómalos que indiquen variaciones abruptas en la señal.  
El FFR (frecuencia media ponderada) permanece dentro de rangos medios, reforzando la ausencia de cambios espectrales que sugieran fatiga.  
Este análisis confirma que **la señal EMG es estable y fisiológicamente consistente** durante toda la medición.

---


### 🔟 Espectro promedio (ventanas de 0.5 s, 50 % de solape)
<img width="1209" height="476" alt="image" src="https://github.com/user-attachments/assets/80823182-7bc8-40d6-8100-a2e58dabc366" />

Esta figura representa el **espectro promedio** obtenido a partir de todas las ventanas cortas de 0.5 s con 50 % de solape.  
La curva muestra cómo se distribuye la potencia de la señal EMG en el dominio de la frecuencia, expresada en decibelios (dB). 
Se nota una alza de potencia entre los **20 y los 450 Hz **correspondientes a la actividad muscular voluntaria, y una disminucion a frecuencias altas donde se evidencia el comiezo de la fatiga.  
---
### 1️⃣1️⃣ FFT POR VENTANAS 
<img width="1197" height="598" alt="image" src="https://github.com/user-attachments/assets/05da7556-5b54-41df-bc7f-c4360717da99" />

Figura 1 — “FFT por ventanas (subconjunto) – 0.5 s, 50% solape”
El gráfico superpone decenas de espectros en dB de ventanas cortas (0.5 s) y muestra la variabilidad espectral del EMG a lo largo del tiempo. Se observa el ascenso rápido desde 0 Hz hasta la banda útil del EMG (~20–30 Hz) y luego un máximo amplio entre ~40–80 Hz, con curvas que difieren en nivel pero comparten la forma general. A partir de ~100 Hz la potencia decrece de manera progresiva, manteniéndose un “suelo” entre −20 y −50 dB según la ventana, y por encima de ~300–400 Hz la energía es baja, coherente con el filtrado pasa-banda y con la naturaleza de la señal muscular. En conjunto, el “abanico” de curvas ilustra cambios de activación/contracción entre ventanas, manteniendo la banda dominante en bajas–medias frecuencias.

---
### 1️⃣2️⃣️ COMPARACION DE VENTANAS DE INICIO, MEDIO Y FINAL 
<img width="1204" height="584" alt="image" src="https://github.com/user-attachments/assets/2dd21dea-23c5-44d5-aeac-62607ae39da6" />

Figura 2 — “FFT – ventanas: inicio vs medio vs final”
Aquí se comparan tres espectros representativos: inicio (azul), mitad (naranja) y final (verde). Las ventanas de inicio y mitad muestran mayor potencia global y picos en la banda típica del EMG (~40–80 Hz), con energía apreciable hasta ~200–250 Hz. En contraste, la ventana final (verde) aparece deprimida en prácticamente todo el espectro (≈15–30 dB por debajo), lo que sugiere una disminución de la amplitud/actividad en esa fase del registro (por ejemplo, relajación o fatiga local). También se aprecian valles estrechos (notch/dips) puntuales, compatibles con variaciones de fase o cancelaciones en frecuencias concretas, pero la conclusión principal es la reducción espectral global hacia el final.

---
### 1️⃣3️⃣ Mapa espectral (ventanas de 0.5 s, 50 % de solape)
<img width="1124" height="413" alt="image" src="https://github.com/user-attachments/assets/91ca29e5-c3e6-40bb-8df6-14a7218a3f3e" />

El mapa espectral de alta resolución muestra la **variación temporal de la potencia en función de la frecuencia** para las 366 ventanas cortas generadas.  
Los tonos cálidos (rojo y amarillo) indican regiones de mayor potencia, concentradas principalmente entre **40 y 120 Hz**, mientras que los tonos fríos (verde y azul) representan zonas de menor actividad eléctrica.  
La densidad espectral se mantiene homogénea durante todo el periodo analizado, sin desplazamiento notable hacia frecuencias bajas, lo que evidencia que **no hubo signos de fatiga muscular** ni pérdida de fuerza en la contracción.  
Este mapa confirma la estabilidad espectral del músculo y demuestra la correcta aplicación del análisis de densidad espectral por ventanas cortas.

---

## 🧾 Conclusión general
El procesamiento, segmentación y análisis espectral realizados muestran una señal electromiográfica limpia, estable y sin evidencia de fatiga muscular.  
El uso combinado de filtros, ventaneo, transformada de Fourier, frecuencia mediana y análisis estadístico permitió caracterizar con precisión la actividad muscular y validar la metodología propuesta.

---

### 📁 Archivos generados por el script
- `01_emg_cruda_vs_filtrada.png`
- `02_fft_global.png`
- `03_fmed_vs_tiempo.png`
- `04_mapa_espectral.png`
- `05_fmed_tendencia_fatiga.png`
- `06_ventanas_0p5s_senal.png`
- `07_metricas_0p5s.png`
- `08_espectro_promedio_0p5s.png`
- `09_significancia_bandas.png`
- `10_mapa_espectral_0p5s.png`

---
