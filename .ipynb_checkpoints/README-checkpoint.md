# Datathon_ML_Henry

# Trabajo Final Henry Machine Learning DataFT06
Se nos pide predecir si el precio de una vivienda podria ser bajo (por debajo de 1000 se considera bajo el resto no lo es) a para ellos vamos a tener un conjunto de información que
debemos usar para lograr esta predicción. 
Nos dan dos tablas:
  1- train.parquet
  2- test.parquet
  
Ambas tablas contienen información real acerca de datos relativos a las viviendas y en el caso de train.parquet tiene también la columna de precio.

En el fichero etl.ipynb realicé el trabajo de limpieza de datos

 ## 1-Abro train.parquet
   #### despues de train.info() tenemos:
   #### RangeIndex: 346479 entries, 0 to 346478
   #### Data columns (total 22 columns):
   #### dtypes: float64(3), int64(10), object(9)
   #### memory usage: 58.2+ MB
 
 ## 2- Vamos a mejorar esos 58.2+ MB
 Realizandole cambios a los tipos de datos podemos ahorrar mucho espacion en la memoria y en el disco, esto nos va a ahorrar tiempo en la ejecucion de acciones sobre los datos.
 Los cambios los realizo al unisono en las dos tablas para mantener consistencia en los datos.
 
 Despues de los primeros cambios
 
 ## Hasta ahora tenemos
   #### RangeIndex: 346479 entries, 0 to 346478
   #### Data columns (total 22 columns):
   #### dtypes: category(3), float16(1), float64(2), int16(1), int64(2), int8(7), object(6)
   #### memory usage: 33.4+ MB
   
 ## 3- Busco los nulos
   #### laundry_options             71171
   #### parking_options            126682
   #### lat                          1722
   #### long                         1722   

 ## 4- Tratamiento a nulos

 ## 4.1- laundry_options
 Voy a llevar a numeros esta columna manteniendo el principio de:
  'no laundry on site' = 0
  'w/d in unit'        = 1
  'w/d hookups'        = 2 
  'laundry on site'    = 3
  'laundry in bldg'    = 4
  None                 = 0
  
  Aparte de resolver el problema de los nulos ya voy codificando la columna para poder usarla en algún modelo. Considero los nulos como 'no laundry on site'
  
  
  
  ## 4.2- parking_options
  Voy a llevar a numeros esta columna manteniendo el principio de:
    'no parking'         = 0
    'detached garage'    = 1
    'carport'            = 2
    'off-street parking' = 3
    'attached garage'    = 4
    'street parking'     = 5 
    'valet parking'      = 6  
    None                 = 0
        
    Aparte de resolver el problema de los nulos ya voy codificando la columna para poder usarla en algún modelo. Considero los nulos como 'no parking' 
    
    
    
  ## 4.3- lat y long no los voy a usar
  lat y long  me brindan informacion de ubicacion geoespacial que en definitiva se irian a convertir en una región, ciudad, etc. Nadie va a entender que las propiedades
  que esten en la latitud = x y la longitud = y  son baratas o caras. Si no hubiese ningun otro tipo de información acerca de donde se encuentran las viviendas ahí si me quedaria con
  esta información para convertirla en información entendible. Pero es que ya me brindan el estado ('state') y la región ('region') donde está ubicada la vivienda, por lo que considero la 
  ubicación geoespacial como redundante.
  En este punto aprovecho para eliminar columnas que no me aportan nada porque no tienen nada que ver con el precio de una vivienda:
  'id','url','region_url','image_url','description'
  El caso de 'description' voy a eliminarla porque no me daba tiempo a analizarla, pero se puede reastrear a ver si en la descripción aparece de forma repetitiva palabras o frases que
  puedan convertirse en una columna nueva que incida sobre el precio de la vivienda. ('amplia','vista al mar',etc)
  
  ## 5- Continúo con la codificación de la información
  
  La cantidad de Regiones, Estados y Tipos de viviendas es lo suficientemente grande como para pensar en guardarlas en un fichero, en caso de un futuro desarrollo de alguna aplicación 
  para automatizar esta tarea.
  ## 5.1- Creo un clasificador de regiones
    
  ## 5.2- Creo un clasificador de estados
  
  ## 5.3- Creo un Clasificador de types
  
  
  ## 6- Crear columna category_price
  Esta es la columna objetivo para hacer la consulta si la categoría de una vivienda es considerada barata ('low') o no.
  Para esto me auxilio de la columna price que es la que tiene la información real de cuanto costo la vivienda.
  # 6.1- Primero elilimino los precios sin sentido
  Existen unas cuantas filas que tienen valores sin sentido para el precio de una vivienda y se eliminaron.
  ### 6.2- Elimino columna price
  Ya no la necesito, ya la codifque en la columna 'category_price'
  
  
  ## 7- Resumen
   ### Empezamos con
     dtypes: float64(3), int64(10), object(9)
     memory usage: 58.2+ MB

   ### Terminamos con
     dtypes: category(3), float16(3), int16(1), int64(2), int8(9), object(4)
     memory usage: 12.8 MB
     
  Un ahorro importante que se va a hacer notar a la hora de ejecutar modelos y que puede permitir cargas de información mucho más grande en un futuro.   

    


 En el fichero Data_Analisis.ipyn realicé el machine learning.

 ## 1- Abro train
 
 ## 2- Preparando variables para entrenar el modelo
  #### X: train sin la columna 'category_price'
  #### y: columna 'category_price' de train

  #### X_train: porciento de X para entrenamiento
  #### X_test: porciento de X para test
  #### y_train: porciento de y para entrenamiento
  #### y_test: porciento de y para test
  
 ## 3- DecisionTreeClassifier
 Voy a usar el modelo de Arbol de Decisión.
 
 ## 4- Ejecuto y Verifico las métricas
 Feature importance:  [0.21270675 0.02420957 0.28545198 0.02989283 0.03357738 0.01117422
                       0.0117397  0.01892919 0.0090337  0.00155472 0.01009935 0.1008366
                       0.04001176 0.21078225]
 Accuracy:  0.9221026762389457
 Recall:  0.9155109298190566
 F1 Score:  0.9157695406086857
 
 ## 5- Matriz Confusion
 Ejecuto la matriz de confusión.
 
 
 ## 6- Ejecuto los datos del test
 
 ## 7- Salvo la respuesta
 