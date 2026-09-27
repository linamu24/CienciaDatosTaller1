# Taller 1: Análisis exploratorio y criterios de focalización, SECOP II (Bienes y Suministros)

## Integrantes

- Lina Marcela Muñoz
- Yair Andrade

## Objetivo

Analizar los contratos de bienes y suministros registrados en SECOP II para identificar problemas de calidad de datos, establecer qué periodo es apropiado para el análisis, entender el comportamiento general de la contratación pública en esta categoría, y proponer criterios de focalización de supervisión respaldados por evidencia estadística.

## Alcance

El análisis cubre el entendimiento inicial de los datos (dimensiones, tipos de variables, calidad), un análisis univariado de los cinco atributos más relevantes, la detección y tratamiento no destructivo de nueve problemas de calidad, y la selección justificada del periodo de análisis. El dataset trae contratos firmados entre 2019 y 2025, pero 2019 registra el ciclo de vida del contrato de otra forma y 2025 aún tiene la mayoría de sus contratos sin cerrar, por lo que el análisis se hace sobre 2020-2024 (139853 contratos).

Sobre ese periodo se formularon y contrastaron hipótesis explícitas sobre si cinco factores conocidos desde la firma del contrato (modalidad de contratación, valor, destino del gasto, sector y tipo de entidad) se asocian con tres señales de desviación: adición de plazo, presupuesto no ejecutado y cierre sin liquidar. Se estratificó el cruce entre sector y modalidad, pero no se ajustó un modelo multivariado que controle por todas las variables al mismo tiempo (ver limitaciones en el notebook `02_estrategia_y_desarrollo.ipynb`, sección 4.4).

## Principales hallazgos

En el periodo 2020-2024, el 7.90% de los contratos tuvo adición de plazo, el 42.54% de los contratos cerrados o terminados no tiene ningún pago registrado, y el 57.40% de los contratos cerrados o terminados quedó sin liquidar.

