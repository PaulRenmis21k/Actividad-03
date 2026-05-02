# Actividad 03 — Algoritmos de Inteligencia de Enjambre

**Curso:** Inteligencia Artificial  
**Institución:** Universidad Nacional del Altiplano — Facultad de Ingeniería de Sistemas  
**Autor:** Paul Renmis  

---

## Descripcion general

Este repositorio contiene la implementacion de tres algoritmos bio-inspirados de optimizacion por inteligencia de enjambre, aplicados a problemas clasicos de aprendizaje automatico. Cada notebook es autocontenido e incluye fundamento teorico, implementacion desde cero y evaluacion experimental.

---

## Estructura del repositorio

```
Actividad-03/
├── 01_ABC_Feature_Selection_POL.ipynb
├── 03_PSO_NN_Training_Without_Backpropagation.ipynb
├── 04_PSO_Clustering.ipynb
└── README.md
```

---

## Contenido

### Notebook 01 — Seleccion de Caracteristicas con ABC

**Archivo:** `01_ABC_Feature_Selection_POL.ipynb`

Implementacion del Algoritmo de Colonia de Abejas Artificiales (Artificial Bee Colony, ABC) para la seleccion de subconjuntos optimos de caracteristicas en problemas de clasificacion binaria.

**Problema:** Seleccion de caracteristicas sobre el dataset Breast Cancer Wisconsin (569 muestras, 30 caracteristicas).

**Metodologia:**
- Codificacion binaria de soluciones (vector de presencia/ausencia de caracteristicas).
- Funcion de aptitud combinada: precision en validacion cruzada estratificada (5 folds) ponderada con un componente de parsimonia segun la formula:

$$f(x) = \alpha \cdot \text{Accuracy}_{CV}(x) + (1 - \alpha) \cdot \left(1 - \frac{|\{i : x_i=1\}|}{d}\right), \quad \alpha = 0.9$$

- Clasificador base: Random Forest (50 estimadores).
- Fases del algoritmo: abejas empleadas (mutacion por bit-flip con seleccion codiciosa), abejas observadoras (seleccion proporcional a la aptitud por ruleta) y abejas exploradoras (abandono por contador de intentos).

**Parametros de ejecucion:**

| Parametro | Valor |
|---|---|
| Numero de abejas | 20 |
| Iteraciones maximas | 40 |
| Limite de abandono | 8 |
| Alpha (precision) | 0.9 |

---

### Notebook 03 — Entrenamiento de Red Neuronal con PSO sin Retropropagacion

**Archivo:** `03_PSO_NN_Training_Without_Backpropagation.ipynb`

Implementacion del entrenamiento de una red neuronal multicapa utilizando Optimizacion por Enjambre de Particulas (Particle Swarm Optimization, PSO) como unico mecanismo de ajuste de parametros, prescindiendo del algoritmo de retropropagacion del error.

**Problema:** Clasificacion multiclase sobre el dataset Iris (150 muestras, 4 caracteristicas, 3 clases).

**Arquitectura de la red:**

| Capa | Tipo | Neuronas | Activacion |
|---|---|---|---|
| Entrada | Input | 4 | — |
| Oculta 1 | Dense | 10 | ReLU |
| Oculta 2 | Dense | 8 | ReLU |
| Salida | Dense | 3 | Softmax |

**Metodologia:**
- Cada particula codifica el vector completo de pesos y sesgos de la red como un vector continuo de dimension igual al total de parametros entrenables.
- La funcion de aptitud es el negativo de la perdida de entropia cruzada sobre el conjunto de entrenamiento (PSO maximiza, el objetivo es minimizar la perdida).
- Decodificacion de pesos por segmentacion secuencial del vector de particula en matrices W1, b1, W2, b2, W3, b3.
- Propagacion hacia adelante implementada desde cero con Softmax numericamente estable.

---

### Notebook 04 — Clustering con PSO

**Archivo:** `04_PSO_Clustering.ipynb`

Aplicacion de PSO al problema de agrupamiento no supervisado, donde el algoritmo optimiza directamente las posiciones de los centroides en el espacio de caracteristicas.

**Problema:** Segmentacion de clientes sobre un dataset sintetico con parametros reales (200 muestras, 3 variables: edad, ingreso anual, puntuacion de gasto; 5 segmentos naturales).

**Metodologia:**
- Cada particula codifica k centroides como un vector continuo de dimension k x n\_features.
- Funcion de aptitud: negativo de la Suma de Cuadrados Intra-Cluster (WCSS) normalizada por el numero de puntos.
- Asignacion de puntos al centroide mas cercano por distancia euclidiana al cuadrado.
- Inicializacion de posiciones mediante seleccion aleatoria de puntos reales del dataset (estrategia analoga a K-Means++).
- Coeficiente de inercia con decaimiento lineal a lo largo de las iteraciones.
- Evaluacion de calidad del clustering mediante los indices Silhouette, Davies-Bouldin y Calinski-Harabasz; comparacion contra K-Means estandar.

**Parametros de ejecucion:**

| Parametro | Valor |
|---|---|
| Numero de clusters (k) | 5 |
| Numero de particulas | 30 |
| Iteraciones maximas | 100 |
| Inercia inicial (w) | 0.7 |
| Coeficiente cognitivo (c1) | 1.5 |
| Coeficiente social (c2) | 1.5 |

---

## Dependencias

Todos los notebooks fueron desarrollados en Python 3. Las bibliotecas requeridas son:

```
numpy
pandas
matplotlib
seaborn
scikit-learn
```

Instalacion:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn
```

---

## Ejecucion

Los notebooks pueden ejecutarse localmente con Jupyter o directamente en Google Colab. Se recomienda ejecutar las celdas en orden secuencial. La semilla aleatoria esta fijada en `42` en todos los experimentos para garantizar reproducibilidad.

---

