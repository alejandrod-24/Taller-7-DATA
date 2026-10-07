# Taller 7 

# 🚖 Uber Demand Forecasting & Spatio-Temporal Prediction Project
El documento a continuación es una presentación técnica y la documentación de arquitectura orientada a la ingeniería de **Predicción de Demanda en Tiempo Real de Uber**. El objetivo centra de este proyecto de Uber es mitigar el desbalance espacio-temporal entre la oferta de socios conductores y la demanda de usuarios mediante el despliegue de modelos de Inteligencia Artificial y arquitecturas escalables de Machine Learning.

---

## 📌 1. Naturaleza y Objetivos del Proyecto
El  sistema opera bajo la premisa: **predecir con alta fidelidad cuántos viajes se solicitarán en un área geográfica específica (hexágonos) y en un intervalo de tiempo futuro** [1.1.2].

### Objetivos Clave:
* **Optimización de Flotas y Despacho:** Direccionar de manera proactiva a los conductores hacia zonas de alta demanda antes de que ocurra el pico de solicitudes [1.1.2, 1.3.2].
*  **Mitigación de Tarifas Dinámicas (Surge Pricing):** Al equilibrar la oferta y la demanda de forma predictiva, se reduce el impacto extremo de tarifas dinámicas para el usuario [1.3.5].
*  **Planificación de Operaciuones a Largo Plazo:** Predecir tendencias semanales y estacionales para gestionar eventos masivos, condiciones climáticas adversas o festividades [1.1.5, 1.3.5].

---

## 🏗️ 2. Arquitectura de Datos y Componentes del Ecosistema
La infraestructura real implementada por Uber se compone de plataformas propietarias y de código abierto diseñadas para operar a escala de petabytes:

### A. Michelangelo: El Motor de MLOps
[Michelangelo](https://www.uber.com/us/en/blog/michelangelo-machine-learning-platform/) es la plataforma interna de Machine Learning de Uber encargada del ciclo de vida de los modelos.
* **Feature Store:** Centraliza las variables agregadas (Por ejemplo, tasas de solicitud por minuto, datos históricos de trafico) para su uso en entrenamiento *offline* e inferencia *online*
* **Inferencia de Baja Latencia:** Permite que los modelos de predicción de demanda se ejecuten en tiempo real respondiendo a flujos continuos de datos (Streaming con Apache Spark y Kafka).

### B. Segmentación Espacial: H3 (Hexagonal Hierarchical Spatial Index)
Uber no divide las ciudades por barrios tradicionales, sino mediante un sistema de indexación espacial hexagonal de código abierto llamado **H3**. Cada celda hexagonal funciona como un punto de datos independiente donde se calcula la densidad de la demanda de manera estandarizada y fluida.

### C. Paquete Orbit: Pronósticos Bayesianos Robustos
Para series de tiempo complejas e inferencia estadística, Uber desarrolló e integró [Orbit](https://github.com/uber/orbit) [1.4.1, 1.4.2].
* Implementa modelos como **DLT (Damped Local Trend)** y **LGT (Local Global Trend)** [1.4.1].
* Permite estimaciones probabilísticas estructuradas que incorporan variables externas como marketing, clima o estacionalidades agresivas [1.4.1, 1.4.2].

---

## 🔬 3. Enfoque Algorítmico y Modelado
El proyecto aborda el problema desde múltiples frentes metodológicos: 
1. **Modelos Secuenciales Profundos (Deep Learning):** Redes Neuronales Recurrentes (**LSTM** y **GRU**) diseñadas para caputar dependencias temporales complejas y patrones cíclicos diarios/semanales [1.1.5, 1.3.8].
2.  **Modelos Espacio-Temporales (ST-GCN): ** Redes Convolucionales de Grafos combinadas con capas temporales que entienden que el hexágono $A$ afecta al hexágono vecino $B$.
3.  **Modelos Ensamble Estructurados:** Implementaciones eficientes de Gradient Boosting (**XGBoost**, **LightGBM**) entrenadas con características de ingeniería de variables de corto plazo [1.1.5, 1.3.7].

---

## 🛠️ 4. Estructura Estándar del Repositorio (Plantilla)

```text
├── LICENSE
├── README.md               <- Presentación del proyecto.
├── data/
│   ├── raw/                <- Conjuntos de datos originales (e.g., Uber NYC Pickups) [1.1.1, 1.1.5].
│   └── processed/          <- Datos procesados agrupados por hexágonos H3 y ventanas de tiempo [1.1.1].
├── notebooks/              <- Análisis exploratorio de datos (EDA) y entrenamiento experimental [1.3.4, 1.3.7].
├── src/                    <- Código fuente modular.
│   ├── data_pipeline.py    <- Consumo de flujos en tiempo real y agregación espacial.
│   ├── features.py         <- Ventanas de tiempo (lags), datos climáticos y festivos.
│   └── models/             <- Scripts de entrenamiento e inferencia (Orbit/Deep Learning).
└── config.yaml             <- Parámetros de hiperconfiguración del modelo.
```

---

## 📊 5. Fuentes y Referencias Técnicas Oficiales

* **[1] Blog de Ingeniería de Uber:** *[Michelangelo: Uber’s Machine Learning Platform](https://www.uber.com/us/en/blog/michelangelo-machine-learning-platform/)*. Detalles exhaustivos sobre el feature store y despliegue distribuido de predicciones.
* **[2] Repositorio de Código Abierto Orbit (Uber):** *[Uber Orbit GitHub Package](https://github.com/uber/orbit)*. Marco probabilístico de series de tiempo para estimación de demanda [1.4.1].
* **[3] Documentación de Indexación Espacial H3:** *[H3: Uber’s Hexagonal Hierarchical Spatial Index](https://h3geo.org/)*. Documentación oficial del sistema de celdas geográficas.
* **[4] Proyectos Comunitarios de Referencia (Dataset de Código Abierto):** Repositorios públicos de análisis basados en los datos abiertos de viajes de Uber en la ciudad de Nueva York, utilizados globalmente para validar algoritmos predictivos (*[Himanshu-1703/uber-demand-prediction](https://github.com/Himanshu-1703/uber-demand-prediction)* [1.1.1] y *[MohammadRehaanAli/Uber-Trip-Analysis](https://github.com/MohammadRehaanAli/Uber-Trip-Analysis-Using-Machine-learning)* [1.1.5]).
