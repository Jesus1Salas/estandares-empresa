# Programming Patterns

Convenciones de codificación que deben seguirse al escribir o modificar código en
este proyecto. Los ejemplos usan Python (con PySpark), pero los principios de
tipado, documentación y estructura aplican de forma general.

## 1. Tipado explícito de variables

Declara el tipo de las variables siempre que aporte claridad, especialmente en
asignaciones cuyo tipo no sea obvio a simple vista.

```python
nombre_tabla: str = "ventas_diarias"
cantidad_registros: int = 0
factor_conversion: float = 1.5
esta_activo: bool = True
```

## 2. Tipado de parámetros y retorno en funciones

Toda función debe anotar el tipo de cada parámetro y el tipo de retorno. Usa
`Optional`, `Union`/`|` y tipos genéricos cuando corresponda.

```python
from typing import Optional


def calcular_total(precios: list[float], descuento: Optional[float] = None) -> float:
    total: float = sum(precios)
    if descuento is not None:
        total -= total * descuento
    return total
```

## 3. Docstrings detallados

Cada módulo, clase y función pública lleva docstring en estilo Google: resumen,
descripción, `Args`, `Returns` y `Raises` cuando aplique.

```python
def obtener_usuario(id_usuario: int) -> dict[str, str]:
    """Recupera los datos de un usuario por su identificador.

    Args:
        id_usuario: Identificador único del usuario a consultar.

    Returns:
        Diccionario con los campos del usuario (nombre, correo, rol).

    Raises:
        ValueError: Si el identificador no existe en la base de datos.
    """
    ...
```

## 4. Inicialización de listas tipadas

Inicializa las listas con su tipo de elemento explícito.

```python
columnas: list[str] = []
montos: list[float] = [100.0, 250.5, 75.25]
filas: list[dict[str, str]] = []
```

## 5. Inicialización de diccionarios tipados

Inicializa los diccionarios anotando el tipo de clave y de valor.

```python
config: dict[str, str] = {}
conteos: dict[str, int] = {"altas": 0, "bajas": 0}
parametros: dict[str, object] = {}
```

## 6. Asignación de variables desde diccionarios

Al extraer valores de un diccionario, tipa la variable destino y usa `.get()`
con valor por defecto cuando la clave pueda no existir.

```python
ruta_origen: str = config["ruta_origen"]
particiones: int = config.get("particiones", 4)
modo_escritura: str = opciones.get("modo", "overwrite")
```

## 7. Imports organizados por categorías y configuraciones Spark

Agrupa los imports en tres bloques separados por una línea en blanco, en este
orden: (1) librería estándar, (2) terceros, (3) módulos locales del proyecto.
Coloca las configuraciones de Spark inmediatamente después de los imports.

```python
# 1. Librería estándar
import os
from datetime import datetime

# 2. Terceros
from pyspark.sql import SparkSession, DataFrame
from pyspark.sql import functions as F

# 3. Módulos locales
from utils.io import leer_parquet
from utils.log import registrar_evento

# Configuración de Spark
spark: SparkSession = (
    SparkSession.builder
    .appName("proceso_ventas")
    .config("spark.sql.shuffle.partitions", "200")
    .getOrCreate()
)
```

## 8. Uso de f-strings para interpolación

Usa siempre f-strings para construir cadenas con variables. No uses `%` ni
`str.format()` ni concatenación con `+`.

```python
nombre: str = "ventas"
anio: int = 2026
ruta: str = f"/data/{nombre}/anio={anio}/"
mensaje: str = f"Se procesaron {cantidad_registros} registros en {ruta}."
```

## 9. Uso de condicionales para el control de flujo

Prefiere condiciones claras y explícitas. Usa cláusulas de salida temprana
(guard clauses) para reducir el anidamiento.

```python
def procesar(df: DataFrame, validar: bool = True) -> DataFrame:
    if df is None:
        raise ValueError("El DataFrame de entrada es None.")

    if validar and df.count() == 0:
        return df

    return df.filter(F.col("estado") == "activo")
```

## 10. Comentarios de código comentado

El código comentado (deshabilitado temporalmente) debe indicar por qué está ahí
y, preferiblemente, un responsable o fecha. No dejes bloques comentados sin
explicación; si el código ya no sirve, elimínalo.

```python
# Deshabilitado temporalmente por incidencia INC-482 (2026-09-26, jsalas).
# Reactivar cuando se corrija el origen de datos duplicado.
# df = df.dropDuplicates(["id"])
```

## 11. Estructura general de script

Los scripts siguen este orden: docstring de módulo → imports → configuración
Spark → constantes → funciones → bloque `main` protegido por
`if __name__ == "__main__":`.

```python
"""Proceso de carga diaria de ventas.

Lee los ficheros de origen, aplica transformaciones y escribe el resultado
en la capa curada.
"""

# Imports (ver punto 7)
# Configuración Spark (ver punto 7)

# Constantes
RUTA_ORIGEN: str = "/data/raw/ventas/"
RUTA_DESTINO: str = "/data/curated/ventas/"


def transformar(df: DataFrame) -> DataFrame:
    """Aplica las reglas de negocio al DataFrame de ventas."""
    ...


def main() -> None:
    """Punto de entrada del proceso."""
    ...


if __name__ == "__main__":
    main()
```

## 12. Manejo de archivos

Usa siempre gestores de contexto (`with`) y especifica la codificación al abrir
archivos de texto. Para rutas, tipa las variables como `str`.

```python
ruta_config: str = "config/parametros.json"

with open(ruta_config, mode="r", encoding="utf-8") as archivo:
    contenido: str = archivo.read()
```

## 13. Comentarios TODO

Marca el trabajo pendiente con `# TODO:` seguido de una descripción accionable
y, cuando sea posible, un responsable o referencia de ticket.

```python
# TODO(jsalas): parametrizar el número de particiones desde config (ORV-123).
# TODO: agregar validación de esquema antes de escribir en la capa curada.
```
