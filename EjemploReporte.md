---
title: "Ejemplo Reporte"
author: "Daiuk"
date: "2024-03-25"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

# Carga de paquetes, datos y sumario con estadísticas descriptivas
```{r }

library(ggplot2)

## Carga de datos (supongamos que el conjunto de datos se llama "educacion_inegi.csv")
## Establecer el directorio de trabajo
#setwd("~/Documentos/cursoR_UNADM/")

## Cargar el archivo y cambiar el nombre
datos <- read.csv("educacion_inegi.csv")
## Crear un subset con las columnas necesarias
datos_filtrados <- subset(datos, select = c(FOLIO, EDAD, PB3_12_1, PB3_12_2, PB3_14, ESC))

## Renombrar las columnas
names(datos_filtrados) <- c("id", "edad", "uso_smartphone", "uso_laptop_o_notebook", "horas_dedicadas_familiar", "escolaridad_alcanzada")

## Visualizar las primeras filas del nuevo dataframe
summary(datos_filtrados)
```

# Añadir algunas gráficas


```{r, echo=FALSE}
## Gráfico de barras del nivel de escolaridad alcanzado
ggplot(datos_filtrados, aes(x = escolaridad_alcanzada)) +
  geom_bar(fill = "blue") +
  labs(title = "Escolaridad Alcanzada", x = "Escolaridad Alcanzada", y = "Frecuencia")

## Gráfico de dispersión entre edad y años de educación
ggplot(datos_filtrados, aes(x = edad, y = horas_dedicadas_familiar)) +
  geom_point() +
  labs(title = "Relación entre Años Cumplidos y Horas Dedicadas al Estudio Apoyadas por un Familiar", x = "Años Cumplidos", y = "Horas Dedicadas")
```

El parámetro 'echo = FALSE' se agregó al bloque de código para evitar la impresión del código R que generó la gráfica.

```{r, echo=FALSE}
## Prueba de correlación entre años cumplidos y horas dedicadas al estudio
cor.test(datos_filtrados$edad, datos_filtrados$horas_dedicadas_familiar)

## Prueba de correlación entre el uso de smartphone y el uso de laptop
cor.test(datos_filtrados$uso_smartphone, datos_filtrados$uso_laptop_o_notebook)

## Prueba de ANOVA para comparar las horas dedicadas al estudio entre diferentes niveles de escolaridad
modelo_anova <- aov(horas_dedicadas_familiar ~ escolaridad_alcanzada, data = datos_filtrados)
summary(modelo_anova)

```

```{r, echo=FALSE}
## Tabla de frecuencias de uso de smartphone y laptop
tabla_frecuencias <- table(datos_filtrados$uso_smartphone, datos_filtrados$uso_laptop_o_notebook)
tabla_frecuencias

## Gráfico de barras apiladas de uso de smartphone y laptop
ggplot(datos_filtrados, aes(x = uso_smartphone, fill = uso_laptop_o_notebook)) +
  geom_bar(position = "stack") +
  labs(title = "Uso de Smartphone y Laptop", x = "Uso de Smartphone", y = "Frecuencia") +
  scale_fill_manual(values = c("green", "red"))

```

