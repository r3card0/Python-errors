# TypeError
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
