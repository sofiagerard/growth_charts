# Estimación de la maduración puberal y modelado jerárquico bayesiano del crecimiento infantil en México

**Autora:** Sofía Gerard Riba  
**Asesor:** Dr. Edgar Francisco Román Rangel  
**Programa:** Maestría en Ciencia de Datos (ITAM)  
**Periodo:** Verano-Otoño 2025  

---

## Descripción general

Este proyecto desarrolla un modelo jerárquico bayesiano para estimar las curvas de crecimiento infantil en México utilizando datos de la Encuesta Nacional de Salud y Nutrición (ENSANUT 2012).  

El objetivo principal es generar curvas de referencia de talla por edad y sexo ajustadas al contexto mexicano y compararlas con los estándares internacionales del CDC 2000.

A mediano plazo, se planea incorporar un modelo de variable latente probabilística (LVPM, *Latent Variable Probability Model*) para estimar el estadio puberal de Tanner con base en covariables biológicas, antropométricas y socioeconómicas.

---

## Objetivos del proyecto

1. Actualizar las curvas de referencia de crecimiento infantil para la población mexicana.  
2. Evaluar el ajuste de un modelo jerárquico bayesiano frente al método LMS del CDC.  
3. Generar funciones reproducibles de cálculo de percentiles (p3–p97) y z-scores individuales.  
4. Construir un producto de datos con resultados replicables y visualizaciones comparativas.  
5. Sentar las bases para una extensión del modelo hacia la estimación del estadio puberal (Tanner).

---

## Estructura del repositorio

