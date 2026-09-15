# Distribución Normal Estandar

La **distribución normal**, también conocida como **distribución de Gauss** o **campana de Gauss**, es la distribución de probabilidad continua más importante en estadística y teoría de probabilidades. Modela fenómenos naturales, sociales, industriales y computacionales debido a su aparición como límite de sumas de variables aleatorias independientes (**Teorema Central del Límite**).

---

## 1. Definición y Función de Densidad

Una variable aleatoria continua $X$ sigue una distribución normal con parámetros $\mu$ (media) y $\sigma^2$ (varianza), denotado como:

$$X \sim \mathcal{N}(\mu, \sigma^2)$$

Su **Función de Densidad de Probabilidad (FDP)** está dada por:

$$f(x) = \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{1}{2}\left(\frac{x - \mu}{\sigma}\right)^2}, \quad -\infty < x < \infty$$

### Parámetros
- **$\mu$ (Media / Esperanza matemática):** Determina el centro de la distribución (traslación en el eje $X$). Representa la localización de la campana.
- **$\sigma$ (Desviación estándar, $\sigma > 0$):** Determina el grado de dispersión o apertura de la campana. A mayor $\sigma$, la curva es más ancha y achatada (platicúrtica); a menor $\sigma$, es más estrecha y puntiaguda (leptocúrtica).

---

## 2. Propiedades de la Distribución Normal
1. La curva presenta un máximo en $x=\mu$ (Moda)

2. **Simetría:** La curva es perfectamente simétrica respecto al eje vertical media $x=\mu$.

3. **Puntos de inflexión (cambio de concavidad):** Se ubican exactamente a una desviación estándar de la media: en $x = \mu - \sigma$ y $x = \mu + \sigma$.
	- Es cóncava hacia abajo si:
		$\mu-\sigma < x < \mu+\sigma$
	- Es cóncava hacia arriba si (esta en alguna de las colas):
		 $-\infty<x<\mu-\sigma \ \land \ \mu+\sigma<x<\infty$
4. La curva normal se aproxima al eje horizontal$(y=0)$ por las colas de manera **asintótica**.
5. **Área bajo la curva:** El área total comprendida bajo la función de densidad es igual a $1$:
   $$\int_{-\infty}^{\infty} f(x) \, dx = 1$$
### Regla Empírica (Regla 68 - 95 - 99.7)
Para cualquier distribución normal:
- Alrededor del **$68.3\%$** de los datos caen dentro del intervalo $[\mu - \sigma, \mu + \sigma]$.
- Alrededor del **$95.5\%$** de los datos caen dentro del intervalo $[\mu - 2\sigma, \mu + 2\sigma]$.
- Alrededor del **$99.7\%$** de los datos caen dentro del intervalo $[\mu - 3\sigma, \mu + 3\sigma]$.

![[Pasted image 20260907094421.png|646]]
![[Pasted image 20260907100110.png]]

---

## 3. Estandarización de la variable: $X → Z$
Convierte los datos en una escala con $\mu=0$ y $\sigma=1$
### ¿Por qué es necesaria la estandarización?
Para calcular las probabilidades de una VA(Variable Aleatoria) Normal

Se transforma cualquier variable normal $X$ a la variable estándar $Z$. De este modo, basta con una **única tabla de distribución normal estándar** (o una única función computacional) para resolver cualquier problema de probabilidad normal.
### Ecuación de Estandarización ($Z$-Score)
Dada una variable $X \sim \mathcal{N}(\mu, \sigma^2)$, el valor tipificado o estandarizado $Z$ se calcula como:

$P_{normal}(X\le x |\mu;\sigma)$
$$z = \frac{x - \mu}{\sigma}$$
$P_{normal}(Z\le z |\mu=0;\sigma=1)$
### Interpretación del Puntaje $Z$ ($Z$-Score)
El valor $Z$ representa el **número de desviaciones estándar** que un dato $X$ se encuentra alejado de la media $\mu$:
- **$Z = 0$:** El dato coincide exactamente con la media ($X = \mu$).
- **$Z > 0$:** El dato está por encima de la media ($X > \mu$).
- **$Z < 0$:** El dato está por debajo de la media ($X < \mu$).
- **Ejemplo:** $Z = +1.5$ indica que el valor está $1.5$ desviaciones estándar por encima del promedio.

