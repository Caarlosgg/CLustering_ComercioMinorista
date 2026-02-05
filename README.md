# 🎯 Customer Intelligence & Segmentation: RFM + K-Means Clustering

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-Data%20Viz-teal?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

> **Transformando datos transaccionales en estrategias de retención.**
> Este proyecto implementa un modelo de Machine Learning no supervisado para segmentar clientes de un e-commerce mayorista, permitiendo identificar automáticamente a los clientes VIP, detectar riesgos de abandono y optimizar campañas de marketing.

---

## 📋 Tabla de Contenidos

1. [Contexto del Negocio](#-contexto-del-negocio)
2. [Sobre el Dataset](#-sobre-el-dataset)
3. [Metodología y Flujo de Trabajo](#-metodología-y-flujo-de-trabajo)
4. [Stack Tecnológico](#-stack-tecnológico)
5. [Resultados y Segmentación](#-resultados-y-segmentación)
6. [Visualización Avanzada](#-visualización-avanzada)
7. [Cómo Ejecutar](#-cómo-ejecutar)
8. [Conclusiones](#-conclusiones)

---

## 💼 Contexto del Negocio

En el sector retail, el costo de adquirir un nuevo cliente es **5 veces mayor** que retener a uno existente. Sin embargo, tratar a todos los clientes por igual es ineficiente.

El objetivo de este proyecto es aplicar la técnica **RFM (Recency, Frequency, Monetary)** potenciada por algoritmos de Clustering para responder preguntas clave:
* ¿Quiénes son nuestros clientes más valiosos (**VIP**)?
* ¿Qué clientes están en riesgo de abandonar la marca (**Churn Risk**)?
* ¿Quiénes tienen potencial para convertirse en grandes compradores?

---

## 💾 Sobre el Dataset

Se utiliza el conjunto de datos público **[Online Retail Dataset](https://www.kaggle.com/datasets/hellbuoy/online-retail-customer-clustering)** del repositorio UCI Machine Learning.

* **Origen:** Transacciones de un e-commerce mayorista con sede en el Reino Unido.
* **Periodo:** 01/12/2010 a 09/12/2011.
* **Volumen Inicial:** 541,909 registros.
* **Variables Clave:** `InvoiceNo`, `StockCode`, `Quantity`, `InvoiceDate`, `UnitPrice`, `CustomerID`, `Country`.

**⚠️ Limpieza de Datos Realizada:**
* Eliminación de ~135,000 registros sin `CustomerID` (esenciales para el perfilado).
* Filtrado de transacciones con `Quantity <= 0` (Devoluciones) y `UnitPrice <= 0` (Errores/Regalos).
* **Dataset Final:** ~397,000 transacciones limpias.

---

## ⚙️ Metodología y Flujo de Trabajo

El proyecto sigue un pipeline riguroso de Data Science:

### 1. Ingeniería de Características (RFM)
Transformamos los datos de nivel "transacción" a nivel "cliente", calculando:
* **Recency (R):** Días transcurridos desde la última compra hasta la fecha de corte.
* **Frequency (F):** Número total de transacciones únicas.
* **Monetary (M):** Suma total del valor de compra (`Quantity * UnitPrice`).

### 2. Preprocesamiento Estadístico
Los datos financieros reales presentan una fuerte asimetría positiva (distribución Power Law). Para optimizar el rendimiento de K-Means:
* **Transformación Logarítmica:** Aplicada a R, F y M para "comprimir" los outliers y suavizar la distribución.
* **Escalado (StandardScaler):** Normalización de datos (Media=0, Desviación=1) para que la variable "Monetary" (miles de $) no domine a "Frequency" (unidades) en el cálculo de distancias euclidianas.

### 3. Modelado (K-Means Clustering)
* **Determinación de K:** Se utilizaron el **Método del Codo (Elbow Method)** y el **Coeficiente de Silueta** para determinar que **K=3** es el número óptimo de segmentos.
* **Asignación Dinámica de Nombres:** Se implementó un algoritmo lógico post-entrenamiento que asigna etiquetas de negocio (*VIP, Riesgo, Potenciales*) basándose en los centroides, evitando la asignación aleatoria de números (Cluster 0, 1, 2).

---

## 🛠 Stack Tecnológico

* **Python 3.8+**
* **Pandas & NumPy:** Manipulación de datos y álgebra lineal.
* **Scikit-Learn:**
    * `KMeans`: Algoritmo de clustering.
    * `StandardScaler`: Normalización Z-Score.
    * `silhouette_score`: Métrica de evaluación.
* **Visualización:**
    * `Matplotlib` & `Seaborn`: Gráficos estáticos y estadísticos.
    * `mpl_toolkits.mplot3d`: Visualización espacial tridimensional.

---

## 📊 Resultados y Segmentación

El modelo identificó exitosamente 3 perfiles de comportamiento distintos:

| Segmento | Descripción del Perfil | Estrategia Recomendada |
| :--- | :--- | :--- |
| **🥇 VIP / Top** | **Gasto Alto, Frecuencia Alta, Recencia Baja.** Son la columna vertebral del negocio. | Programas de fidelización exclusivos, acceso anticipado, trato personalizado. |
| **🌿 Potenciales** | **Gasto Medio, Frecuencia Media.** Clientes activos con margen de crecimiento. | Estrategias de *Up-selling* y *Cross-selling* para aumentar su ticket medio. |
| **💜 Riesgo / Perdidos** | **Recencia Muy Alta.** Hace mucho tiempo que no compran. | Campañas agresivas de reactivación (*Win-back*) o encuestas de satisfacción. |

---

## 📈 Visualización Avanzada

El notebook genera un set completo de visualizaciones estratégicas:

1.  **Dashboard de Distribución:** Gráficos de Pastel y Barras para entender el tamaño y valor monetario de cada grupo (Principio de Pareto).
2.  **Boxplots con Escala Logarítmica:** Para visualizar la dispersión de la Recencia y el Gasto sin que los valores extremos distorsionen la imagen.
3.  **Mapas de Dispersión (2D):** Visualización de fronteras entre grupos (ej. *Frequency vs Monetary*).
4.  **Snake Plot:** Gráfico técnico estandarizado que muestra la desviación de cada atributo respecto a la media global.
5.  **Mapa de Calor (Heatmap):** Tabla de importancia relativa porcentual.

*(Nota: Ejecuta el notebook para generar las imágenes interactivas y estáticas)*

---

## 🚀 Cómo Ejecutar

1.  Clona el repositorio:
    ```bash
    git clone [https://github.com/tu-usuario/customer-clustering-rfm.git](https://github.com/tu-usuario/customer-clustering-rfm.git)
    ```
2.  Instala las dependencias necesarias:
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn
    ```
3.  Abre el archivo `Clustering.ipynb` en Jupyter Notebook, VS Code o Google Colab.
4.  Asegúrate de tener el archivo `OnlineRetail.csv` en el mismo directorio.
5.  Ejecuta todas las celdas para reproducir el análisis.

---

## 🏁 Conclusiones

Este proyecto demuestra cómo la ciencia de datos puede transformar una base de datos bruta en **decisiones de negocio**. Hemos logrado separar matemáticamente a los clientes leales de los que están en riesgo, proporcionando al equipo de marketing una herramienta precisa para optimizar su presupuesto y mejorar la retención de clientes.
