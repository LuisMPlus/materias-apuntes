# Medidas de Forma, Curtosis y Teorema de Chebyshev

Las **medidas de forma** y las **acotaciones de concentración** complementan a las medidas de tendencia central y dispersión, permitiendo caracterizar la silueta geométrica, la simetría de las colas, el apuntamiento y las garantías probabilísticas de una distribución de datos.

---

## 1. Medidas de Forma: Asimetría y Sesgo (Skewness)

La **asimetría** evalúa el grado en que los datos se distribuyen equilibradamente a ambos lados de su centro de gravedad (la media aritmética).

![[Pasted image 20260831114614.png]]


---

### 1.1 Índice de Asimetría de Pearson

Karl Pearson formuló coeficientes empíricos basados en la distancia normalizada entre las medidas de tendencia central:

$$Sesgo = \frac{(\bar{x} - \text{Me})}{s}$$

#### Interpretación de los Coeficientes de Pearson:
- **$A_p = 0$:** Distribución **simétrica**.
- **$A_p > 0$:** **Asimetría positiva (sesgada a la derecha)**. Los valores atípicos elevados extienden la cola derecha y arrastran a la media hacia valores mayores que la mediana.
- **$A_p < 0$:** **Asimetría negativa (sesgada a la izquierda)**. Los valores bajos extienden la cola izquierda y arrastran a la media hacia valores menores que la mediana.

---

### 1.2 Coeficiente de Asimetría de Fisher

El coeficiente de Fisher se fundamenta en el **tercer momento central estandarizado** ($m_3$). Al elevar al cubo las desviaciones $(x_i - \bar{x})^3$, conserva el signo de la desviación:

#### A. Tercer Momento Central ($m_3$)
$$m_3 = \frac{1}{n} \sum_{i=1}^{n} (x_i - \bar{x})^3$$

#### B. Coeficiente Poblacional / Muestral Simple 
$$As = \frac{m_3}{s^3} = \frac{\sum_{i=1}^{n}(x_i - \bar{x})^3}{n*s^3}$$

#### C. Coeficiente Muestral Insesgado ($G_1$)
Para muestras de tamaño $n$ (utilizado comúnmente en librerías estadísticas como Scipy o Pandas):

$$G_1 = \frac{n}{(n-1)(n-2)} \sum_{i=1}^{n} \left( \frac{x_i - \bar{x}}{s} \right)^3$$

#### Interpretación del Coeficiente de Fisher:
- **$As= 0$:** Perfecta simetría.
- **$As > 0$:** Asimetría positiva (cola derecha prolongada).
- **$As < 0$:** Asimetría negativa (cola izquierda prolongada).
![[Pasted image 20260831115533.png]]
- **Escala de magnitud:**
  - $|\gamma_1| < 0.5$: Distribución aproximadamente simétrica.
  - $0.5 \le |\gamma_1| \le 1.0$: Asimetría moderada.
  - $|\gamma_1| > 1.0$: Asimetría fuerte y pronunciada.

---

## 2. Medidas de Forma: Curtosis (Apuntamiento)

La **curtosis** (o *kurtosis*) cuantifica la **concentración de datos en el centro** (la agudeza del pico) y el **peso / grosor de las colas** en comparación directa con la distribución normal estándar.

![[Pasted image 20260831115710.png]]

---

### 2.1 Coeficiente de Exceso de Curtosis de Fisher ($\gamma_2$)

Se calcula a partir del **cuarto momento central estandarizado** ($m_4$). Dado que para una distribución normal $\frac{m_4}{\sigma^4} = 3$, se define el **exceso de curtosis** restando $3$:

#### A. Cuarto Momento Central ($m_4$)
$$m_4 = \frac{1}{n} \sum_{i=1}^{n} (x_i - \bar{x})^4$$

#### B. Coeficiente de Exceso de Curtosis ($\gamma_2$)
$$\gamma_2 = \frac{m_4}{s^4} - 3 = \frac{\frac{1}{n}\sum_{i=1}^{n}(x_i - \bar{x})^4}{s^4} - 3$$

---

### 2.2 Clasificación de las Distribuciones según la Curtosis

| Tipo | Exceso ($\gamma_2$) | Cuarto Momento ($\frac{m_4}{s^4}$) | Forma de la Campana | Características de las Colas | Comportamiento de Outliers |
| :--- | :---: | :---: | :--- | :--- | :--- |
| **Mesocúrtica** | $\gamma_2 = 0$ | $= 3$ | Concentración estándar (campana clásica). | Colas normales intermedias. | Frecuencia estándar esperada en una normal. |
| **Leptocúrtica** | $\gamma_2 > 0$ | $> 3$ | Pico central agudo, elevado y puntiagudo. | **Colas pesadas / gruesas** (fat tails). | **Alta propensión a valores atípicos extremos**. Común en finanzas y fallos de sistemas. |
| **Platicúrtica** | $\gamma_2 < 0$ | $< 3$ | Curva aplanada, achatada o en forma de meseta. | **Colas delgadas / ligeras**. | **Muy baja probabilidad de valores extremos**. Común en distribuciones uniformes. |

---

## 3. Teorema de Chebyshev (Desigualdad de Chebyshev)

