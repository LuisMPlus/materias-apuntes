## 1. Estandarización de la variable: $X → Z$
Convierte los datos en una escala con $\mu=0$ y $\sigma=1$
### Ecuación de Estandarización ($Z$-Score)
Dada una variable $X \sim \mathcal{N}(\mu, \sigma^2)$, el valor tipificado o estandarizado $Z$ se calcula como:

$P_{normal}(X\le x |\mu;\sigma)$
$$z = \frac{x - \mu}{\sigma}$$
### Interpretación del Puntaje $Z$ ($Z$-Score)
El valor $Z$ representa el **número de desviaciones estándar** que un dato $X$ se encuentra alejado de la media $\mu$:
- **$Z = 0$:** El dato coincide exactamente con la media ($X = \mu$).
- **$Z > 0$:** El dato está por encima de la media ($X > \mu$).
- **$Z < 0$:** El dato está por debajo de la media ($X < \mu$).
- **Ejemplo:** $Z = +1.5$ indica que el valor está $1.5$ desviaciones estándar por encima del promedio.
---

## 2. Propiedades
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
---
### 3. Regla Empírica (Regla 68 - 95 - 99.7)
Para cualquier distribución normal:
- Alrededor del **$68.3\%$** de los datos caen dentro del intervalo $[\mu - \sigma, \mu + \sigma]$.
- Alrededor del **$95.5\%$** de los datos caen dentro del intervalo $[\mu - 2\sigma, \mu + 2\sigma]$.
- Alrededor del **$99.7\%$** de los datos caen dentro del intervalo $[\mu - 3\sigma, \mu + 3\sigma]$.

![[Pasted image 20260907094421.png|646]]
