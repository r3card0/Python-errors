# UnicodeDecodeError: 'utf-8' codec can't decode byte 0xfc in position 191: invalid start byte

Se pretende leer un archivo csv con pandas, pero se arroja el siguiente error: UnicodeDecodeError: 'utf-8' codec can't decode byte 0xfc in position 191: invalid start byte

El error UnicodeDecodeError: 'utf-8' codec can't decode byte 0xfc indica que pandas está intentando leer el archivo CSV utilizando la codificación utf-8, pero el archivo contiene caracteres que no están codificados en utf-8. El byte 0xfc corresponde a la letra "ü" en la codificación ISO-8859-1 (también conocida como latin1).

Si el archivo proviene de un entorno Windows donde se usan codificaciones especificas, se puede probar el encoding  **cp1252**, que es muy similar al de **latin1** con la diferencia de que **cp1252** maneja mas caracteres.

Al aplicar el encodign **cp1252**, pandas pudo leer el archivo CSV.
```python
df = pd.read_csv('archivo.csv', encoding='cp1252')
```

## ¿Pandas usa el encoding UTF-8 por defecto al leer un archivo CSV o con cualquier formato?
La respuesta es **SI**. Pandas utiliza el encoding UTF-8 por defecto al leer archivos CSV o cualquier otro formato que implique texto como .json, .txt, etc. Esto quiere decir que si no especifico explícitamente la codificación (encoding), pandas buscará interpretar los archivos de texto con UTF-8

## ¿Como crear una función que cuando lea un archivo csv y se presente el error : UnicodeDecodeError: 'utf-8' codec can't decode byte ANY in position ANY: invalid start byte, entonces, use la opcion "encoding='cp1252' "?
La función sugerida se presenta a continuación, aplicando el bloque *try-except*
```python
import pandas as pd

def leer_csv_con_fallback(ruta_archivo):
    try:
        # Intentar leer con codificación utf-8
        df = pd.read_csv(ruta_archivo, encoding='utf-8')
        print("Archivo leído con codificación utf-8")
    except UnicodeDecodeError:
        # Si ocurre el error, intentar con cp1252
        print("Error de codificación utf-8. Reintentando con cp1252...")
        df = pd.read_csv(ruta_archivo, encoding='cp1252')
        print("Archivo leído con codificación cp1252")
    return df

# Uso de la función
ruta_archivo = 'archivo.csv'
df = leer_csv_con_fallback(ruta_archivo)
```



# FileExistsError: [Errno 17] File exists: '/mnt/c/sql/spool/shapefiles/1000'
Se levantó el error porque el path ya existiá. Se generó en la corrida anterior.

# TypeError: chdir: path should be string, bytes, os.PathLike or integer, not NoneType
logica cuando se levanto el error:
````
# Create path to store shapefile
new_folder_name = f'{self.jira_number}'
parent_path = spool + f'/shapefiles/'
path = os.path.join(parent_path,new_folder_name)
path_in = os.mkdir(path)
os.chdir(path=path_in)
````
Se corrigió cuando cambié ````os.chdir(path=path_in)```` por ````os.chdir(path_in)````

# UnicodeDecodeError: 'utf-8' codec can't decode byte 0xf3 in position 1210: invalid continuation byte
Este error se levanto porque puse un acento de una palabra dentro del sql script. A pesar de que estaba *comentado*, se levanto el error. Se resolvió removiendo el acento y guardando nuevamente el archivo sql. Fuente consultada: [Obtengo el error "UnicodeDecodeError: 'utf-8' codec can't decode byte 0xf3 in position 30: invalid continuation byte"](https://es.stackoverflow.com/questions/382281/obtengo-el-error-unicodedecodeerror-utf-8-codec-cant-decode-byte-0xf3-in-po)

# AttributeError
### AttributeError: 'MergedCell' object attribute 'value' is read-only
Se resolvio descomentando la linea ````ws.unmerge_cells("A1:B5")````
````
# aplicar merge functions
ws.merge_cells("A1:B5")
ws.unmerge_cells("A1:B5")
ws.merge_cells(start_row=2, start_column=2,end_row=5, end_column=5)
````

### AttributeError: 'set' object has no attribute 'to_excel'
Este error fue causado porque quise menter un set a un excel. El set debo convertirlo en dataframe y después enviarlo a excel. Y si, se resolvió convirtiendo el set en dataframe
```
df = pd.DataFrame(set)
```

# TypeError
### FutureWarning: Dropping invalid columns in DataFrameGroupBy.add is deprecated. In a future version, a TypeError will be raised. Before calling .add, select only columns which should be valid for the function
Este error salió porque la columna de Total en vez de ser numerica esta como object. Es porque los valores de Febrero vienen como object. Seguramente es porque tienen coma en vez de punto para separar los decimales. Eso fue, se arreglo en el archivo fuente y ya corre bien
```orden             float64
cantidad          float64
producto           object
$ unitario        float64
Total              object
Método de Pago     object
Fecha              object
mesNum             object
mes                object
dtype: object
```

### TypeError: cannot concatenate object of type '<class 'str'>'; only Series and DataFrame objs are valid
Este error fue causado cuando quise colocar los keys de un diccionario, (pensando que ya eran variables válidas).
```
 for d in dfDict.keys(): 
    concatList.append(d)
```


**SOLUCION** Para resolverlo, en vez de elegir *keys* elegí *values*, porque los values son ```<class 'pandas.core.frame.DataFrame'>``` t este tipo de dato puede ser usado para crear dataframes ✅
```
 for d in dfDict.values(): 
    concatList.append(d)
```

### TypeError: 'Timestamp' object is not subscriptable
Este error salio porque quiero extraer una porcion de la fecha de un dataframe. La formato de la fecha es: **YYYY-MM-DD** y quiero extraer el mes: **MM**. Supongo que el error se refiere a que la fecha no puede partirse en partes. Se tiene que convertir en string.
```
for e in df_year['Fecha']:
  e[5:7]
```
**SOLUCION.** Se resolvió usando el siguiente código ```.astype(str)``` 
```
for e in df_year['Fecha'].astype(str):
  print(e[5:7])
```
Y para crear la columna que almacena el mes (numero) se hizo lo siguiente:
```
mes = []
for e in df_year['Fecha'].astype(str):
  mes.append(e[5:7])
  
df_year['mes'] = mes
```



# ValueError

### ValueError: If using all scalar values, you must pass an index

This errors appears when i run this code to create a dataframe
````
mydict = {
    'latitude': 12.34567, 'longitude': 4.5550009983,    
}

pd.DataFrame(mydict)
````

[The error message says that if you're passing scalar values, you have to pass an index. So you can either not use scalar values for the columns -- e.g. use a list](https://stackoverflow.com/questions/17839973/constructing-pandas-dataframe-from-values-in-variables-gives-valueerror-if-usi)

Solution:
````
mydict = {
    'latitude': [12.34567], 'longitude': [4.5550009983],    
}

pd.DataFrame(mydict)
````
or
````
mydict = {
    'latitude': 12.34567, 'longitude': 4.5550009983,    
}
pd.DataFrame(mydict,index=[0])
````

## ValueError: DataFrame index must be unique for orient='columns'. - exporting to a json file

Se corrigio haciendo un reset del index del Dataframe 
```python
df = df.reset_index(drop=True)
```
