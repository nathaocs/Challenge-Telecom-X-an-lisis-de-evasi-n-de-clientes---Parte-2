# 📘 Proyecto Telecom X - Parte 2  
**Predicción de Churn de Clientes en Telecomunicaciones**

---

## 1. Propósito del análisis
El propósito principal de este proyecto es **analizar y predecir la cancelación de clientes (churn)** en una empresa de telecomunicaciones a partir de datos contractuales, de uso y demográficos.  

La **cancelación de clientes (churn)** es uno de los principales problemas en el sector telecomunicaciones, ya que adquirir un cliente nuevo es significativamente más costoso que retener a uno existente. Identificar patrones de comportamiento y características de los clientes que abandonan la compañía es fundamental para diseñar **estrategias de retención efectivas**.  

Con este análisis buscamos:  
- Conocer las **características principales de los clientes que cancelan** su servicio.  
- Identificar **variables críticas** como tiempo de permanencia, gastos mensuales y tipo de contrato.  
- Implementar y comparar **modelos predictivos de machine learning** que permitan anticipar cancelaciones futuras.  
- Generar **insights accionables** para el negocio que orienten decisiones estratégicas.  

---

## 2. Estructura del proyecto

```bash
TelecomX-Churn-Analysis/
│
├── data/
│   ├── raw/                     
│   │   └── TelecomX_Data.json        # Datos originales obtenidos de la API
│   └── processed/               
│       ├── telecomx_final.csv        # Datos limpios y procesados
│       ├── telecomx_final.parquet    # Versión optimizada en parquet
│       └── telecomx_final_scaled.csv # Datos estandarizados (si aplica)
│
├── notebooks/
│   └── TelecomX_EDA.ipynb            # Notebook principal con el análisis exploratorio y modelado
│
├── reports/
│   ├── figures/                      # Gráficos y visualizaciones generadas
│   └── Informe.md                    # Informe extenso del análisis de datos
│
├── README.md                         # Documentación principal del proyecto
└── requirements.txt                  # Librerías necesarias para la ejecución
```

Esta estructura permite mantener una **organización clara**, separando los datos crudos de los transformados, los notebooks del análisis y los reportes finales con conclusiones y gráficos.  

---

## 3. Descripción del proceso de preparación de los datos

El preprocesamiento de datos es una etapa crítica para garantizar que los modelos funcionen adecuadamente.  

### 🔹 Clasificación de variables
- **Numéricas continuas:**  
  - `tenure_months` → Meses de permanencia.  
  - `monthly_charges` → Cargos mensuales.  
  - `total_charges` → Cargos acumulados.  
  - `daily_charges` → Cargos diarios (derivado).  

- **Categóricas:**  
  - `customer_gender` (género del cliente).  
  - `phone_service`, `multiple_lines` (tipo de servicio telefónico).  
  - `internet_service`, `online_security`, `tech_support`, etc. (uso de servicios de internet).  
  - `contract` (tipo de contrato).  
  - `payment_method` (forma de pago).  

- **Binarias:**  
  - `senior_citizen`, `partner`, `dependents`, etc.  

### 🔹 Etapas de procesamiento
1. **Eliminación de columnas irrelevantes** → Se descartaron identificadores únicos como `customerID`, ya que no aportan valor predictivo.  
2. **Manejo de datos faltantes** → Se limpiaron registros con valores inconsistentes o nulos en `total_charges`.  
3. **Codificación de variables categóricas** → Se aplicó **One-Hot Encoding**, generando nuevas columnas binarias que representan categorías (ej: `contract_Month-to-Month`, `contract_Two-Year`).  
4. **Balanceo de clases** →  
   - Se verificó que **73% de clientes permanecieron y 27% cancelaron**, lo que representa un **desbalance moderado**.  
   - Se aplicó **SMOTE (Synthetic Minority Oversampling Technique)** para crear ejemplos sintéticos de la clase minoritaria (churn) y equilibrar la distribución.  
