# Predicción del Precio de Coches de Segunda Mano en España

## El problema

Llevo más de 10 años trabajando en el sector de la automoción. En todo ese tiempo he visto una y otra vez el mismo problema: nadie sabe con certeza si el precio de un anuncio de segunda mano es justo o no. Ni el comprador ni el vendedor tienen una referencia objetiva.

Este proyecto construye un modelo de Machine Learning que, dadas las características de un coche, predice su precio de mercado en España. Los datos vienen de anuncios reales de AutoScout24 — no son datos sintéticos ni de laboratorio.

## Dataset

- **Fuente:** Kaggle — [EU Used Cars](https://www.kaggle.com/datasets/alemazz11/cars-europe)
- **Origen:** Scraping de AutoScout24, el mayor portal de venta de coches de Europa
- **Tamaño original:** 40.494 anuncios, 25 variables
- **Países:** Alemania, Italia, Francia, España, Bélgica, Holanda, Austria y Luxemburgo
- **Variable objetivo:** Precio en euros

## Estrategia

El proyecto tiene dos fases:

1. **EDA amplio sobre toda Europa** — para entender cómo funciona el mercado europeo de segunda mano, comparar países y marcas, y verificar hipótesis.
2. **Modelo ML enfocado en España** — porque cada mercado tiene su propia lógica de precios y mezclar países introduciría ruido innecesario.

## Hallazgos principales del EDA

- **La potencia es el factor que más predice el precio** — correlación de 0.73. El comprador mira los caballos, no los cilindros.
- **El kilometraje deprecia más que la antigüedad** — los coches clásicos rompen la lógica de "más viejo, más barato".
- **España está por debajo de la media europea** — Francia y Luxemburgo lideran en precio medio.
- **CUPRA lidera el precio medio por marca**, seguida de Volvo y Jaguar. BMW y Audi mantienen el valor incluso con muchos kilómetros. Fiat y Opel se deprecian más rápido.
- **El diesel ha caído por debajo de la gasolina** en precio medio — reflejo de su pérdida de valor en el mercado europeo actual.

## Resultados del modelo

| Métrica | Valor |
|---------|-------|
| Modelo | RandomForest |
| Registros España | 1.373 |
| MAE validación cruzada | 4.662 € |
| MAE test | 4.344 € |
| RMSE test | 7.878 € |
| R² test | 0.850 |
| Baseline (media) | 13.769 € |

El modelo explica el 85% de la variabilidad del precio y reduce el error medio de 13.769€ (baseline) a 4.344€. Funciona bien en el segmento 0-40.000€, que es donde se concentra la masa del mercado español. En gama alta tiende a subestimar por escasez de datos.

**¿Para qué sirve en la vida real?**
- Concesionarios que necesitan tasar coches rápidamente antes de una negociación
- Portales de segunda mano para detectar anuncios con precio anómalo
- Empresas de remarketing que valoran flotas enteras de forma automatizada

## Mejoras identificadas

**1. Ampliar el dataset español**
Con solo 1.373 registros el modelo tiene poco donde aprender, especialmente en gama alta. Haciendo scraping de Coches.net o Wallapop se conseguiría mucho más volumen y representaría mejor el mercado real español.

**2. Recuperar el modelo del coche con Target Encoding**
Tuve que descartar el modelo del coche porque había 268 valores únicos y eso generaba demasiadas columnas. Con Target Encoding — sustituir cada modelo por su precio medio histórico — se podría incluir sin ese problema. Un Golf GTI no vale lo mismo que un Golf base.

**3. Antigüedad del anuncio como variable predictiva**
Si un coche lleva 6 meses en el anuncio sin venderse, algo pasa. Es una señal de mercado real que este dataset no recoge pero que se podría obtener haciendo scraping con fecha de publicación.

## Pipeline técnico

1. Descarga del dataset via Kaggle API (`kagglehub`)
2. Limpieza: outliers imposibles, missings, duplicados — pérdida del 5% de registros
3. EDA completo: univariante, bivariante, multivariante, verificación de hipótesis
4. Split train/test 80/20 sobre registros españoles
5. Preprocesado: encoding de categóricas (`get_dummies`), escalado de numéricas (`StandardScaler`)
6. Comparativa de 5 modelos con validación cruzada — ganó RandomForest
7. Optimización de hiperparámetros con GridSearchCV (81 combinaciones)
8. Evaluación final contra test
9. Modelo guardado en `src/models/rf_usedcars_spain.pkl`

## Estructura del repositorio

    ├── main.ipynb          # Notebook final, ejecutable de principio a fin
    ├── Presentacion.pdf    # Diapositivas del proyecto
    ├── README.md
    └── src/
        ├── data_sample/    # Muestra del dataset (1.000 registros)
        ├── img/            # Gráficas exportadas
        ├── models/         # Modelo guardado en pickle
        ├── notebooks/      # Notebooks de desarrollo
        └── utils/          # Funciones auxiliares

## Instalación y uso

    git clone https://github.com/azooles/ML_UsedCars_Europe
    cd ML_UsedCars_Europe
    pip install -r requirements.txt

Configurar Kaggle API en `~/.config/kaggle/kaggle.json` y ejecutar `main.ipynb` de principio a fin.

## Tecnologías

Python · pandas · numpy · matplotlib · seaborn · scikit-learn · kagglehub
