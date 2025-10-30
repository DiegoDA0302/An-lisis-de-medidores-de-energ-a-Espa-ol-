# Informe Final: Análisis del Estado Actual del Sistema de Medidores

## Objetivo

Este proyecto se realiza como prueba técnica del proceso de selección para el cargo de Analista de datos II en EnerBit.

Este archivo tiene como finalidad describir paso a paso de lo que se realizó en el desarrollo de la prueba.

## Fuentes de Datos

Se integraron y depuraron tres conjuntos de datos en formato Excel:

1. **resumen_criticas_v2.xlsx** – Registro de críticas a medidores.
2. **estados_y_seriales_v2.xlsx** – Información sobre ubicación y seriales.
3. **certificados_v3.xlsx** – Datos de visitas técnicas realizadas.

## Limpieza e Integración de Datos

Durante la limpieza se abordaron:

- Valores nulos y duplicados.
- Inconsistencias en los tipos de datos.
- Conversión de fechas.
- Cruces entre tablas para enriquecer el análisis.

1. Lo primero que veo es que tengo una columna llamada 'state' que corresponde a los departamentos de Colombia que estamos evaluando en este análisis, sin embargo y por conocimiento de la geografía colombiana, no coincide la información con la columna 'city' lo cual hago una corrección creando un diccionario que contenga todos los departamentos con su respectiva capital. *A pesar que no todos los 32 departamentos están en el análisis lo hago con todos para clara la información y por un posible futuro donde se agreguen departamentos adicionales*

2. También elimino duplicados del dataset **estados_y_seriales_v2.xlsx** y a algunas columnas de los otros datasets cambio el 'datatype' de algunas columnas de fechas a datetime para mejor análisis y manipulación de la información.



## Análisis Realizado

- Exploración de tendencias en críticas fallidas y exitosas.
- Análisis por región y por tipo de dispositivo.
- Evaluación de tiempos de respuesta entre crítica y visita técnica.
- Visualización de patrones mediante gráficos de línea, barras y mapas.

Realizó 5 conclusiones que encuentro relevantes para la toma de decisiones. Cada conclusión tiene su cálculo realizado mediante las librerías, su respectiva gráfica y una interpretación de lo mostrado y analizado. Explico cómo lo realicé parte por parte a continuación:

1. Críticas fallidas por departamento.

Creo un df llamado 'fallas_por_departamento' tomando las críticas que fallaron ('FALSO') y uniéndolo mediante `.merge` con el serial que está el df_ubicación para realizar un conteo y ordenándolo de mayor menor.
La gráfica la realizo gracias la información extraída anteriormente y muestro por departamento el número de críticas fallidas por departamento.

También hago un análisis para comparar los registros fallidos con los exitosos tomando la columna 'desde', agrupo por la fecha y por 'subjuicio' y luego con `.size` cuento las filas que salieron en cada agrupación.
Para hacer la gráfica uso una de líneas que muestre la evolución diaria para evaluar picos y similitudes.
En ambas gráficas utilizo las librerías matplotlib y seaborn para dar personalización.

2. Número de medidores únicos que fallan por departamento.

Para este análisis creo un df que contenga solo las criticas fallidas del df_criticas y lo uno nuevamente con los departamentos del df_ubicación (esto lo realizo con el df creado anteriormente) lo agrupo y mediante `.nunique` saco el valor único de cada serial y lo ordeno de mayor a menor.
Para la gráfica con lo realizado en el paso anterior tomando los valores y los departamentos.
Para realizar la gráfica utilizo la librería seaborn y matplotlib para dar formato. También utilizo el clico for para ubicar el porcentaje encima de cada barra y tener una información más descriptiva.

3. Medidores más problemáticos (top 10 seriales con más fallas)

Este punto lo realizo en uno solo. Tomo los seriales fallidos y hago un conteo. Para graficarlo solo tomo los 10 primeros, solo utilizo la librería matplotlib e igual que el punto anterior para poner los números que hay al lado de las barras utilizo un ciclo for.

4. ¿Los medidores problemáticos recibieron visitas técnicas?

Acá nuevamente tomo los seriales que fallan y los filtro con el df_visitas que tengan esos seriales mediante `.isin`, luego contamos las visitas que tuvo cada serial y el dato lo multiplico por 1000000 (que es el valor que cuesta cada visita).
Para realizar la gráfica utilizo solo la librería matplotlib.

5. ¿Las visitas se hicieron dentro del plazo de 48 horas?

En este caso primero realizo el análisis primero filtrando los seriales que más fallaron y una vez filtrados los por la fecha. Luego tomo la fecha más antigua (`.min()`) de la columna 'desde' y le cambio el nombre a la columna por 'fecha_primera_falla'
Realizo lo mismo para obtener la fecha más antigua, pero del df_visitas y le cambio el nombre a la columna por 'fecha_primera_visita'.
Ambos dfs las combino por el serial y luego hago la diferencia de las fechas. Me va a entregar los valores en segundos entonces hago la división por 36000 y con `.round` redondeo a solo dos decimales para obtener el valor por horas.
Luego con el resultado solo lo comparo si fue menor o igual a 48 horas (<=48).
Para la gráfica utilizo matplotlib y nuevamente seaborn.




## Conclusiones Relevantes

- Se identificaron regiones con mayor índice de fallos.
- Algunos tipos de dispositivos presentan mayor tasa de errores.
- Existen demoras significativas en las visitas técnicas en ciertos casos.
- Se proponen áreas de mejora en la logística de atención.

## Herramientas Utilizadas

- Python
- Pandas, NumPy
- Matplotlib, Seaborn
- Jupyter Notebook

## Estructura del Proyecto

```
── Diego_Rodríguez.ipynb           # Notebook principal con todo el análisis
── resumen_criticas_v2.xlsx        # Dataset de críticas
── estados_y_seriales_v2.xlsx      # Dataset de estados y ubicaciones
── certificados_v3.xlsx            # Dataset de visitas técnicas
── README.md                       # Este archivo
```

##  Autor

- **Diego Rodríguez**  *correo: diego0302.dr@gmail.com*