De acuerdo con este teorema para z 2, 3 y 4 desviaciones estándar se 
tiene
- Por lo menos 0.75, o 75%, de los valores de los datos deben estar 
dentro de z 2 desviaciones estándar de la media.
- Al menos 0.89, o 89%, de los valores deben estar dentro de z 3 
desviaciones estándar de la media.
-  Por lo menos 0.94, o 94%, de los valores deben estar dentro de z 4 
desviaciones estándar de la media

---
### 3.2 Regla empírica
Cuando los datos tienen una distribución en forma de campana:
- Cerca de 68% de los valores de los datos se encontrarán a no más de una desviación estándar desde la media.
- Aproximadamente 95% de los valores de los datos se encontrarán a 
no más de dos desviaciones estándar desde la media.
- Casi todos los valores de los datos estarán a no más de tres 
desviaciones estándar de la media

> [!IMPORTANT]
> - **Chebyshev** es una cota **universal y no paramétrica**. Actúa como piso mínimo de seguridad en situaciones donde se desconoce el comportamiento estocástico del fenómeno.
> - La **Regla Empírica** es mucho más precisa, pero su aplicación exige que el fenómeno siga rigurosamente un modelo normal.

---

## 4. Ejemplo Numérico Integrador

Consideremos una muestra de $n = 10$ tiempos de latencia (en ms):

$$\{ 20, 22, 23, 23, 24, 25, 26, 28, 30, 75 \}$$

- **Estadísticos base:**
  - Media: $\bar{x} = 29.60\text{ ms}$
  - Mediana: $\text{Me} = \frac{24 + 25}{2} = 24.50\text{ ms}$
  - Moda: $\text{Mo} = 23.00\text{ ms}$
  - Desviación estándar muestral: $s \approx 16.32\text{ ms}$

---

### 1. Cálculo de Asimetría:
- **Pearson 1:** $A_{p1} = \frac{29.60 - 23.00}{16.32} = \frac{6.60}{16.32} \approx \mathbf{+0.40}$
- **Pearson 2:** $A_{p2} = \frac{3(29.60 - 24.50)}{16.32} = \frac{15.30}{16.32} \approx \mathbf{+0.94}$
- **Fisher ($\gamma_1$):** $\gamma_1 \approx \mathbf{+2.68}$ (fuerte asimetría positiva a la derecha por la latencia de $75\text{ ms}$).

### 2. Cálculo de Curtosis:
- **Exceso de Curtosis ($\gamma_2$):** $\gamma_2 \approx \mathbf{+7.51} > 0 \implies$ **Leptocúrtica** (alta concentración en el rango $20-30\text{ ms}$ con cola derecha pesada).


---

## 5. Implementación en Python

```python
import numpy as np
import pandas as pd
from scipy import stats

datos = np.array([20, 22, 23, 23, 24, 25, 26, 28, 30, 75])
s = pd.Series(datos)
n = len(datos)

media = s.mean()
mediana = s.median()
moda = s.mode().tolist()[0]
std_m = s.std(ddof=1)

# Asimetría
p1 = (media - moda) / std_m
p2 = 3 * (media - mediana) / std_m
fisher_skew = stats.skew(datos, bias=False)

# Curtosis
fisher_kurt = stats.kurtosis(datos, bias=False)
tipo_curt = "Leptocúrtica" if fisher_kurt > 0 else ("Platicúrtica" if fisher_kurt < 0 else "Mesocúrtica")

# Teorema de Chebyshev (k = 2)
k = 2
lim_inf, lim_sup = media - k * std_m, media + k * std_m
dentro = np.sum((datos >= lim_inf) & (datos <= lim_sup))
pct_real = (dentro / n) * 100
cota_chebyshev = (1 - 1 / (k**2)) * 100

print("=== ASIMETRÍA Y FORMA ===")
print(f"Coef. Pearson 1 (Moda)   : {p1:.2f}")
print(f"Coef. Pearson 2 (Mediana): {p2:.2f}")
print(f"Fisher Skewness (g1)     : {fisher_skew:.2f} (Sesgo positivo a la derecha)")

print("\n=== CURTOSIS ===")
print(f"Exceso de Curtosis (g2)  : {fisher_kurt:.2f} -> {tipo_curt}")

print("\n=== TEOREMA DE CHEBYSHEV (k = 2) ===")
print(f"Intervalo [μ - 2σ, μ + 2σ]: [{lim_inf:.2f}, {lim_sup:.2f}]")
print(f"Cota Mínima Garantizada   : {cota_chebyshev:.2f}%")
print(f"Porcentaje Real Observado : {pct_real:.2f}% ({dentro}/{n} datos)")
```

---

## 🔗 Temas Relacionados

- **[[Materias/Probabilidad y Estadistica/Estadística/Estadística Descriptiva/Conceptos|Conceptos Fundamentales y Medidas de Tendencia Central y Dispersión]]**: Población, variables, media, mediana, moda, varianza y desviación estándar.
- **[[Materias/Probabilidad y Estadistica/Estadística/Estadística Descriptiva/Distribución de Frecuencias|Distribución de Frecuencias y Tablas de Contingencia]]**: Tablas simples, acumuladas y análisis bivariado.
- **[[Materias/Probabilidad y Estadistica/Estadística/Estadística Descriptiva/Graficas|Gráficas Estadísticas]]**: Representaciones visuales (histogramas, boxplots, ojivas).
