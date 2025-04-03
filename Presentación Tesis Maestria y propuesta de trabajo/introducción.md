### Elaboración de un algoritmo aplicado a imágenes obtenidas por dron para la cuantificación de Fitoplancton.

Universidad Distrital Francisco José de Caldas
Maestría en Ciencias de la Información y las Comunicaciones

**Autor: Iván Darío Carrillo Durán**

#### Objetivos

##### General:

Crear un modelo matemático aplicado a imágenes de sensores abordo de dron para la cuantificación de la concentración total de Fitoplancton en (mg m−3) en las aguas de la laguna de Tota.

##### Específicos:

1. Identificar el comportamiento de la reflectancia de la clorofila-a en el área de estudio ubicada en la laguna de Tota.
2. Determinar cualitativamente las consecuencias del fenómeno de brillo del sol y de rugosidad del agua en la determinación de la concentración de la clorofila-a partir de las imágenes de dron.
3. Proponer una técnica de mosaico de las imágenes de dron que no altere los niveles digitales de las imágenes individuales.
4. Analizar la distribución espacial del Fitoplancton empleando el algoritmo de detección de clorofila-a sobre imágenes Landsat 8 y Sentinel 2B.
5. Evaluar y validar los resultados obtenidos del modelo matemático obtenido con las mediciones in-situ y las imágenes de dron contra los obtenidos por imagen de satélite Landsat 8 y Sentinel 2B.

### Presentación

![alt text](images/presentacion-maestria-1.jpg)
![alt text](images/presentacion-maestria-2.jpg)

#### Introducción

![alt text](images/presentacion-maestria-3.jpg)
![alt text](images/presentacion-maestria-4.jpg)

#### Objetivos

![alt text](images/presentacion-maestria-5.jpg)

#### Marco Teórico

![alt text](images/presentacion-maestria-6.jpg)
![alt text](images/presentacion-maestria-7.jpg)
![alt text](images/presentacion-maestria-8.jpg)

#### Algoritmo SIFT

El algoritmo SIFT (Scale-Invariant Feature Transform) es un método clásico en visión por computadora, propuesto por David Lowe en 1999, diseñado para detectar y describir características locales (keypoints) en imágenes de manera invariante a escala, rotación, iluminación y cierto grado de distorsión perspectiva. Consiste en cuatro etapas principales:

1. Detección de puntos clave invariantes a escala Utiliza una pirámide de diferencias de Gaussianas (DoG) para identificar regiones estables en diferentes escalas.

   Los máximos y mínimos locales en esta pirámide se consideran puntos clave candidatos, que luego se filtran para eliminar aquellos con bajo contraste o localizados en bordes (usando la curvatura hessiana).

2. Localización precisa de puntos clave Mediante interpolación, se ajusta la posición, escala y relación de curvatura de cada punto clave para mejorar su precisión.

3. Asignación de orientación Para lograr invarianza a rotación, se calcula la orientación dominante del gradiente en la región alrededor de cada punto clave (usando histogramas de gradientes).

4. Descriptores de características Se genera un vector descriptor de 128 dimensiones basado en los gradientes locales (divididos en subregiones de 4×4), que captura la textura y geometría alrededor del punto clave.

El algoritmo **SIFT** (_Scale-Invariant Feature Transform_) se basa en una serie de operaciones matemáticas para detectar y describir características invariantes. A continuación, se detallan las fórmulas clave paso a paso:

---

### **1. Construcción de la Pirámide de Escala (Scale Space)**

La imagen se suaviza a diferentes escalas usando una **Gaussiana**:
\[
L(x, y, \sigma) = G(x, y, \sigma) \* I(x, y)
\]
donde:

- \( I(x, y) \) es la imagen original.
- \( G(x, y, \sigma) = \frac{1}{2\pi\sigma^2} e^{-\frac{x^2 + y^2}{2\sigma^2}} \) es el filtro Gaussiano.
- \( \* \) denota convolución.

Se genera una **pirámide de escalas** mediante el incremento progresivo de \( \sigma \) (usando \( \sigma = k^n \sigma_0 \), donde \( k \) es un factor de escala).

---

### **2. Diferencias de Gaussianas (DoG)**

Para detectar regiones estables, se resta niveles consecutivos de la pirámide Gaussiana:
\[
D(x, y, \sigma) = L(x, y, k\sigma) - L(x, y, \sigma)
\]
Los **puntos clave candidatos** son los máximos/mínimos locales en \( D(x, y, \sigma) \), comparados con sus 8 vecinos en la misma escala y los 9 vecinos en escalas adyacentes.

---

### **3. Localización Precisa de Puntos Clave**

Se ajusta la posición del punto clave mediante una **aproximación cuadrática** (Taylor expansion de \( D \)):
\[
\hat{x} = -\frac{\partial^2 D^{-1}}{\partial x^2} \frac{\partial D}{\partial x}
\]
Si \( |D(\hat{x})| < \text{umbral} \), se descarta el punto (bajo contraste).  
Además, se eliminan puntos en bordes usando la **matriz Hessiana**:
\[
H = \begin{bmatrix}
D*{xx} & D*{xy} \\
D*{xy} & D*{yy}
\end{bmatrix}, \quad \text{Traza}(H)^2 / \text{Det}(H) < \frac{(r+1)^2}{r}
\]
(\( r = 10 \), umbral para eliminar respuestas de bordes).

