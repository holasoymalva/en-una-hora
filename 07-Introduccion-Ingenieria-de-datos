# Introducción a la Ingeniería de Datos

Aprenderás los conceptos fundamentales de la Ingeniería de Datos y las herramientas más utilizadas en el campo. 📊

## Requisitos Previos
- Python instalado
- Conocimientos básicos de SQL
- Un editor de código (VS Code recomendado)
- pip (gestor de paquetes de Python)

## 1. Fundamentos y Conceptos

### ¿Qué es la Ingeniería de Datos?
- Proceso de diseñar y construir sistemas para recolectar, almacenar y analizar datos
- Puente entre datos crudos y análisis de datos
- Enfoque en la calidad, consistencia y accesibilidad de los datos

### Pipeline de Datos Típico
1. Extracción de datos (Extract)
2. Transformación de datos (Transform)
3. Carga de datos (Load)
4. Almacenamiento
5. Procesamiento y Análisis

> **Nota sobre ETL vs ELT:**
> - ETL: Transformación antes de cargar (tradicional)
> - ELT: Transformación después de cargar (moderno, cloud)
> - La elección depende de: volumen de datos, requisitos de procesamiento, costos

## 2. Herramientas y Configuración

```bash
# Instalar librerías principales
pip install pandas numpy sqlalchemy
pip install python-dotenv requests
pip install apache-airflow
```

### Estructura Típica de Proyecto
```plaintext
data_engineering_project/
├── data/
│   ├── raw/
│   ├── processed/
│   └── final/
├── scripts/
│   ├── extract.py
│   ├── transform.py
│   └── load.py
├── config/
│   └── config.yaml
├── tests/
└── README.md
```

> **Nota sobre Herramientas:**
> - Pandas: Manipulación de datos
> - SQLAlchemy: ORM y conexiones a bases de datos
> - Airflow: Orquestación de pipelines
> - Docker: Contenedorización
> - DBeaver/pgAdmin: Herramientas de BD

## 3. Extracción de Datos

```python
# extract.py
import pandas as pd
import requests
from sqlalchemy import create_engine
import os
from dotenv import load_dotenv

# Cargar variables de entorno
load_dotenv()

def extract_from_api():
    """Extraer datos de una API"""
    url = "https://api.ejemplo.com/datos"
    response = requests.get(url)
    data = response.json()
    df = pd.DataFrame(data)
    return df

def extract_from_database():
    """Extraer datos de una base de datos"""
    # Conexión a la base de datos
    db_url = os.getenv("DATABASE_URL")
    engine = create_engine(db_url)
    
    # Consulta SQL
    query = """
    SELECT *
    FROM ventas
    WHERE fecha >= CURRENT_DATE - INTERVAL '30 days'
    """
    
    df = pd.read_sql(query, engine)
    return df

def extract_from_csv():
    """Extraer datos de archivos CSV"""
    df = pd.read_csv("data/raw/datos.csv")
    return df

# Ejemplo de uso
if __name__ == "__main__":
    # Extraer datos de diferentes fuentes
    api_data = extract_from_api()
    db_data = extract_from_database()
    csv_data = extract_from_csv()
    
    # Guardar datos extraídos
    api_data.to_csv("data/raw/api_data.csv", index=False)
    db_data.to_csv("data/raw/db_data.csv", index=False)
```

> **Nota sobre Extracción:**
> - Identificar fuentes de datos
> - Manejar autenticación y credenciales
> - Considerar límites de API y tamaño de datos
> - Implementar manejo de errores
> - Documentar el proceso de extracción

## 4. Transformación de Datos

