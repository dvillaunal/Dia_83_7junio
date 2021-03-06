#Libreria [e1072], Ejercicio Complementario al del Dia_82_6junio 

```{r, eval=FALSE, include=TRUE}
"Protocolo:
 
 1. Daniel Felipe Villa Rengifo
 
 2. Lenguaje: R
 
 3. Tema: Libreria [e1072], Ejercicio Complementario al del Dia_82_6junio (Continuación del tema de Clasificadores Bayesianos) y Correción de Laplace
 
 4. Fuentes:  
    https://dlegorreta.wordpress.com/tag/e1071/"
```

> Nota: Profe de este libro estare sacando más temas ya que me parecio muy interesante y facil de comprender su escritura

# e1072

Vamos a probar otro paquete que contienen a naive_Bayes el `e1071`.

Usaré los datos de supervivientes del Titanic que vienen en los datasets de R por defecto.

```{r, eval=FALSE, include=TRUE}
# Conversión de dataset a csv:
## Llamamaos a laba se datos "Titanic"
#data("Titanic")

# Convertimos en data.frame a "Titanic"
Titanic_df <- as.data.frame(Titanic)

#Exportamos el data.frame:
write.csv(Titanic_df, file = "Titanic_df.csv", row.names = F)
```

La tabla de datos tiene 32 filas pero en realidad esconde en la columna freq el número de repeticiones de cada caso, por lo que el primer paso es crear una tabla completa. 

pero antes carganremos la libreria `e1071`

```{r}
# Cargar la libreria:
#install.packages("e1071")
library(e1071)
```

ahora cargamos la base de datos:

```{r}
# importamos la base de datos
Titanic_df <- read.csv(file = "Titanic_df.csv")

#Creamos una tabla de casos competos a partir de la frecuencia de cada uno
##Esto repite cada caso según la frecuencia dada en la col de la tabla.

repeating_sequence <- rep.int(seq_len(nrow(Titanic_df)), Titanic_df$Freq)

#Creamos una nueva tabla repitiendo los casos según el modelo anterior.

Titanic_dataset <- Titanic_df[repeating_sequence,]

#Ya no necesitamos la tabla de frecuencias más del anterior data.frame

Titanic_dataset$Freq <- NULL

# Miramos nuestra nueva base de datos
head(Titanic_dataset)

# Guardamos la base de datos:
write.csv(Titanic_dataset, file = "Titanic_dataset.csv", row.names = F)

# Observemos las clases de las varaibles del data.frame:
str(Titanic_dataset)

"Todos son factores"

# Ajustamos un modelo de naive bayes con la librería `e1071`
# ESte modelo se basa apartir del vector logico, (es sobreviviente)

m.e1071 <- naiveBayes(Survived ~ ., data = Titanic_dataset)

print(m.e1071) # observemos el modelo

#-------------------------

# realizamos la prediccion con el modelo

predicciones.m <- predict(m.e1071,Titanic_dataset)

## Matriz de confusión

table(predicciones.m,Titanic_dataset$Survived)
```