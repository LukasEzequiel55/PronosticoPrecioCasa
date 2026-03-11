# 🏠 Pronóstico del Precio de una Casa — Regresión Lineal Múltiple

> Predicción del precio de venta de casas usando OLS resuelto analíticamente mediante álgebra matricial, validado contra `statsmodels`.

---

## 📌 Descripción General

Este proyecto construye un modelo de **Regresión Lineal Múltiple** para pronosticar el precio de venta de inmuebles a partir de sus características observables. El objetivo principal no es solo predecir, sino **entender las matemáticas detrás del algoritmo** — resolviendo OLS analíticamente con la fórmula matricial:

$$\hat{\beta} = (X'X)^{-1}X'Y$$

Los resultados se validan contra `statsmodels.OLS` para confirmar equivalencia matemática exacta.

---

## 🎯 Variable Objetivo

| Variable | Descripción |
|----------|-------------|
| `price` | Precio de venta de una casa en dólares |

---

## 📊 Dataset y Selección de Variables

Las siguientes variables fueron **excluidas desde el inicio**:

| Variable | Motivo |
|----------|--------|
| `id`, `date` | Identificadores sin valor predictivo |
| `zipcode` | Categórica sin escala lineal |
| `lat`, `long` | Coordenadas crudas sin relación lineal directa |
| `sqft_above` | Colineal con `sqft_living` |
| `yr_renovated` | ~96% de valores en cero |

Las **11 variables candidatas** que ingresaron al proceso de selección estadística:

| Variable | Descripción |
|----------|-------------|
| `bedrooms` | Número de habitaciones |
| `bathrooms` | Número de baños |
| `sqft_living` | Superficie habitable (ft²) — mayor correlación individual con el precio |
| `sqft_lot` | Superficie del terreno (ft²) |
| `floors` | Número de pisos |
| `waterfront` | Vista al agua (0/1) |
| `view` | Calidad de la vista (0–4) |
| `condition` | Estado de conservación (1–5) |
| `grade` | Calidad de construcción (1–13) |
| `sqft_basement` | Superficie del sótano (ft²) |
| `yr_built` | Año de construcción |

---

## 🔁 Eliminación Hacia Atrás (α = 0.05)

Se partió del modelo completo con 11 variables y se eliminaron iterativamente aquellas con p-value superior a **0.05**.

| Iteración | Acción |
|-----------|--------|
| **1** | Eliminar `sqft_basement` — p-value = 0.3967 ❌ |
| **2** | Todas las variables restantes son significativas (p < 0.0001) ✅ — **PARAR** |

**Modelo final: 10 variables**, todas con p-value < 0.0001.

---

## 📐 Coeficientes del Modelo Final

| Variable | Coeficiente | t-stat | IC 95% |
|----------|-------------|--------|--------|
| Intercepto | 6,216,011.72 | 41.58 | [5,922,968 — 6,509,054] |
| `bedrooms` | −34,441.54 | −14.90 | [−38,972 — −29,910] |
| `bathrooms` | 44,274.79 | 10.79 | [36,232 — 52,317] |
| `sqft_living` | 162.91 | 48.50 | [156.33 — 169.49] |
| `sqft_lot` | −0.24 | −5.90 | [−0.317 — −0.159] |
| `floors` | 27,512.42 | 6.24 | [18,872 — 36,152] |
| `waterfront` | 563,462.16 | 28.19 | [524,286 — 602,638] |
| `view` | 43,372.89 | 15.93 | [38,037 — 48,708] |
| `condition` | 16,625.68 | 5.69 | [10,895 — 22,355] |
| `grade` | 123,645.98 | 44.24 | [118,167 — 129,124] |
| `yr_built` | −3,576.88 | −46.89 | [−3,726 — −3,427] |

---

## ✅ Evaluación del Modelo

| Métrica | Valor |
|---------|-------|
| R² — Entrenamiento (70%) | **65.1%** |
| R² — Prueba (30%) | **65.3%** |
| Estadístico F | **2,566** |
| Variables finales | **10** |

El R² casi idéntico entre entrenamiento y prueba confirma que **no hay sobreajuste**.  
Los residuales sobre el conjunto de prueba siguen una distribución aproximadamente normal, validando los supuestos del modelo.

---

## 🔬 Validación: Matricial vs. OLS Automatizado

Los coeficientes calculados manualmente con NumPy mediante β = (X'X)⁻¹X'Y son **idénticos** a los producidos por `statsmodels.OLS` (diferencia < 10⁻⁴ en todas las variables), confirmando comprensión profunda de los fundamentos matemáticos de la regresión lineal.

---

## 💡 Hallazgos Clave

- **La calidad supera al tamaño**: aunque `sqft_living` importa, `waterfront` (+$563K) y `grade` (+$123K por nivel) tienen un impacto económico mucho mayor.
- **El año de construcción es un fuerte depresor**: cada año adicional de antigüedad reduce el precio estimado en ~$3,577.
- **Las habitaciones tienen coeficiente negativo** al controlar las demás variables — más cuartos en una superficie fija implica espacios más pequeños.
- **`sqft_basement` no aporta valor** una vez incluidas las demás variables de tamaño — eliminada en la iteración 1.

---

## 🛠️ Stack Tecnológico

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![NumPy](https://img.shields.io/badge/NumPy-álgebra_matricial-013243?logo=numpy)
![Pandas](https://img.shields.io/badge/Pandas-manipulación_de_datos-150458?logo=pandas)
![statsmodels](https://img.shields.io/badge/statsmodels-validación_OLS-green)
![Matplotlib](https://img.shields.io/badge/Matplotlib-visualización-orange)

---

## 📁 Estructura del Proyecto

```
house-price-regression/
│
├── data/
│   └── kc_house_data.csv              # Dataset de ventas de casas King County
│
├── notebooks/
│   └── pronostico_precio_casa.ipynb   # Análisis completo y desarrollo del modelo
│
├── outputs/
│   └── Resumen_Pronostico_Casa.pdf    # Reporte resumen del proyecto
│
└── README.md
```

---

## 👤 Autor

**Ulises Abraham Ortiz Sánchez**  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-uliabraham-blue?logo=linkedin)](https://linkedin.com/in/uliabraham)
[![Kaggle](https://img.shields.io/badge/Kaggle-uliabraham-20BEFF?logo=kaggle)](https://www.kaggle.com/uliabraham)
[![Portafolio](https://img.shields.io/badge/Portafolio-lukasezequiel55-black?logo=google-chrome)](https://portafoliocv.lukasezequiel55.com)