```python
# transform.py
import pandas as pd
import numpy as np

def clean_data(df):
    """Limpieza básica de datos"""
    # Eliminar duplicados
    df = df.drop_duplicates()
    
    # Manejar valores nulos
    df = df.fillna({
        'numeric_column': 0,
        'text_column': 'DESCONOCIDO'
    })
    
    # Convertir tipos de datos
    df['fecha'] = pd.to_datetime(df['fecha'])
    df['precio'] = pd.to_numeric(df['precio'], errors='coerce')
    
    return df

def transform_data(df):
    """Transformaciones de negocio"""
    # Calcular nuevas columnas
    df['total'] = df['cantidad'] * df['precio']
    df['mes'] = df['fecha'].dt.month
    
    # Categorizar datos
    df['categoria_precio'] = pd.cut(
        df['precio'],
        bins=[0, 100, 500, np.inf],
        labels=['Bajo', 'Medio', 'Alto']
    )
    
    # Agregaciones
    resumen = df.groupby('categoria_precio').agg({
        'total': 'sum',
        'cantidad': 'count'
    }).reset_index()
    
    return df, resumen

def validate_data(df):
    """Validación de datos"""
    assert df['precio'].min() >= 0, "Precios negativos encontrados"
    assert df['cantidad'].min() >= 0, "Cantidades negativas encontradas"
    assert not df['producto'].isna().any(), "Productos nulos encontrados"

# Ejemplo de uso
if __name__ == "__main__":
    # Cargar datos crudos
    df = pd.read_csv("data/raw/ventas.csv")
    
    # Proceso de transformación
    df = clean_data(df)
    df, resumen = transform_data(df)
    validate_data(df)
    
    # Guardar datos transformados
    df.to_csv("data/processed/ventas_procesadas.csv", index=False)
    resumen.to_csv("data/processed/resumen_ventas.csv", index=False)
```

> **Nota sobre Transformación:**
> - Limpieza de datos es crucial
> - Mantener consistencia en tipos de datos
> - Documentar transformaciones
> - Implementar validaciones
> - Considerar el rendimiento

## 5. Carga de Datos

```python
# load.py
from sqlalchemy import create_engine
import pandas as pd
import os
from dotenv import load_dotenv

load_dotenv()

def load_to_database(df, table_name, if_exists='replace'):
    """Cargar datos a una base de datos"""
    engine = create_engine(os.getenv("DATABASE_URL"))
    
    df.to_sql(
        table_name,
        engine,
        if_exists=if_exists,
        index=False,
        chunksize=1000
    )

def load_to_files(df, format='parquet'):
    """Cargar datos a archivos"""
    if format == 'parquet':
        df.to_parquet("data/final/datos.parquet")
    elif format == 'csv':
        df.to_csv("data/final/datos.csv", index=False)
    else:
        raise ValueError(f"Formato no soportado: {format}")

# Ejemplo de uso
if __name__ == "__main__":
    # Cargar datos procesados
    df = pd.read_csv("data/processed/ventas_procesadas.csv")
    
    # Cargar a diferentes destinos
    load_to_database(df, "ventas_final")
    load_to_files(df, format='parquet')
```

## 6. Pipeline Completo - Ejemplo Práctico

```python
# pipeline.py
import pandas as pd
from extract import extract_from_api, extract_from_database
from transform import clean_data, transform_data, validate_data
from load import load_to_database
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def run_pipeline():
    try:
        # 1. Extracción
        logger.info("Iniciando extracción de datos...")
        api_data = extract_from_api()
        db_data = extract_from_database()
        
        # Combinar datos
        df = pd.concat([api_data, db_data])
        
        # 2. Transformación
        logger.info("Iniciando transformación de datos...")
        df = clean_data(df)
        df, resumen = transform_data(df)
        validate_data(df)
        
        # 3. Carga
        logger.info("Iniciando carga de datos...")
        load_to_database(df, "datos_finales")
        load_to_database(resumen, "resumen_datos")
        
        logger.info("Pipeline completado exitosamente!")
        
    except Exception as e:
        logger.error(f"Error en pipeline: {str(e)}")
        raise

if __name__ == "__main__":
    run_pipeline()
```

## Recursos Adicionales
- [Apache Airflow Documentation](https://airflow.apache.org/docs/)
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [The Data Engineering Cookbook](https://github.com/andkret/Cookbook)
- [Modern Data Engineering with Python](https://www.manning.com/books/modern-data-engineering-with-python)

## Mejores Prácticas

1. **Datos**
   - Siempre hacer respaldo de datos crudos
   - Validar datos en cada etapa
   - Documentar transformaciones
   - Mantener versionado de datos

2. **Código**
   - Usar control de versiones (Git)
   - Escribir pruebas unitarias
   - Documentar funciones y clases
   - Seguir estándares de código (PEP 8)

3. **Pipeline**
   - Implementar logging detallado
   - Manejar errores apropiadamente
   - Monitorear rendimiento
   - Hacer el pipeline idempotente

4. **Seguridad**
   - Usar variables de entorno
   - Implementar autenticación
   - Encriptar datos sensibles
   - Seguir principio de mínimo privilegio

¡Felicitaciones! 🎉 Has completado la introducción a la Ingeniería de Datos. Este es solo el comienzo de un campo muy amplio y en constante evolución.