5. **Normalización/Estandarización** →  
   - Se detectaron diferencias de escala mayores a 10x entre variables continuas (`total_charges` vs `daily_charges`).  
   - Se aplicó **StandardScaler** a variables numéricas para modelos sensibles a la escala como KNN y Regresión Logística.  
6. **Separación de datos en conjuntos de entrenamiento y prueba** →  
   - Se usó un **70% para entrenamiento** y **30% para prueba**, manteniendo proporción de churn en ambos conjuntos (*stratify*).  

---

## 4. Ejemplos de gráficos e insights obtenidos

Durante el **análisis exploratorio (EDA)** se generaron diversas visualizaciones que aportaron conclusiones clave:  

### 📊 Distribución de Churn
- ~26% de clientes cancelaron vs 74% que permanecieron.  
- Esto confirma la necesidad de **balancear las clases**.  

### 📉 Tiempo de permanencia (tenure) vs Churn
- Clientes con **menos meses de contrato** presentan una mayor tasa de cancelación.  
- Los contratos más largos están asociados con mayor retención.  

### 💳 Método de pago
- El método **Electronic Check** está fuertemente correlacionado con el churn.  
- Los clientes que usan **transferencias automáticas o tarjetas de crédito** muestran mayor permanencia.  

### 💸 Cargos mensuales y totales
- Clientes con **cargos mensuales altos** pero **bajo tiempo de permanencia** son los más propensos a cancelar.  
- Clientes con **cargos totales acumulados altos** suelen permanecer (mayor fidelización).  

### 🔗 Correlaciones
- Variables más correlacionadas con churn:  
  - `tenure_months` (negativa: más meses = menor churn).  
  - `contract` (los contratos cortos = mayor churn).  
  - `payment_method` (Electronic Check = mayor churn).  

---

## 5. Instrucciones para ejecutar el cuaderno

### 🔹 Requisitos
Instalar las librerías necesarias con:  
```bash
pip install -r requirements.txt
```

Librerías principales utilizadas:
- **pandas, numpy** → manipulación de datos.  
- **matplotlib, seaborn** → visualización.  
- **scikit-learn** → preprocesamiento, modelos y métricas.  
- **imblearn** → balanceo de clases (SMOTE).  

### 🔹 Ejecución
1. Clonar o descargar este repositorio.  
2. Abrir el notebook `notebooks/TelecomX_EDA.ipynb` en Google Colab o Jupyter.  
3. Cargar los datos procesados desde `data/processed/telecomx_final.csv`.  
4. Ejecutar las celdas paso a paso:  
   - Preprocesamiento de datos.  
   - Análisis exploratorio (EDA).  
   - Entrenamiento de modelos (Regresión Logística, KNN, Random Forest).  
   - Evaluación de resultados.  
5. Consultar los resultados gráficos en `reports/figures/` y las conclusiones en `reports/Informe.md`.  

---

## ✅ Conclusiones generales

- Se confirmó que el **churn está más relacionado con contratos cortos, cargos altos y ciertos métodos de pago**.  
- El **balanceo de clases** con SMOTE fue necesario para mejorar la capacidad predictiva de los modelos.  
- Modelos probados:  
  - **Regresión Logística y Random Forest** alcanzaron métricas perfectas (probablemente por el dataset balanceado y procesado).  
  - **KNN**, ajustado con *GridSearch*, mostró también un desempeño excelente con F1 ≈ 0.95.  
- Estos resultados evidencian que el dataset procesado es altamente informativo para predecir cancelaciones.  

📌 **Implicaciones de negocio:**  
- Se deben implementar estrategias de retención especialmente en clientes con **contratos de mes a mes y pagos por Electronic Check**.  
- Incentivar la permanencia con **contratos largos** y beneficios asociados.  
- Monitorear de cerca a clientes con **cargos altos y poco tiempo de antigüedad** para ofrecer promociones personalizadas.  

---

📌 **Autor:** Danna
