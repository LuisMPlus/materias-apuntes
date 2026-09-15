## Normalización
Escala los datos entre 0 y 1
$$x'=\frac{x-x_{min}}{x_{max}-x_{min}}$$
## Desventajas:
- Sensible a valores atípicos(outliers): si no se tratan comprimirá el intervalo de los demás datos
### Diferencia entre Normalización y Estandarización:
- **Estandarización ($Z$-Score):** $z = \frac{x - \mu}{\sigma}$ $\rightarrow$ Centrada en $0$, no acota a un rango fijo.
- **Min-Max Scaling (Normalización $[0, 1]$):** $x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$ $\rightarrow$ Acota estrictamente en el intervalo $[0, 1]$.
