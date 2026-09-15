
```bash
awk '{print $2, $3, $4, $11}'
```
`awk` procesa cada línea de entrada y la divide en campos separados por espacios.

- `{ ... }`: indica las instrucciones que ejecutará para cada línea.
- `print`: imprime información.
- `$2`: segundo campo, el `PID`.
- `$3`: tercer campo, `%CPU`.
- `$4`: cuarto campo, `%MEM`.
- `$11`: undécimo campo, `COMMAND`.
- Las comas agregan espacios entre los valores impresos.
- Las comillas simples `'...'` evitan que Bash intente interpretar los símbolos `$`.