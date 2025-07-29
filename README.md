
# 📊 Estimación de la maduración puberal y modelado jerárquico bayesiano del crecimiento infantil en México utilizando datos de ENSANUT y curvas de referencia del CDC

Este proyecto tiene como objetivo construir un modelo estadístico para evaluar los patrones de crecimiento infantil en México utilizando datos de la **ENSANUT**. El modelo busca estimar si los niños están alcanzando los estándares de crecimiento esperados según su edad y sexo, incorporando características tanto a nivel individual como del hogar.

---

## 📁 Estructura del Proyecto

```text
data/
├── raw/           # Datasets originales de ENSANUT (CSV, XLSX)
├── interim/       # Datos limpios pero aún no integrados
├── processed/     # Dataset final a nivel individuo

src/
├── preprocessing/ # Scripts de limpieza y transformación
├── features/      # Creación de variables y funciones auxiliares
├── modeling/      # Definición del modelo bayesiano
└── evaluation/    # Evaluación y diagnóstico del modelo

notebooks/
├── 01_exploration.ipynb     # Exploración inicial de datos
├── 02_preprocessing.ipynb   # Limpieza y unión de tablas
└── 03_modeling.ipynb        # Inferencia y análisis del modelo

outputs/
├── figures/       # Visualizaciones del análisis
└── results/       # Predicciones y tablas resumen

requirements.txt
README.md