---

### Reglas para el Cálculo de Probabilidades con $Z$

| Tipo de Probabilidad                   | Notación Original  | Estandarización                                                         | Uso de la Función Acumulada $\Phi(z)$ |
| :------------------------------------- | :----------------- | :---------------------------------------------------------------------- | :------------------------------------ |
| **Menor o igual que** (cola izquierda) | $P(X \le a)$       | $P\left(Z \le \frac{a - \mu}{\sigma}\right)$                            | $P(z_a)$                              |
| **Mayor que** (cola derecha)           | $P(X > a)$         | $P\left(Z > \frac{a - \mu}{\sigma}\right)$                              | $1 - P(z_a)$                          |
| **Entre dos valores** (intervalo)      | $P(a \le X \le b)$ | $P\left(\frac{a - \mu}{\sigma} \le Z \le \frac{b - \mu}{\sigma}\right)$ | $P(z_b) - P(z_a)$                     |
| **Simetría negativa**                  | $P(Z \le -z)$      | —                                                                       | $1 - P(z) = P(Z \ge z)$               |


---

### Proceso Inverso: Des-estandarización

Cuando se conoce la probabilidad o percentil deseado y se busca el valor original $X$:
1. Se localiza en la tabla el valor $z$ correspondiente a la probabilidad acumulada $\Phi(z) = p$.
2. Se despeja $X$ de la fórmula de estandarización:

$$x = \mu + z \cdot \sigma$$

---

## 4. Ejemplos Prácticos Paso a Paso

### Ejemplo 1: Cálculo de Probabilidad (Directo)
**Enunciado:** El tiempo de respuesta de las peticiones a un servidor web sigue una distribución normal con media $\mu = 200\text{ ms}$ y desviación estándar $\sigma = 25\text{ ms}$. ¿Cuál es la probabilidad de que una petición tarde entre $180\text{ ms}$ y $235\text{ ms}$?

1. **Definir la variable:** $X \sim \mathcal{N}(200, 25^2)$. Buscamos $P(180 \le X \le 235)$.
2. **Estandarizar los límites:**
   $$z_1 = \frac{180 - 200}{25} = \frac{-20}{25} = -0.80$$
   $$z_2 = \frac{235 - 200}{25} = \frac{35}{25} = 1.40$$
3. **Plantear en términos de $Z$:**
   $$P(180 \le X \le 235) = P(-0.80 \le Z \le 1.40) = \Phi(1.40) - \Phi(-0.80)$$
4. **Consultar la tabla normal estándar:**
   - $\Phi(1.40) \approx 0.9192$
   - $\Phi(-0.80) = 1 - \Phi(0.80) = 1 - 0.7881 = 0.2119$
5. **Calcular:**
   $$P = 0.9192 - 0.2119 = 0.7073 \implies 70.73\%$$

---

### Ejemplo 2: Cálculo de Cuantil / Percentil (Inverso)
**Enunciado:** Con los mismos datos ($\mu = 200$, $\sigma = 25$), ¿cuál es el tiempo de respuesta por debajo del cual se encuentra el $95\%$ de las peticiones más rápidas (Percentil 95)?

1. **Plantear la ecuación:** $P(X \le x) = 0.95 \implies \Phi(z) = 0.95$.
2. **Buscar $z$ en la tabla:**
   - Para un área acumulada de $0.95$, el valor crítico es $z \approx 1.645$.
3. **Des-estandarizar:**
   $$x = \mu + z \cdot \sigma = 200 + (1.645)(25) = 200 + 41.125 = 241.125\text{ ms}$$
4. **Conclusión:** El $95\%$ de las peticiones tarda $241.13\text{ ms}$ o menos.
