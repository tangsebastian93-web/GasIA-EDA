# GasIA — Análisis Exploratorio de Datos (EDA)
**Universidad Industrial de Santander — Sede Barrancabermeja**  
**Escuela de Ingeniería de Sistemas e Informática | Semillero de IA Aplicada**

### Integrantes del Equipo
* Sebastian David Tang Gutierrez (2264049)
* Danna Valentina Perez Alvarado (2254262)
* Edison Bayardo Puerto Maldonado (2254239)
* Pathy Alexandra Rache Guaman (2254260)
* Arianalucia Cogollos Fernandez (2254202)

---

## 1. Descripción y Pregunta de Investigación
Este repositorio documenta el análisis exploratorio de datos (EDA) del proyecto **GasIA** (*Alerta Inteligente de Fugas de Gas Doméstico*), cuyo objetivo es detectar fugas tempranas de GLP y reducir falsas alarmas en cocinas sometidas a altas temperaturas en Barrancabermeja.

* **Pregunta de investigación:** ¿Cómo se comportan las lecturas de concentración de gases frente a las variaciones de temperatura y humedad ambiental, y qué diferencias estadísticas permiten distinguir una condición segura de una alerta temprana?
* **Fuente de datos utilizada:** *Smoke Detection Dataset* (`smoke_detection_iot.csv`), obtenido de Kaggle (datos reales capturados por arreglos de sensores IoT de gas, temperatura y humedad a 1 Hz). Se utiliza como línea base preliminar mientras se completa la adquisición primaria con nuestro prototipo ESP32 + MQ-2/MQ-6 + DHT22.
* **Unidad de observación:** Cada fila corresponde a una medición temporal instantánea del conjunto de sensores.

## 2. Procedimiento y Limpieza de Datos
El análisis se desarrolló en **Python** utilizando **Pandas, NumPy, Matplotlib y Seaborn** dentro de un cuaderno Jupyter (`EDA_GasIA.ipynb`):
1. **Inspección inicial:** Se cargaron `[62,630]` observaciones iniciales verificando tipos de datos (`.info()`), resumen estadístico (`.describe()`) y descartando la columna índice residual (`Unnamed: 0`).
2. **Selección y limpieza:** Se filtraron las 6 variables físicas homologables a nuestros sensores (`temperatura_c`, `humedad_pct`, `tvoc_ppb`, `raw_h2`, `raw_etanol` y `estado_alerta`), eliminando filas duplicadas y verificando la ausencia de valores nulos o ceros físicamente imposibles.
3. **Análisis descriptivo y visual:** Se calcularon medidas de tendencia central (media, mediana) y dispersión (desviación estándar) agrupadas por estado mediante `.groupby()`, respaldadas por 4 gráficos rotulados (histograma, diagrama de cajas, dispersión y mapa de calor de correlación).

## 3. Resultados Principales
* **Comportamiento en estado seguro vs. alerta:** En condición segura (`0`), la lectura cruda de hidrógeno (`Raw H2`) presenta una mediana de `[valor de la tabla]` (DE = `[valor]`), mientras que en estado de alerta (`1`) la mediana desciende/cambia a `[valor de la tabla]` (DE = `[valor]`).
* **Presencia de valores atípicos:** Se identificaron picos transitorios de concentración (`TVOC`) en condiciones normales que se solapan con eventos de alerta, demostrando el riesgo de falsas alarmas al usar umbrales estáticos.
* **Influencia ambiental:** La temperatura registró un rango entre `[min]` °C y `[max]` °C, mostrando correlaciones lineales moderadas con las lecturas de los sensores de gas (`r = [valor]`).

## 4. Conclusiones
El análisis confirma la hipótesis técnica de **GasIA**: las variables ambientales (temperatura y humedad) alteran la línea base de los sensores de gas y generan solapamientos entre picos normales de operación y eventos anómalos. Por tanto, un sistema basado únicamente en un umbral fijo de concentración (ppm) es insuficiente; se requiere un modelo de Machine Learning multivariable y temporal para clasificar con precisión el riesgo real.

## 5. Limitaciones del Análisis
* **Diferencia de gas objetivo:** El dataset público de referencia evalúa humo y compuestos volátiles generales en interiores, mientras que la aplicación final de **GasIA** requiere calibración específica para Gas Licuado de Petróleo (GLP: propano/butano).
* **Condiciones térmicas locales:** Las temperaturas registradas en la muestra pública no alcanzan de forma sostenida los picos térmicos extremos típicos de las cocinas domésticas en Barrancabermeja (>35 °C).
* **Correlación vs. causalidad:** Las correlaciones observadas entre temperatura y respuesta del sensor indican asociación estadística en esta muestra, pero no sustituyen la validación experimental controlada con el hardware propio del proyecto.