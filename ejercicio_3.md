# Ejercicio 3

# Simulación de imágenes con ruido y técnicas de filtrado digital: filtros de mediana y gaussianos 

Se explorará operaciones fundamentales de procesamiento de imágenes, incluyendo la comprensión de la profundidad de bits, la creación de imágenes, los cálculos aritméticos de imágenes y las técnicas de filtrado espacial (filtros de mediana y gaussianos).

## Paso 1: Abrir e Inspeccionar una Imagen

Antes de realizar cálculos o filtrados, conocer características de la imagen.

1. Abra su software de análisis de bioimágenes (por ejemplo, Fiji / ImageJ).

2. Vaya a **File \> Open Samples** y seleccione una imagen de muestra de Fiji/ImageJ por ejemplo, **Hela cells.tif** o  **Fluorescent Cells.tif** .

3. Diríjase a **Image \> Type** para inspeccionar la profundidad de bits de la imagen (por ejemplo, 8 bits, 16 bits o 32 bits). Note el rango de valores de intensidad de píxeles asociados con cada profundidad de bits.	

4. Determine el número de dimensiones de la imagen y de canales. Si usa la imagen hela-cells.tiff deberá separar los canales (**Image \> Stack \> Stack to Images**) para trabajar con un solo canal a la vez.

## Paso 2: Crear una Imagen Nueva y Añadir Ruido

Para comprender cómo funcionan los filtros y los cálculos, simularemos la generación de ruido en una imagen en blanco.

1. Cree una nueva imagen en blanco seleccionando **File \> New \> Image**. Ajuste las dimensiones para que coincidan con su imagen original y seleccione un tipo de escala de grises de 8 o 16 bit.  (Si desea aplicar ruido Salt and Pepper, considere que se aplica únicamente a imágenes de 8 bit).

2. Añada ruido de tipo sal y pimienta a la imagen en blanco utilizando las herramientas de generación de ruido (por ejemplo, **Process \> Noise \> Salt and Pepper**).

3. Observe cómo se distribuyen los píxeles extremos aislados (puntos blancos y negros) a lo largo de la imagen.

## Paso 3: Uso de la Calculadora de Imágenes

Los cálculos de imágenes permiten realizar operaciones matemáticas píxel por píxel entre dos imágenes de las mismas dimensiones.

1. Abra la herramienta Calculadora de Imágenes (por ejemplo, **Process \> Image Calculator**).

2. Seleccione su imagen biológica original como Imagen 1 y la imagen con ruido creada en el Ejercicio 2 como Imagen 2\.

3. Elija la operación **Add** (Sumar) y marque la opción **Create new window** (Crear nueva ventana).

4. Analice cómo el ruido añadido altera los detalles estructurales y la distribución de la intensidad de los píxeles de la imagen original.

5. Se sugiere duplicar la imagen con ruido para poder aplicar diferentes filtros y comparar  (**Image \> Duplicate** ).

## Paso 4: Eliminación del Ruido Sal y Pimienta con un Filtro de Mediana

El ruido de sal y pimienta introduce valores atípicos definidos y localizados que pueden confundir el análisis cuantitativo. Los filtros no lineales como el filtro de mediana son ideales para esto.

1. Seleccione la imagen ruidosa resultante del paso P3.

2. Vaya a **Process \> Filters \> Median...**

3. Estudie diferentes radios y active la opción preview para poder ver el resultado en vivo.

4. Aplique otros filtros a la imagen con ruido y compare los resultados. 

El ruido de tipo "Salt and Pepper" altera la imagen asignando valores extremos (blanco o negro puro) a los píxeles afectados. El filtro de mediana contrarresta esto reemplazando cada píxel con el valor intermedio de su vecindad, lo que le permite remover eficazmente estos puntos extremos mientras preserva la nitidez de los bordes. Por el contrario, los filtros de Promedio y Gaussiano emplean operaciones lineales (promedios simples o ponderados) que integran estos valores extremos en el cálculo final, provocando que los puntos de ruido se difuminen en lugar de desaparecer.

Nota: noten que el ruido se puede aplicar directamente a la imagen de la célula. Para fines didácticos, es recomendable crear el ruido en una imagen en blanco por separado. De esta manera, podemos estudiar el comportamiento del ruido de forma aislada y, además, practicar cómo sumar y restar matrices utilizando la calculadora de imágenes. 

## Opcional: Filtrado Gaussiano 

Diferentes tipos de ruido requieren diferentes estrategias de filtrado. Aquí investigaremos el ruido aleatorio aditivo (Gaussiano) y su eliminación mediante un filtro de suavizado Gaussiano.

1. Cree una nueva imagen en blanco y añada ruido gaussiano (aleatorio) (por ejemplo, a través de **Process \> Noise \> Add Specified Noise...** con una desviación estándar de 25).

2. Use la Calculadora de Imágenes para añadir esta imagen de ruido aleatorio a su imagen original. Duplique la imagen con ruido  (**Image \> Duplicate** ).

3. Aplique un Filtro Gaussiano (por ejemplo, **Process \> Filters \> Gaussian Blur...**) con un valor de sigma de 1.5.

4. Aplique otros filtros y compare. 

Compare el rendimiento de suavizado del filtro gaussiano frente al filtro de mediana al manejar ruido aleatorio.

El análisis comparativo de este ejercicio demuestra que la elección de un filtro digital debe estar estrictamente alineada con la naturaleza física y matemática del ruido presente en la imagen.  