```text
data/
├── raw_salud/         # Datos originales del módulo de salud ENSANUT
├── raw_nutricion/     # Datos originales del módulo de nutrición ENSANUT
├── clean/             # Datos limpios, depurados y listos para integración
└── processed/         # Dataset final a nivel individuo

estancia_investigacion/
├── 1_preprocessing_salud.ipynb
├── 2_preprocessing_nutricion.ipynb
├── 3_processing_salud_and_nutricion.ipynb
├── 4_eda.ipynb
├── 5_intro_bayesian_models.ipynb
├── 6_1st_bayesian_model.ipynb
├── 7_ENSANUT_modelos_jerarquicos.ipynb
├── 8_ENSANUT_final_model.ipynb
├── lms_m5a.csv
├── percentiles_cdc_like_m5a_hombre.csv
└── percentiles_cdc_like_m5a_mujer.csv

notebooks/
├── 1_preprocessing_salud.ipynb
├── 2_preprocessing_nutricion.ipynb
├── 3_processing_salud_and_nutricion.ipynb
├── 4_eda.ipynb
├── 5_intro_bayesian_models.ipynb
├── 6_1st_bayesian_model.ipynb
├── 7_2nd_bayesian_model.ipynb
├── 8_estimacion_tanner.ipynb
├── 8.1_estimacion_tanner.ipynb
└── 8.2_estimacion_tanner.ipynb

docs/
└── Instalacion_PyMC_mac.md   # Guía paso a paso para instalación de PyMC en macOS (M1/M2/M3)

environment.yml
README.md
````

---

## Instalación rápida (entorno)

**Opción A — Recomendado (Conda/Mamba):**

```bash
conda env create -f environment.yml
conda activate growth_charts_env
python -m ipykernel install --user --name=growth_charts_env
```

**Opción B — Alternativa (pip/venv):**

```bash
python -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
python -m ipykernel install --user --name=growth_charts_env
```

> Guía detallada para macOS (M1/M2/M3): ver `docs/Instalacion_PyMC_mac.md`.

---

## Metodología

El proyecto sigue una estrategia de modelado incremental, partiendo de una estructura lineal básica hasta llegar a un modelo jerárquico flexible con splines y efectos aleatorios por municipio.

| Modelo | Descripción resumida                                                    |
| ------ | ----------------------------------------------------------------------- |
| M0     | Media lineal en edad y sexo con intercepto aleatorio por municipio      |
| M1     | M0 + interacción edad × sexo                                            |
| M2     | M1 + pendiente aleatoria de edad por municipio                          |
| M3     | M2 + verosimilitud Student–t para robustez a valores extremos           |
| M4     | M3 + término cuadrático de edad                                         |
| M5a    | Modelo final: splines cúbicos, jerarquía completa y priors informativos |

**Distribución verosímil:** Student–t
**Inferencia:** NUTS (4 cadenas, 2000 warmup + 2000 muestras)
**Evaluación:** PSIS–LOO y WAIC, complementado con *posterior predictive checks* (PPC)

---

## Resultados principales

* Se entregan todos los notebooks necesarios para **preprocesar, limpiar, integrar, modelar, comparar y generar las predicciones finales** de las curvas de crecimiento infantil en México.
* El flujo de trabajo abarca desde la depuración de los módulos ENSANUT de salud y nutrición, hasta la construcción y evaluación del modelo jerárquico bayesiano final.
* El modelo M5a genera curvas suavizadas y clínicamente coherentes con los patrones de crecimiento observados, mostrando mejor ajuste y cobertura que el método LMS del CDC.
* Los resultados incluyen las tablas finales de percentiles p3–p97 por sexo y la documentación completa de la inferencia, diagnóstico y comparación de modelos.

Archivos y notebooks principales:

* `estancia_investigacion/1_preprocessing_salud.ipynb` — Limpieza y estandarización del módulo de salud.
* `estancia_investigacion/2_preprocessing_nutricion.ipynb` — Limpieza del módulo de nutrición.
* `estancia_investigacion/3_processing_salud_and_nutricion.ipynb` — Integración de ambos módulos a nivel individuo.
* `estancia_investigacion/4_eda.ipynb` — Análisis exploratorio y control de calidad de variables.
* `estancia_investigacion/5_intro_bayesian_models.ipynb` — Introducción y justificación del enfoque jerárquico bayesiano.
* `estancia_investigacion/6_1st_bayesian_model.ipynb` — Primer modelo jerárquico y evaluación inicial.
* `estancia_investigacion/7_ENSANUT_modelos_jerarquicos.ipynb` — Comparativa incremental de modelos M0–M5a.
* `estancia_investigacion/8_ENSANUT_final_model.ipynb` — Modelo final M5a, generación de predicciones y exportación de resultados.

Tablas de salida finales (percentiles nacionales):

* `estancia_investigacion/lms_m5a.csv` — Parámetros suavizados Lambda–Mu–Sigma del modelo M5a.
* `estancia_investigacion/percentiles_cdc_like_m5a_hombre.csv` — Percentiles p3–p97 para población masculina.
* `estancia_investigacion/percentiles_cdc_like_m5a_mujer.csv` — Percentiles p3–p97 para población femenina.

---

## Reproducibilidad

El proyecto está diseñado bajo principios de ciencia abierta y replicabilidad:

* Scripts modulares y comentados en los notebooks.
* Notebooks autocontenidos que permiten rastrear cada etapa del flujo analítico.
* Control de versiones y trazabilidad de datos ENSANUT (niveles raw → processed).
* Semillas aleatorias fijadas (`random_state=42`) para garantizar reproducibilidad.

Para ejecutar el análisis:

**Con Conda/Mamba (recomendado):**

```bash
conda activate growth_charts_env
```

**Con pip/venv (alternativa):**

```bash
source .venv/bin/activate
```

Luego, ejecuta los notebooks en el orden numérico dentro de `estancia_investigacion/` o `notebooks/`.

---

## Instalación de entorno PyMC en macOS (M1, M2 o M3)

Para usuarios de macOS 15 o superior con chips Apple Silicon (M1, M2 o M3), el repositorio incluye una guía detallada en:

```
docs/Instalacion_PyMC_mac.md
```

Este documento describe paso a paso cómo instalar PyMC, Aesara y TensorFlow Probability en entornos Conda o Miniforge optimizados para ARM.
Incluye configuraciones recomendadas para Visual Studio Code y la resolución de errores comunes de compatibilidad en macOS.

---

## Referencias bibliográficas

* Cole, T. J., & Green, P. J. (1992). *Smoothing reference centile curves: The LMS method*. Statistics in Medicine.
* Gelman, A., Carlin, J. B., Stern, H. S., Dunson, D. B., Vehtari, A., & Rubin, D. B. (2013). *Bayesian Data Analysis* (3rd ed.). CRC Press.
* McElreath, R. (2020). *Statistical Rethinking: A Bayesian Course with Examples in R and Stan*.
* Rigby, R. A., & Stasinopoulos, D. M. (2019). *Generalized Additive Models for Location, Scale and Shape (GAMLSS)*.
* Vehtari, A., Gelman, A., & Gabry, J. (2017). *Practical Bayesian model evaluation using LOO and WAIC*. Statistics and Computing.

---

## Cumplimiento con los lineamientos del ITAM

De acuerdo con los lineamientos de la Estancia de Investigación y de Titulación de la Maestría en Ciencia de Datos del ITAM:

* Se entrega el **reporte técnico en formato PDF** y este **repositorio documentado** como parte de los entregables oficiales.
* El proyecto cumple con los requisitos de **contexto, metodología, resultados, producto de datos y conclusiones** establecidos en el descriptivo oficial.
* Se incluye **cronograma de trabajo equivalente a 240 horas** en el anexo del reporte.
* Se garantiza trazabilidad, replicabilidad y documentación completa conforme a las normas institucionales.

---

## Próximas etapas

1. Extensión del modelo hacia la estimación del estadio puberal mediante LVPM (Latent Variable Probability Model).
2. Inclusión de covariables ambientales y socioeconómicas para ajustar heterogeneidad regional.
3. Desarrollo de un panel interactivo (Streamlit o Dash) para visualización de curvas y percentiles.
4. Publicación del dataset procesado y funciones z/p en un repositorio institucional.

---

## Contacto

**Autora:** Sofía Gerard Riba
Correo: [sgerardr@itam.mx](mailto:sgerardr@itam.mx)

**Asesor:** Dr. Edgar Francisco Román Rangel
Departamento Académico de Ciencia de Datos, ITAM
Correo: [edgar.roman@itam.mx](mailto:edgar.roman@itam.mx)

**Coordinación de la Maestría en Ciencia de Datos:**
Angélica Torres — [atorres@itam.mx](mailto:atorres@itam.mx)

**Dirección del Programa:**
Dr. Felipe Medina — [felipe.medina@itam.mx](mailto:felipe.medina@itam.mx)