`Sector` resultó ser el predictor más fuerte (Cramér's V de 0.2939 frente a cierre sin liquidar). Sin embargo, al cruzarlo con la modalidad se encontró que el alto riesgo de `Salud y Protección Social` (73.36% sin liquidar) se explica en buena parte porque el 69.10% de sus contratos se firma por régimen especial, casi todos en hospitales y Empresas Sociales del Estado. Por eso los criterios propuestos son:

- **Modalidades de régimen especial**, foco transversal: `Contratación régimen especial (con ofertas)` tiene la tasa más alta de no ejecución (82.97%) y de cierre sin liquidar (83.94%), y el efecto se mantiene dentro y fuera del sector Salud.
- **Sectores `Ley de Justicia` y `Educación Nacional`**, con 70.73% y 68.42% sin liquidar y volumen alto. En Ley de Justicia el riesgo se concentra en Mínima cuantía.
- **Entidades territoriales** (63.74% sin liquidar frente a 52.07% en las nacionales) y **contratos de funcionamiento** (48.88% sin pagos registrados frente a 32.65% en inversión).
- **`Licitación pública`**, foco de plazos: el 23.71% de sus contratos recibió adición, frente a 6.13% en Mínima cuantía, y es la única modalidad donde los contratos sin liquidar son los de mayor valor.

También se reportan resultados que no alcanzaron relevancia práctica: la diferencia en días de adición entre modalidades (13.31 días, bajo el umbral de 15), el valor del contrato usado como criterio aislado, y el carácter centralizado o descentralizado de la entidad. `defensa` y `Servicio Público`, los sectores con más contratos, no muestran más riesgo que el promedio y no deberían priorizarse solo por su tamaño. Entre las limitaciones, parte de la no ejecución refleja falta de registro de pagos en SECOP II, y la liquidación no siempre es obligatoria para las entidades de régimen especial.

El detalle completo de estos hallazgos, las pruebas estadísticas y sus limitaciones está en el notebook `02_estrategia_y_desarrollo.ipynb` y en el informe ejecutivo, disponible en `docs/Informe_Ejecutivo_Taller1.pdf` y en su versión editable `docs/Informe_Ejecutivo_Taller1.docx`.

## Organización del repositorio

```
CienciaDatosTaller1/
├── data/
│   ├── secop_bienes.parquet              # Datos crudos originales (incluidos en el repositorio)
│   └── secop_bienes_limpio.parquet       # Datos limpios, generados por el notebook 01
├── docs/
│   ├── EnunciadoTaller1.pdf               # Enunciado del taller
│   ├── Informe_Ejecutivo_Taller1.docx     # Informe ejecutivo del Punto 4, con tablas y gráficas
│   └── Informe_Ejecutivo_Taller1.pdf      # Versión en PDF del informe ejecutivo
├── notebooks/
│   ├── 01_entendimiento_inicial.ipynb       # Punto 1: entendimiento inicial, calidad de datos y periodo de análisis
│   └── 02_estrategia_y_desarrollo.ipynb     # Puntos 2, 3 y 4: estrategia, hipótesis y resultados
├── requirements.txt                       # Dependencias con versiones probadas
└── README.md
```

## Requisitos previos: datos crudos

El archivo de datos crudos `data/secop_bienes.parquet` ya está incluido en el repositorio, así que al clonarlo no hace falta descargar nada adicional. El notebook `01_entendimiento_inicial.ipynb` lo carga desde esa ruta.

Si por alguna razón el archivo no está disponible, se puede descargar desde el enlace proporcionado por el curso y ubicarlo exactamente en `data/secop_bienes.parquet`, respetando el nombre y la ruta:

[Descargar secop_bienes.parquet](https://drive.google.com/file/d/1R0pSXh2bgCoPKcXlAlafdavVwvzZX6AQ/view?usp=sharing)

## Instrucciones de ejecución

Los notebooks deben ejecutarse en este orden estricto:

1. `notebooks/01_entendimiento_inicial.ipynb`. Carga `data/secop_bienes.parquet`, realiza el entendimiento inicial y la limpieza no destructiva de los datos, justifica el periodo de análisis 2020-2024, y al final guarda el resultado en `data/secop_bienes_limpio.parquet`. Este archivo es necesario para el siguiente notebook.
2. `notebooks/02_estrategia_y_desarrollo.ipynb`. Carga `data/secop_bienes_limpio.parquet`, generado en el paso anterior, y lo filtra al periodo 2020-2024. Luego desarrolla la estrategia de análisis, las pruebas de hipótesis, las visualizaciones multivariadas y la generación de resultados.

Ambos notebooks están diseñados para ejecutarse de principio a fin sin errores usando "Restart & Run All" en Jupyter.

## Dependencias

El proyecto se probó con Python 3.13 y las siguientes librerías, cuyas versiones exactas están en `requirements.txt`:

- pandas
- numpy
- matplotlib
- seaborn
- scipy
- pyarrow (necesaria para leer y escribir archivos `.parquet`)
- ipykernel (para ejecutar los notebooks en Jupyter o VS Code)

Se pueden instalar con:

```bash
pip install -r requirements.txt
```

## Declaración de uso de Inteligencia Artificial

Durante el desarrollo de este taller se usó Claude como asistente de apoyo, principalmente para tres cosas: 
- Proponer código en Python para algunas secciones del entendimiento inicial, la limpieza de datos y las pruebas de hipótesis.
- Sugerir posibles tecnicas estadísticas apropiadas según las hipotesis que se le planteaban (incluyendo pruebas no paramétricas como Kruskal-Wallis y Mann-Whitney U, y tamaños de efecto como eta cuadrado y Cramér's V)
- Ayudar a definir el formato y secciones que deberia tener un informe ejecutivo (el informe fue redactado sobre la plantilla que generó Claude).
  
Adicionalmente, durante la escritura del código se tuvo activo GitHub Copilot en Visual Studio Code, por lo que en algunos momentos autocompletó fragmentos de código mientras se escribía.
