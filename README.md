# lab-4
# 🧠 Análisis de Señales Electromiográficas (EMG)

Este repositorio contiene el desarrollo y los resultados del análisis electromiográfico realizado a partir de un archivo CSV de señal EMG.  
El código implementa **filtrado digital**, **ventaneo**, **análisis espectral**, **detección de fatiga** y **evaluación estadística** entre ventanas según los criterios del laboratorio.  

---

## 📘 Descripción general del proceso

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
![Imagen de WhatsApp 2025-10-22 a las 19 22 26_d9533141](https://github.com/user-attachments/assets/5e9ba250-8c78-41b1-b3d6-be60b2337375)

### 1️⃣ EMG cruda vs filtrada
En esta gráfica se compara la señal **EMG cruda** (azul) con la **EMG filtrada** (naranja) tras aplicar filtros pasa-altas y pasa-bajas de 20 a 450 Hz.  
El filtrado elimina el componente DC y el ruido de baja frecuencia proveniente de movimiento o interferencia eléctrica, permitiendo conservar únicamente la actividad eléctrica muscular relevante.  
Se observa una reducción notable en la amplitud y mayor limpieza en la señal filtrada, evidenciando que los filtros fueron aplicados correctamente y que la energía útil de la contracción muscular se mantiene dentro de la banda esperada.

---

### 2️⃣ FFT global de la señal EMG filtrada
![Imagen de WhatsApp 2025-10-22 a las 19 22 39_fc9ad6de](https://github.com/user-attachments/assets/47bd3d98-ec6b-432c-ba0f-5fff9c719305)

Esta figura muestra el **análisis en frecuencia de toda la señal EMG** mediante la Transformada Rápida de Fourier (FFT).  
Se observa un pico principal alrededor de los **60 Hz**, indicando que gran parte de la energía se concentra en ese rango, lo cual es característico de señales musculares activas.  
Además, el espectro decae progresivamente hacia frecuencias altas, lo que confirma que el contenido útil se encuentra principalmente entre 20 y 150 Hz.

---

### 3️⃣ Frecuencia mediana (*f_med*) por ventana de 30 s
![Imagen de WhatsApp 2025-10-22 a las 19 22 56_c976c637](https://github.com/user-attachments/assets/80972cfb-318a-4328-80b2-96ecc580a821)

Aquí se representa la evolución de la **frecuencia mediana** (*f_med*) a lo largo del tiempo, calculada en ventanas de 30 s sin solape.  
Los valores se mantienen alrededor de **60 Hz**, con variaciones mínimas entre las cuatro ventanas analizadas.  
La estabilidad de *f_med* sugiere una contracción muscular mantenida y sin signos de fatiga durante el registro.

---

### 4️⃣ Consola de resultados del ventaneo largo
![Imagen de WhatsApp 2025-10-22 a las 19 23 05_27bfec1a](https://github.com/user-attachments/assets/d5edccef-9e70-4a2b-a89c-2f1619306fd9)

En este bloque se observan los datos generados por el programa para cada ventana de 30 s, indicando las posiciones de las muestras, el tiempo correspondiente y la *f_med* calculada.  
Se registraron **cuatro ventanas**, todas con frecuencias medianas próximas a 60 Hz, lo cual respalda la consistencia espectral del músculo a lo largo del ensayo.

---

### 5️⃣ Mapa espectral por ventanas (30 s, Hamming)
![Imagen de WhatsApp 2025-10-22 a las 19 23 23_4781633d](https://github.com/user-attachments/assets/513811cc-b859-43c0-8a51-7a1efb5fe5f1)

El mapa espectral representa la **distribución de potencia en función del tiempo y la frecuencia** para las ventanas largas.  
Los colores cálidos (rojo y amarillo) muestran las zonas de mayor intensidad de señal, ubicadas entre 40 y 120 Hz, mientras que los tonos fríos (azul) representan menor potencia.  
Se observa una densidad espectral estable a lo largo del tiempo, sin desplazamientos hacia frecuencias bajas, lo cual indica que la actividad muscular se mantuvo constante y **no hay evidencia de fatiga** durante el periodo analizado.

---

### 6️⃣ Resultados numéricos de detección de fatiga
![Imagen de WhatsApp 2025-10-22 a las 19 23 46_2bb50eae](https://github.com/user-attachments/assets/c7842ca3-1612-40d4-b0ed-50bd193a1331)

Este recuadro presenta los resultados del **análisis estadístico de fatiga**.  
La pendiente de la *f_med* (-0.0017 Hz/s) es prácticamente nula y no significativa (p = 0.875), con una caída porcentual del 0.33 %, muy por debajo del umbral del 5 %.  
Esto significa que no se detecta una reducción relevante de la frecuencia mediana, por lo que el músculo **no presentó signos de fatiga electromiográfica** a lo largo del registro.

---

### 7️⃣ Gráfica de *f_med* y decisión de fatiga
![Imagen de WhatsApp 2025-10-22 a las 19 24 04_2ca1d8c5](https://github.com/user-attachments/assets/6d68c7d8-e783-459e-a307-c634210fb9ad)

En esta figura se visualiza la evolución de la *f_med* junto con la línea de tendencia (naranja discontinua) y las zonas de comparación entre el bloque inicial y final.  
La tendencia casi horizontal y los valores similares al inicio y al final reafirman que **la potencia espectral del músculo se mantuvo estable**, sin desplazamiento hacia frecuencias más bajas.  
Por lo tanto, el sistema detecta correctamente que **no hubo fatiga muscular** durante el ensayo.

---

### 8️⃣ Señal segmentada en 366 ventanas de 0.5 s
![Imagen de WhatsApp 2025-10-22 a las 19 24 20_c2079646](https://github.com/user-attachments/assets/17990348-8aa0-4f82-a964-3093e3e390af)

Aquí se muestra la **señal completa segmentada en 366 ventanas cortas de 0.5 s con un 50 % de solape**.  
Cada color representa una ventana distinta utilizada para el análisis estadístico y espectral de corto plazo.  
Este ventaneo mejora la resolución temporal del análisis, permitiendo observar variaciones locales en la contracción muscular y preparar los datos para calcular parámetros como la media, desviación estándar y SNR en cada segmento.

---

### 9️⃣ Parámetros calculados por ventana corta
![Imagen de WhatsApp 2025-10-22 a las 19 24 41_f60d5ad7](https://github.com/user-attachments/assets/fc10110b-b2f6-46b2-8fbc-cc354fbe59ea)

Esta figura muestra la evolución de varios **parámetros estadísticos** (media, desviación estándar, coeficiente de variación, SNR y FFR) calculados en cada una de las 366 ventanas.  
Los valores se mantienen estables, sin picos anómalos que indiquen variaciones abruptas en la señal.  
El FFR (frecuencia media ponderada) permanece dentro de rangos medios, reforzando la ausencia de cambios espectrales que sugieran fatiga.  
Este análisis confirma que **la señal EMG es estable y fisiológicamente consistente** durante toda la medición.

---![Imagen de WhatsApp 2025-10-22 a las 19 24 57_529837a0](https://github.com/user-attachments/assets/e38af485-084a-4dcd-937f-7c42372baccf)


### 🔟 Espectro promedio (ventanas de 0.5 s, 50 % de solape)
![Imagen de WhatsApp 2025-10-22 a las 19 25 15_50c85ee7](https://github.com/user-attachments/assets/68cf1390-7c74-45d8-9e27-2ad0d6464c95)

Esta figura representa el **espectro promedio** obtenido a partir de todas las ventanas cortas de 0.5 s con 50 % de solape.  
La curva muestra cómo se distribuye la potencia de la señal EMG en el dominio de la frecuencia, expresada en decibelios (dB).  
Se observan picos notables entre **50 Hz y 120 Hz**, correspondientes a la actividad muscular voluntaria, y una disminución progresiva hacia frecuencias más altas, lo cual es característico de un músculo activo pero sin fatiga.  
La ausencia de desplazamiento del espectro hacia frecuencias bajas indica que la energía de la señal se mantiene estable y que el músculo conserva su capacidad de contracción sin deterioro en la respuesta eléctrica.

---

### 1️⃣1️⃣ Mapa espectral (ventanas de 0.5 s, 50 % de solape)
![Imagen de WhatsApp 2025-10-22 a las 19 45 23_6c335f8a](https://github.com/user-attachments/assets/436f2faa-2327-49e1-bd40-7bee27d80661)

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
