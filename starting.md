# TypeError
### TypeError: cannot concatenate object of type '<class 'str'>'; only Series and DataFrame objs are valid
Este error fue causado cuando quise colocar los keys de un diccionario, (pensando que ya eran variables válidas).
```
 for d in dfDict.keys(): 
    concatList.append(d)
```


Para resolverlo, en vez de elegir *keys* elegí *values*, porque los values son ```<class 'pandas.core.frame.DataFrame'>``` t este tipo de dato puede ser usado para crear dataframes ✅
```
 for d in dfDict.values(): 
    concatList.append(d)
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
