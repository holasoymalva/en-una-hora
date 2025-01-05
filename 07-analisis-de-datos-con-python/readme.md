# Análisis de Datos con Python - Guía de 1 Hora

Aprenderás los fundamentos del análisis de datos usando Python y sus principales bibliotecas en solo 1 hora! 🚀

## Requisitos Previos
- Python 3.x instalado
- Conocimientos básicos de Python
- Un editor de código (VS Code o Jupyter Notebook recomendado)

## 1. Instalación de Bibliotecas (2 minutos)
```bash
pip install pandas numpy matplotlib seaborn
```

## 2. Pandas Básico (15 minutos)

```python
import pandas as pd
import numpy as np

# Crear un DataFrame
datos = {
    'nombre': ['Ana', 'Juan', 'María', 'Carlos'],
    'edad': [25, 30, 22, 28],
    'ciudad': ['Madrid', 'Barcelona', 'Sevilla', 'Valencia'],
    'salario': [30000, 45000, 28000, 35000]
}

df = pd.DataFrame(datos)

# Lectura de archivos
# df = pd.read_csv('datos.csv')
# df = pd.read_excel('datos.xlsx')

# Exploración básica
print(df.head())        # Primeras 5 filas
print(df.info())        # Información del DataFrame
print(df.describe())    # Estadísticas descriptivas
print(df.columns)       # Nombres de columnas

# Selección de datos
nombres = df['nombre']              # Seleccionar una columna
subset = df[['nombre', 'edad']]     # Múltiples columnas
filtro = df[df['edad'] > 25]        # Filtrar datos

# Operaciones básicas
df['bonus'] = df['salario'] * 0.1   # Crear nueva columna
df['edad'].mean()                   # Promedio
df.groupby('ciudad')['salario'].mean()  # Agrupar y calcular
```

## 3. NumPy Básico (10 minutos)

```python
import numpy as np

# Crear arrays
arr = np.array([1, 2, 3, 4, 5])
matriz = np.array([[1, 2, 3], [4, 5, 6]])

# Arrays especiales
zeros = np.zeros(5)                 # Array de ceros
ones = np.ones((3, 3))             # Matriz de unos
rango = np.arange(0, 10, 2)        # Array con rango
aleatorio = np.random.rand(5)       # Números aleatorios

# Operaciones
suma = arr + 2                      # Operaciones elemento por elemento
multiplicacion = arr * 2
raiz = np.sqrt(arr)                # Funciones matemáticas
media = np.mean(arr)               # Estadísticas
```

## 4. Visualización con Matplotlib (15 minutos)

```python
import matplotlib.pyplot as plt

# Datos de ejemplo
x = np.linspace(0, 10, 100)
y = np.sin(x)

# Gráfico de líneas
plt.figure(figsize=(10, 6))
plt.plot(x, y, 'b-', label='Seno')
plt.title('Función Seno')
plt.xlabel('X')
plt.ylabel('Y')
plt.legend()
plt.grid(True)
plt.show()

# Gráfico de barras con datos del DataFrame
plt.figure(figsize=(10, 6))
df['salario'].plot(kind='bar')
plt.title('Salarios por Empleado')
plt.xlabel('Empleados')
plt.ylabel('Salario')
plt.show()

# Gráfico de dispersión
plt.figure(figsize=(10, 6))
plt.scatter(df['edad'], df['salario'])
plt.title('Salario vs Edad')
plt.xlabel('Edad')
plt.ylabel('Salario')
plt.show()
```

## 5. Visualización con Seaborn (10 minutos)

```python
import seaborn as sns

# Configuración del estilo
sns.set_style("whitegrid")

# Gráfico de dispersión mejorado
plt.figure(figsize=(10, 6))
sns.scatterplot(data=df, x='edad', y='salario', hue='ciudad')
plt.title('Salario vs Edad por Ciudad')
plt.show()

# Box plot
plt.figure(figsize=(10, 6))
sns.boxplot(data=df, x='ciudad', y='salario')
plt.title('Distribución de Salarios por Ciudad')
plt.show()

# Heatmap de correlación
plt.figure(figsize=(8, 6))
sns.heatmap(df.corr(), annot=True, cmap='coolwarm')
plt.title('Matriz de Correlación')
plt.show()
```

## 6. Análisis de Datos Práctico (8 minutos)

```python
# Ejemplo con un dataset real
df = pd.DataFrame({
    'fecha': pd.date_range('2024-01-01', periods=100),
    'ventas': np.random.randint(100, 1000, 100),
    'categoria': np.random.choice(['A', 'B', 'C'], 100),
    'region': np.random.choice(['Norte', 'Sur', 'Este', 'Oeste'], 100)
})

# Análisis temporal
ventas_mensuales = df.set_index('fecha')['ventas'].resample('M').mean()

# Análisis por categoría
ventas_categoria = df.groupby('categoria')['ventas'].agg(['mean', 'sum', 'count'])

# Análisis por región
ventas_region = df.groupby('region')['ventas'].mean().sort_values(ascending=False)

# Visualización combinada
fig, axes = plt.subplots(2, 2, figsize=(15, 10))

# Gráfico de línea temporal
ventas_mensuales.plot(ax=axes[0,0])
axes[0,0].set_title('Ventas Mensuales')

# Gráfico de barras por categoría
ventas_categoria['sum'].plot(kind='bar', ax=axes[0,1])
axes[0,1].set_title('Ventas por Categoría')

# Box plot por región
sns.boxplot(data=df, x='region', y='ventas', ax=axes[1,0])
axes[1,0].set_title('Distribución de Ventas por Región')

# Gráfico de violín
sns.violinplot(data=df, x='categoria', y='ventas', ax=axes[1,1])
axes[1,1].set_title('Distribución de Ventas por Categoría')

plt.tight_layout()
plt.show()
```

## Ejercicios Prácticos

1. Cargar un dataset (puedes usar datasets de ejemplo de seaborn)
```python
# Cargar dataset de ejemplo
titanic = sns.load_dataset('titanic')
```

2. Realizar análisis exploratorio básico
```python
# Exploración
print(titanic.info())
print(titanic.describe())
print(titanic.isnull().sum())  # Verificar valores nulos
```

3. Crear visualizaciones informativas
```python
# Supervivencia por clase
sns.catplot(data=titanic, x='class', y='survived', kind='bar')
plt.title('Tasa de Supervivencia por Clase')
plt.show()
```

## Recursos Adicionales
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [Matplotlib Gallery](https://matplotlib.org/stable/gallery/index.html)
- [Seaborn Tutorial](https://seaborn.pydata.org/tutorial.html)
- [Kaggle Datasets](https://www.kaggle.com/datasets)
- [Google Colab](https://colab.research.google.com/) - Entorno gratuito para análisis de datos

¡Felicitaciones! 🎉 Has completado el tutorial de análisis de datos con Python. Ahora tienes las bases necesarias para comenzar a analizar datos utilizando las principales bibliotecas de Python.
