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
