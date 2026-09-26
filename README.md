# CienciaDatosTaller1# Taller 1: Análisis exploratorio y criterios de focalización, SECOP II (Bienes y Suministros)

## Integrantes

- Lina Marcela Muñoz
- Yair Andrade

## Objetivo

Analizar los contratos de bienes y suministros registrados en SECOP II entre 2019 y 2025 para identificar problemas de calidad de datos, entender el comportamiento general de la contratación pública en esta categoría, y proponer criterios de focalización de supervisión respaldados por evidencia estadística.

## Alcance

El análisis cubre el entendimiento inicial de los datos (dimensiones, tipos de variables, calidad), un análisis univariado de los cinco atributos más relevantes, la detección y tratamiento no destructivo de nueve problemas de calidad, y la formulación y contraste de hipótesis explícitas sobre si la modalidad de contratación (`modalidad_de_contratacion`) explica tres señales de desviación encontradas en los datos: adición de plazo, presupuesto no ejecutado, y cierre sin liquidar. El alcance se limita a esta única variable predictora categórica, no se ajustó un modelo multivariado que controle por otras variables simultáneamente (ver limitaciones en el notebook `02_estrategia_y_desarrollo.ipynb`, sección 4.4).

## Principales hallazgos

Del total de 196391 contratos analizados, el 9.51% tuvo adición de plazo, el 21.79% quedó cerrado o terminado sin ejecutar presupuesto, y el 55.31% de los contratos cerrados o terminados quedó sin liquidar. Se evaluaron cuatro factores frente a estas tres señales: modalidad de contratación, valor del contrato, destino del gasto, y sector.

`Sector` resultó ser el predictor más fuerte de todo el análisis (Cramér's V de 0.2815 frente a cierre sin liquidar). Los sectores `Ley de Justicia` (8676 contratos, 71.35% sin liquidar) y `Salud y Protección Social` (11097 contratos, 70.67% sin liquidar) son los hallazgos más sólidos y accionables, por combinar una tasa alta con un volumen de contratos grande. En contraste, `defensa` y `Servicio Público`, los dos sectores de mayor volumen, no muestran riesgo diferencial y no deberían priorizarse solo por su tamaño.

La modalidad `Contratación régimen especial (con ofertas)` se mantiene como un foco transversal, con el porcentaje más alto de no ejecución (38.64%) y de cierre sin liquidar (81.74%) entre todas las modalidades. `Licitación pública` es un foco distinto, no destaca en esas dos señales, pero sí presenta un riesgo específico de adición de plazo (18.65 días en promedio) y concentra sus contratos sin liquidar en los de mayor valor. El valor del contrato y el destino del gasto, evaluados de forma independiente, resultaron ser predictores más débiles, el valor del contrato depende de con qué otra variable se combine, y el destino del gasto solo mostró una señal relevante en los registros con destino sin definir, más como una alerta de calidad de datos que como un criterio de focalización.

El detalle completo de estos hallazgos, las pruebas estadísticas y sus limitaciones está en el notebook `02_estrategia_y_desarrollo.ipynb` y en el informe ejecutivo `docs/Informe_Ejecutivo_Taller1.docx`.

## Organización del repositorio

```
CienciaDatosTaller1/
├── data/
│   ├── secop_bienes.parquet              # Datos crudos originales (descargar, ver sección siguiente)
│   └── secop_bienes_limpio.parquet       # Datos limpios, generados por el notebook 01
├── docs/
│   ├── EnunciadoTaller1.pdf               # Enunciado del taller
│   └── Informe_Ejecutivo_Taller1.docx     # Informe ejecutivo del Punto 4, con tablas y gráficas
├── notebooks/
│   ├── 01_entendimiento_inicial.ipynb       # Punto 1: entendimiento inicial y calidad de datos
│   └── 02_estrategia_y_desarrollo.ipynb     # Puntos 2, 3 y 4: estrategia, hipótesis y resultados
└── README.md
```

## Requisitos previos: datos crudos

Antes de ejecutar los notebooks es necesario tener el archivo de datos crudos `secop_bienes.parquet` dentro de la carpeta `data/`. Este archivo no se genera con el código, se descarga desde el enlace proporcionado por el curso:

[Descargar secop_bienes.parquet](https://drive.google.com/file/d/1R0pSXh2bgCoPKcXlAlafdavVwvzZX6AQ/view?usp=sharing)

Una vez descargado, ubícalo exactamente en `data/secop_bienes.parquet` (respetando el nombre y la ruta), ya que el notebook `01_entendimiento_inicial.ipynb` lo carga desde ahí al inicio. Sin este archivo, ningún notebook del repositorio podrá ejecutarse.

## Instrucciones de ejecución

Los notebooks deben ejecutarse en este orden estricto:

1. `notebooks/01_entendimiento_inicial.ipynb`. Carga `data/secop_bienes.parquet`, realiza el entendimiento inicial y la limpieza no destructiva de los datos, y al final guarda el resultado en `data/secop_bienes_limpio.parquet`. Este archivo es necesario para el siguiente notebook.
2. `notebooks/02_estrategia_y_desarrollo.ipynb`. Carga `data/secop_bienes_limpio.parquet` (generado en el paso anterior) y desarrolla la estrategia de análisis, las pruebas de hipótesis, las visualizaciones multivariadas y el informe ejecutivo de resultados.

Ambos notebooks están diseñados para ejecutarse de principio a fin sin errores usando "Restart & Run All" en Jupyter.

## Dependencias

El proyecto usa Python 3 con las siguientes librerías:

- pandas
- numpy
- matplotlib
- seaborn
- scipy
- pyarrow (necesaria para leer y escribir archivos `.parquet`)

Se pueden instalar con:

```bash
pip install pandas numpy matplotlib seaborn scipy pyarrow
```
## Declaración de uso de Inteligencia Artificial

Durante el desarrollo de este taller se usó Claude como asistente de apoyo, principalmente para tres cosas: 
- Proponer código en Python para algunas secciones del entendimiento inicial, la limpieza de datos y las pruebas de hipótesis.
- Sugerir posibles tecnicas estadísticas apropiadas según las hipotesis que se le planteaban (incluyendo pruebas no paramétricas como Kruskal-Wallis y Mann-Whitney U, y tamaños de efecto como eta cuadrado y Cramér's V)
- Ayudar a definir el formato y secciones que deberia tener un informe ejecutivo (el informe fue redactado sobre la plantilla que generó Claude).
  
Adicionalmente, durante la escritura del código se tuvo activo GitHub Copilot en Visual Studio Code, por lo que en algunos momentos autocompletó fragmentos de código mientras se escribía.
