# Instalación de PyMC en Mac paso a paso (macOS 15+ con M1/M2/M3)

Este archivo documenta cómo se resolvieron los errores de instalación de PyMC en un entorno macOS con chip ARM (Apple Silicon) y Xcode actualizado. 

---

## ✅ Qué hicimos (resumen técnico)

### 1. Reinstalar correctamente las herramientas de línea de comandos de Xcode

* Borramos herramientas rotas:

  ```bash
  sudo rm -rf /Library/Developer/CommandLineTools/
  ```
* Reinstalamos:

  ```bash
  xcode-select --install
  ```
* Verificamos ruta correcta:

  ```bash
  xcode-select -p
  # Resultado esperado: /Library/Developer/CommandLineTools
  ```
* Si no funcionaba, se corrigió con:

  ```bash
  sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
  ```

### 2. Configurar `.pytensorrc` para apuntar al compilador correcto

Se creó el archivo de configuración para PyTensor:

```bash
nano ~/.pytensorrc
```

Contenido:

```
[global]
cxx = /usr/bin/clang++
```

### 3. Crear un entorno limpio de Conda con dependencias compatibles

```bash
conda create -n pymc_env python=3.10 pymc arviz numpy pandas matplotlib seaborn -c conda-forge
```

> **Nota:** `libblas=*=*accelerate` fue omitido porque `zsh` lanza error si no se escapan los asteriscos. En su lugar, se usa el default de numpy para ARM.

Activación:

```bash
conda activate pymc_env
```

### 4. Instalar `ipykernel` para usar notebooks en VSCode

Si aparece el mensaje de "requires ipykernel", instalarlo dentro del entorno:

```bash
conda install ipykernel
```

### 5. Probar PyMC

Abrir Python o notebook y probar:

```python
import pymc as pm
import arviz as az
import numpy as np

with pm.Model():
    x = pm.Normal("x", mu=0, sigma=1)
    trace = pm.sample(100, tune=100)

az.plot_trace(trace)
```

---

## 🧠 ¿Por qué todo esto?

* PyMC usa **PyTensor**, que compila código C++ en tiempo real.
* macOS rompe muchas veces las rutas a `clang++` tras actualizaciones.
* `conda-forge` ofrece binarios precompilados compatibles con Mac ARM (evita muchos errores de compilación).

---

## ✔️ Resultado

* PyMC funcionando correctamente en Mac
* Entorno reproducible y compatible con notebooks VSCode

---

## 🛠️ Siguientes pasos

* Añadir `jupyterlab` si quieres una interfaz visual:

  ```bash
  conda install jupyterlab -c conda-forge
  ```
---