---

### **4. Asignación de Orientación**

Se calcula la **orientación dominante** del gradiente en una ventana alrededor del punto clave:
\[
m(x, y) = \sqrt{(L(x+1, y) - L(x-1, y))^2 + (L(x, y+1) - L(x, y-1))^2}
\]
\[
\theta(x, y) = \tan^{-1}\left(\frac{L(x, y+1) - L(x, y-1)}{L(x+1, y) - L(x-1, y)}\right)
\]
Se genera un histograma de 36 bins (10° cada uno) y se selecciona la orientación con mayor magnitud (y picos secundarios > 80% del máximo).

---

### **5. Descriptor SIFT (128-D)**

- Se divide la región alrededor del punto en subventanas de \( 4 \times 4 \).
- En cada subventana, se calcula un histograma de 8 orientaciones (gradientes ponderados por magnitud y suavizados con Gaussiana).
- Se concatenan los 16 histogramas (\( 4 \times 4 \times 8 = 128 \) dimensiones).
- El descriptor se normaliza para ser invariante a cambios de iluminación.

---

### **Resumen Matemático Clave**

| Paso             | Fórmula Principal                                                                     | Objetivo                   |
| ---------------- | ------------------------------------------------------------------------------------- | -------------------------- |
| **Scale Space**  | \( L(x, y, \sigma) = G(x, y, \sigma) \* I(x, y) \)                                    | Suavizado multiescala.     |
| **DoG**          | \( D(x, y, \sigma) = L(x, y, k\sigma) - L(x, y, \sigma) \)                            | Detección de puntos clave. |
| **Localización** | \( \hat{x} = -\frac{\partial^2 D^{-1}}{\partial x^2} \frac{\partial D}{\partial x} \) | Ajuste sub-pixel.          |
| **Orientación**  | \( \theta(x, y) = \tan^{-1}\left(\frac{\Delta_y L}{\Delta_x L}\right) \)              | Invarianza a rotación.     |
| **Descriptor**   | Histogramas \( 4 \times 4 \times 8 \)                                                 | Vector 128-D invariante.   |

Este enfoque matemático permite a SIFT ser robusto frente a transformaciones geométricas y fotométricas, aunque hoy existen alternativas más rápidas (como **ORB** o basadas en redes neuronales).

![alt text](image.png)

![alt text](image-1.png)

![alt text](images/presentacion-maestria-9.jpg)
![alt text](images/presentacion-maestria-10.jpg)

#### Metodología

![alt text](images/presentacion-maestria-11.jpg)
![alt text](images/presentacion-maestria-12.jpg)

#### Zona de Estudio

![alt text](images/presentacion-maestria-13.jpg)

#### Materiales

![alt text](images/presentacion-maestria-14.jpg)
![alt text](images/presentacion-maestria-15.jpg)
![alt text](images/presentacion-maestria-16.jpg)
![alt text](images/presentacion-maestria-17.jpg)
![alt text](images/presentacion-maestria-18.jpg)
![alt text](images/presentacion-maestria-19.jpg)

#### Resultados

![alt text](images/presentacion-maestria-20.jpg)
![alt text](images/presentacion-maestria-21.jpg)
![alt text](images/presentacion-maestria-22.jpg)
![alt text](images/presentacion-maestria-23.jpg)
![alt text](images/presentacion-maestria-24.jpg)
![alt text](images/presentacion-maestria-25.jpg)
![alt text](images/presentacion-maestria-26.jpg)
![alt text](images/presentacion-maestria-27.jpg)
![alt text](images/presentacion-maestria-28.jpg)
![alt text](images/presentacion-maestria-29.jpg)
![alt text](images/presentacion-maestria-30.jpg)

#### Análisis de resultados

![alt text](images/presentacion-maestria-31.jpg)
![alt text](images/presentacion-maestria-32.jpg)
![alt text](images/presentacion-maestria-33.jpg)
![alt text](images/presentacion-maestria-34.jpg)
![alt text](images/presentacion-maestria-35.jpg)

#### Conclusiones y recomendaciones

![alt text](images/presentacion-maestria-36.jpg)
![alt text](images/presentacion-maestria-37.jpg)

![alt text](images/presentacion-maestria-38.jpg)
![alt text](images/presentacion-maestria-39.jpg)

#### Anexos

![alt text](images/presentacion-maestria-40.jpg)

[Video Muestreo de Agua - Parte 1](https://www.youtube.com/watch?v=J7TLZfONQGo)
[Video Muestreo de Agua - Parte 2](https://www.youtube.com/watch?v=3W2f6Yw1CTE&t=3s)

![alt text](images/presentacion-maestria-41.jpg)

[Repositorio de Código](https://github.com/ivankarrillin/Algoritmo-SIFT)

![alt text](images/presentacion-maestria-42.jpg)
![alt text](images/presentacion-maestria-43.jpg)
