ML — Predicción del Precio de Coches de Segunda Mano en Europa
Descripción
Proyecto de Machine Learning para predecir el precio de coches de segunda mano en el mercado español, a partir de un dataset de anuncios reales de AutoScout24 con datos de varios países europeos.
El proyecto incluye un EDA completo sobre el mercado europeo y un modelo de regresión enfocado en España.
Dataset

Fuente: Kaggle — EU Used Cars
Origen: Anuncios reales de AutoScout24
Tamaño original: 40.494 registros, 25 columnas
Países: Alemania, Italia, Francia, España, Bélgica, Holanda, Austria, Luxemburgo

Problema de negocio
Predecir el precio de un coche de segunda mano en el mercado español a partir de sus características técnicas y comerciales. La métrica principal es el MAE (error medio absoluto en euros).
Resultados
MétricaValorModeloRandomForestMAE test4.344 €RMSE test7.878 €R² test0.850Baseline (media)13.769 €
Estructura del repositorio
├── main.ipynb          # Notebook final con el pipeline completo
├── Presentacion.pdf    # Diapositivas de la exposición
├── README.md           
└── src/
    ├── data_sample/    # Muestra del dataset
    ├── img/            # Gráficos exportados
    ├── models/         # Modelo guardado (pickle)
    ├── notebooks/      # Notebooks de desarrollo
    └── utils/          # Funciones auxiliares
Requisitos
pip install -r requirements.txt
Uso

Clonar el repositorio
Instalar dependencias: pip install -r requirements.txt
Configurar Kaggle API: ~/.config/kaggle/kaggle.json
Ejecutar main.ipynb de principio a fin

Tecnologías
Python, pandas, numpy, matplotlib, seaborn, scikit-learn, kagglehub