# Documentación de la clase de Machine Learning

**Machine Learning (ML)** es una rama de la inteligencia artificial que permite a las computadoras aprender de datos sin ser programadas explícitamente. Mediante algoritmos, los sistemas identifican patrones, hacen predicciones o toman decisiones basadas en información previa, mejorando su precisión con la experiencia. Se usa en aplicaciones como recomendaciones, reconocimiento de voz, diagnóstico médico y autos autónomos. En esencia, el ML transforma permite generar conocimiento a paritr del entendimiento de los datos.

### Link al repositorio de código en Google Colab para cada tarea:

### Ejercicio de Rimas

**Las rimas en español** son la repetición de sonidos al final de dos o más versos, a partir de la última vocal acentuada. Pueden ser _consonantes_ (iguales sonidos en vocales y consonantes) o _asonantes_ (solo coinciden las vocales). Son clave en poesía, música y juegos infantiles.

_Fuente: Real Academia Española (RAE). "Diccionario de la lengua española" (Ed. 23.ª, 2014)._

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ivankarrillin/ML_Doc/blob/main/ejercicio_rimas.ipynb)

### Ejercicio de sufijos (tarea Extra)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ivankarrillin/ML_Doc/blob/main/ejercicio_sufijos.ipynb)

### Tutorial Pandas

**Pandas** es una biblioteca de Python para análisis y manipulación de datos. Ofrece estructuras eficientes como _DataFrames_ y _Series_, permitiendo limpieza, filtrado, agregación y visualización de datos. Esencial en ciencia de datos, integrable con NumPy y Scikit-learn.

_Fuente: McKinney, W. (2010). "Data Structures for Statistical Computing in Python". Proceedings of SciPy._

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ivankarrillin/ML_Doc/blob/main/tutorial_pandas.ipynb)

### Tutorial de Numpy

> :warning: Tarea que faltaba

**NumPy** es una biblioteca fundamental para computación científica en Python que proporciona soporte para arrays y matrices multidimensionales, junto con una amplia colección de funciones matemáticas de alto nivel para operar con estas estructuras de datos. Es el paquete básico para computación numérica en Python, permitiendo operaciones vectorizadas y matriciales eficientes, lo que lo hace esencial para aplicaciones de análisis de datos, machine learning, procesamiento de señales y álgebra lineal. NumPy destaca por su rendimiento optimizado (está escrito en C y Fortran) y su sintaxis concisa, siendo la base sobre la que se construyen muchas otras bibliotecas científicas del ecosistema Python como Pandas, SciPy y scikit-learn. Fuente: https://numpy.org/doc/stable/user/absolute_beginners.html

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ivankarrillin/ML_Doc/blob/main/numpy/numpy.ipynb)

### Tutorial Kaggle

**Kaggle** es una plataforma en línea para competencias de ciencia de datos y aprendizaje automático, donde usuarios comparten datasets, colaboran en proyectos y desarrollan modelos. Ofrece recursos educativos, kernels (notebooks) y acceso a herramientas cloud. Ideal para practicar, aprender y destacar en la comunidad de IA.

_Fuente: Kaggle.com_

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ivankarrillin/ML_Doc/blob/main/tutorial_kaggle.ipynb)

### Árboles de decisión

**Los árboles de decisión** son modelos de _machine learning_ que dividen datos en nodos mediante reglas **if-else**, basadas en características. Cada nodo representa una pregunta, cada rama una decisión y cada hoja un resultado. Son intuitivos, pero pueden sufrir _overfitting_. Se usan en clasificación y regresión. 🌳📊

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ivankarrillin/ML_Doc/blob/main/decision_tree.ipynb)

### Hopfield

**La red de Hopfield** es un modelo de red neuronal recurrente que almacena patrones como estados estables, actuando como memoria asociativa. Usa aprendizaje hebbiano para recuperar información incluso con entradas parciales o ruidosas. Aplicada en optimización y reconocimiento de patrones, aunque tiene limitaciones en capacidad de almacenamiento.

_Fuente: Hopfield, J. J. (1982). "Neural networks and physical systems with emergent collective computational abilities". PNAS._

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ivankarrillin/ML_Doc/blob/main/Hopfield.ipynb)

### PCA

**PCA (Análisis de Componentes Principales)** es una técnica de reducción de dimensionalidad que transforma datos complejos en componentes ortogonales, ordenados por varianza explicada. Identifica patrones clave, eliminando redundancias y ruido, facilitando visualización y análisis. Usado en imágenes, genómica y finanzas, simplifica modelos sin perder información esencial. ¡Eficaz para datos correlacionados!
_Fuente: Jolliffe, I. T. (2002). "Principal Component Analysis". Springer._

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ivankarrillin/ML_Doc/blob/main/PCA.ipynb)

### Ejercicio de detección dedo índice

[Ver ejercicio](https://ivankarrillin.github.io/ML_Doc/Hand_Detection_LandMark_IvanCarrillo.html)

**Función para encontrar el coseno de dos vectores**

```javascript
function calcularCosenoEntreVectores(landmarks, idx1, idx2, idx3, idx4) {
  const x1 = landmarks[idx2].x - landmarks[idx1].x;
  const y1 = landmarks[idx2].y - landmarks[idx1].y;
  const x2 = landmarks[idx4].x - landmarks[idx3].x;
  const y2 = landmarks[idx4].y - landmarks[idx3].y;

  const producto_punto = x1 * x2 + y1 * y2;
  const norm1 = Math.sqrt(x1 * x1 + y1 * y1);
  const norm2 = Math.sqrt(x2 * x2 + y2 * y2);
  // división por cero (si algún vector es nulo)
  if (norm1 === 0 || norm2 === 0) {
    return 0;
  }

  const coseno = producto_punto / (norm1 * norm2);
  return coseno;
}
```

![alt text](image.png)
