# CienciaDatosTaller1# Taller 1: Análisis exploratorio y criterios de focalización, SECOP II (Bienes y Suministros)

## Integrantes

- Lina Marcela Muñoz
- Yair Andrade

## Objetivo

Analizar los contratos de bienes y suministros registrados en SECOP II entre 2019 y 2025 para identificar problemas de calidad de datos, entender el comportamiento general de la contratación pública en esta categoría, y proponer criterios de focalización de supervisión respaldados por evidencia estadística.

## Alcance

El análisis cubre el entendimiento inicial de los datos (dimensiones, tipos de variables, calidad), un análisis univariado de los cinco atributos más relevantes, la detección y tratamiento no destructivo de nueve problemas de calidad, y la formulación y contraste de hipótesis explícitas sobre si la modalidad de contratación (`modalidad_de_contratacion`) explica tres señales de desviación encontradas en los datos: adición de plazo, presupuesto no ejecutado, y cierre sin liquidar. El alcance se limita a esta única variable predictora categórica, no se ajustó un modelo multivariado que controle por otras variables simultáneamente (ver limitaciones en el notebook `02_estrategia_y_desarrollo.ipynb`, sección 4.4).

## Principales hallazgos

Del total de 196391 contratos analizados, el 9.51% tuvo adición de plazo, el 21.79% quedó cerrado o terminado sin ejecutar presupuesto, y el 55.31% de los contratos cerrados o terminados quedó sin liquidar. La modalidad `Contratación régimen especial (con ofertas)` se identificó como el foco de riesgo más consistente, con los porcentajes más altos tanto de no ejecución (38.64%) como de cierre sin liquidar (81.74%). La modalidad `Licitación pública` presentó un riesgo específico de adición de plazo (18.65 días en promedio) y concentra sus contratos sin liquidar en los de mayor valor. La modalidad `Mínima cuantía`, aunque concentra el 60% del volumen total de contratos, no mostró señales de riesgo diferencial y no se recomienda como prioridad de focalización. El detalle completo de estos hallazgos, las pruebas estadísticas y sus limitaciones está en el notebook `02_estrategia_y_desarrollo.ipynb`.
El informe ejecutivo con las tablas, gráficas y criterios de focalización propuestos está disponible en `docs/Informe_Ejecutivo_Taller1.docx`.

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
