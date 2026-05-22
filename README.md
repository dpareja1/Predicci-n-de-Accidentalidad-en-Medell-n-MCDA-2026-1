# 🚦 Predicción de Accidentalidad en Medellín

> Modelo de clasificación binaria para predecir la ocurrencia de accidentes de tránsito por barrio y hora en Medellín. Proyecto del Taller MCDA 2026-1.
---

## 📋 Descripción del Proyecto

Este proyecto aborda la predicción de accidentalidad vial en Medellín a nivel de granularidad **barrio × hora**, usando un enfoque de clasificación binaria con clases fuertemente desbalanceadas (~1.5% de positivos sobre ~7.6M registros).

El modelo final (LightGBM) alcanza un **ROC-AUC de 0.9223** en el conjunto de test (2019), y permite cubrir el **61.8% de los accidentes reales** monitoreando solo el 10% de las celdas disponibles mediante un sistema de alertas Top-N diario.

**Integrantes:** Daniel Pareja · Sebastián Ruiz · David Gómez · Andrés Velasco · Juan Morales  
**Profesor:** Pablo Saldarriaga · Mayo 2026

---

## ✨ Características Principales

### Notebook (`Taller_Accidentalidad_VF.ipynb`)
- **EDA completo** — análisis temporal, espacial y climatológico de 125K+ accidentes (2017–2019)
- **Ingeniería de características** — 32 features incluyendo variables cíclicas (seno/coseno), históricas sin data leakage y meteorológicas
- **Comparación de 4 modelos** — Regresión Logística, Random Forest, XGBoost y LightGBM con `TimeSeriesSplit`
- **Estrategias de balanceo** — evaluación de `class_weight`, Undersampling y SMOTE
- **Ajuste de umbral** — selección sobre validación para evitar contaminación del test
- **Sistema de alertas Top-N** — simulación del caso de uso operativo para la Secretaría de Movilidad

### Informe (`docs/Informe_Accidentalidad.docx`)
- Resumen ejecutivo con métricas clave y decisiones de diseño
- Análisis de calidad de datos y estrategia de imputación sin leakage
- Justificación detallada de la selección del modelo final
- Limitaciones, conclusiones y propuestas de trabajo futuro

---

## 📁 Estructura del Repositorio

```
accidentalidad-medellin/
│
├── notebooks/
│   └── Taller_Accidentalidad_VF.ipynb   # Notebook principal del taller
│
├── docs/
│   └── Informe_Accidentalidad.docx      # Reporte técnico completo
│
├── data/
│   └── .gitkeep                         # La BD SQLite no se sube al repo
│                                        # (ver instrucciones abajo)
│
├── .gitignore
├── requirements.txt
└── README.md
```

> **⚠️ Nota sobre los datos:** La base de datos SQLite (~2GB) no se incluye en el repositorio por su tamaño. Solicítala al equipo o descárgala desde el enlace compartido en el canal del curso.

---

## ⚙️ Requisitos Previos

- Python 3.10 o superior
- pip / conda
- Jupyter Notebook o JupyterLab (o Google Colab)

### Librerías principales

| Librería | Versión recomendada | Uso |
|---|---|---|
| `pandas` | ≥ 2.0 | Manipulación de datos |
| `numpy` | ≥ 1.24 | Cálculo numérico |
| `scikit-learn` | ≥ 1.3 | Modelos base y métricas |
| `lightgbm` | ≥ 4.0 | Modelo final |
| `xgboost` | ≥ 2.0 | Modelo comparado |
| `imbalanced-learn` | ≥ 0.11 | SMOTE y undersampling |
| `matplotlib` / `seaborn` | ≥ 3.7 / 0.12 | Visualizaciones |
| `holidays` | ≥ 0.32 | Festivos colombianos |
| `sqlalchemy` | ≥ 2.0 | Conexión a SQLite |

---

## 🚀 Instalación y Ejecución

### 1. Clonar el repositorio

```bash
git clone https://github.com/<tu-usuario>/accidentalidad-medellin.git
cd accidentalidad-medellin
```

### 2. Crear entorno virtual e instalar dependencias

```bash
python -m venv .venv
source .venv/bin/activate          # Linux / macOS
# .venv\Scripts\activate           # Windows

pip install -r requirements.txt
```

### 3. Colocar la base de datos

Copia el archivo `accidentalidad.db` en la carpeta `data/`:

```
data/
└── accidentalidad.db
```

### 4. Abrir el notebook

```bash
jupyter notebook notebooks/Taller_Accidentalidad_VF.ipynb
```

O ejecutar directamente en **Google Colab** subiendo el `.ipynb` y montando el archivo `.db` en Drive.

---

## 📊 Resultados Principales

| Métrica | Valor (Test 2019) |
|---|---|
| ROC-AUC | **0.9223** |
| PR-AUC | 0.1157 |
| Recall | 0.4193 |
| Precision | 0.1150 |
| F1-Score | 0.1805 |

**Sistema Top-N (alertas diarias):**

| Alertas/día | Recall capturado |
|---|---|
| 50 | 7.04% |
| 100 | 13.11% |
| ~4,100 (10% celdas) | **61.8%** |

---

## 📄 Licencia

Este proyecto fue desarrollado con fines académicos en el marco del curso MCDA 2026-1 de la Universidad de Antioquia. Consulta el archivo [LICENSE](LICENSE) para más detalles.
