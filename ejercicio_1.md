## Ejercicio 1

## Parte 1: Control de Calidad y Rango Dinámico (Identificación de Píxeles Saturados y Sin Señal)

En esta práctica, utilizaremos tablas de consulta (LUTs) e histogramas para identificar problemas críticos de exposición como la saturación y la subexposición,  asegurando que las imágenes sean válidas para su posterior análisis.

Atención: la presencia de píxeles con valores saturados o nulos es un defecto que no se puede corregir mediante el análisis de imágenes. 

Se propone usar las imágenes publicadas en [https://zenodo.org/records/15374754](https://zenodo.org/records/15374754)

Descargar las imágenes: 

* **sequential.nd2:**  imagen de dos canales obtenida en forma secuencial y en condiciones óptimas que llamaremos muestra\_optima  
* **seq\_underexposed.nd2**  que llamaremos muestra\_subexpuesta  
* **seq\_saturated.nd2**        muestra\_saturada

**Paso 1: Inspección visual de los valores de intensidad de la imagen mediante LUT**

1. Abre las tres imágenes de prueba en Fiji (por ejemplo: `muestra_subexpuesta.tif`, `muestra_saturada.tif` y `muestra_optima.tif`).

2. Para cada una de las imágenes, ve a **Image \> Lookup Tables** y selecciona **HiLo**.

   La tabla de color (LUT) **HiLo** mostrará la imagen en escala de grises, pero pintará automáticamente de azul los píxeles que valgan exactamente 0 (sin señal) y de rojo puro los píxeles que alcancen el valor máximo del rango dinámico (saturados). 

3. Observar las 3 imágenes en la escala HiLo y evaluar la presencia de píxeles sin señal o saturados. 

**Comentario:** Además, el LUT **HiLo**, es muy útil para ajustar el contraste en forma precisa. Permite cambiar los valores de brillo y contraste sin saturar la imagen por accidente. 

Como sugerencia ajuste en forma manual el brillo y contraste (pruebe cambiar valor mínimo y máximo, brillo y contraste)  de la imagen  `muestra_optima.tif` empleando el LUT HiLO.

**Paso 2: Evaluación mediante el histograma de intensidad de los píxeles de una imagen.** 

El **histograma** es la herramienta gráfica fundamental para realizar el control de calidad de una imagen científica. Permite diagnosticar de un vistazo la exposición, el rango dinámico y la validez de los datos antes de realizar cualquier análisis. 

1. Haz clic en la primera imagen (`muestra_optima.tif`) y presiona `Ctrl + H` (o ve a **Analyze \> Histogram**). Este paso te permitirá observar el histograma de la intensidad de todos los píxeles de la imagen. Notar que entre otra información, podemos ver el valor mínimo y el máximo de intensidad.

2. Repetir el proceso con la imagen subexpuesta (`muestra_subexpuesta.tif`) y  la imagen saturada (`muestra_saturada.tif`).

3. Observar cada histograma y compararlos, en particular prestar atención a sus valores extremos. 

Si el histograma presenta píxeles con intensidad 0, significa que se ha perdido la información de esos píxeles por falta de señal. Por otro lado, si existen píxeles con el valor máximo de la escala según la profundidad de bits (por ejemplo, 255 en 8 bits o 65 535 en 16 bits), entonces esas zonas están saturadas y tampoco ofrecen información cuantitativa confiable.  

## PARTE 2: Manejo de brillo, contraste y ecualización de histograma

Para el análisis de bioimágenes es fundamental distinguir qué operaciones modifican los valores de intensidad de los píxeles de aquellas que solo alteran su visualización. El siguiente ejercicio está pensado para utilizar las herramientas de contraste de forma segura.

1. Abrir la imágen **M51.tif** de ImageJ/Fiji. 

   **File  \> Open Samples** buscar la imagen  **M51.tif** 

   ¿Qué se ve en la imagen? Inspeccionar su histograma (**Analyze \> Histogram** ).

    Duplicar la imagen  (**Image \> Duplicate** ).

2. Dibujar una línea en la imagen y ver el perfil de intensidades (**Analyze \> Plot profile**). Habilitar la opción «Live» del Plot profile y mover la línea para analizar diferentes partes de la imagen.

3. Manipular el brillo y contraste **(Image \> Adjust \> Brightness/Contrast.**..) de la visualización de la imagen para descubrir regiones no visibles de la imagen y elegir la «mejor imagen» a su criterio.

   1. ¿Qué se muestra en la región superior del cuadro de diálogo de B\&C?

   2. La operación que se realiza es una transformación lineal representada por la recta que se muestra. Cuando se mueven los parámetros (minimum, maximum, brightness, contrast) se mueve la línea. ¿Qué representan estos parámetros de la transformación lineal?

4. Duplicar la imagen original y aplicar Enhance contrast (**Process \> Enhance contrast** (sin las opciones Normalize ni Equalize). Varíe el porcentaje de píxeles saturados (típicamente se usa 0.3% o 0.4%). 

5. Duplicar la imagen original y aplicar la normalización a la imagen original (**Process \> Enhance contrast**  opción **Normalize**)

6.  Duplicar la imagen original y aplicar la ecualización de histograma a la imagen original(**Process \> Enhance contrast** opción **Equalize**)

7. Comparar los histogramas de la imagen original y las imágenes obtenidas en las partes 3, 4,  5 y 6\.

8. ¿Cuál de los procedimientos anteriores (3, 4, 5 y 6\) es reproducible?

Adicional:  Compare los histogramas de una imagen a la cual se le manipuló el brillo y contraste SIN poner **APPLY** con el de la misma imagen pero poniendo **APPLY**. 