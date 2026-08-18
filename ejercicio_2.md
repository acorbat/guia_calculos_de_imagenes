## **Ejercicio 2**

## **Operaciones entre imágenes:  Corrección de Ruido de Oscuridad, Fondo y Homogeneidad de Iluminación** 

Realizar operaciones entre imágenes empleando el *Image Calculator* o el *Image Expression Parser*, para eliminar artefactos de la adquisición. Evitar la pérdida de datos por truncamiento numérico (*clipping*). Corrección de fondo mediante algoritmo *Rolling Ball*.

Las imágenes que usaremos en este ejercicio pueden descargarlas del siguiente enlace:

[https://drive.google.com/drive/folders/1YCWuxRbj2xVzBzhJlBEOPt0eEn1J2tI4?usp=drive\_link](https://drive.google.com/drive/folders/1YCWuxRbj2xVzBzhJlBEOPt0eEn1J2tI4?usp=drive_link) 

Descargue estas 3 imágenes:

1. `imagen_celulas.tif` (Imagen de fluorescencia de la muestra).  
   2. `ruido_oscuridad.tif` (Imagen obtenida sin iluminar a la muestra).  
   3. `fluorescent_slide.tif` ( imagen de fluorescencia del portaobjeto de plástico fluorescente obtenido con el mismo objetivo, canal y configuración de la cámara que la `imagen_celulas.tif`).

---

## PASO 1\. Abrir archivos 

* Abra el software **Fiji**.  
* Vaya a **File \> Open...** y cargue las siguientes dos imágenes de 16 bits:  
  1. `imagen_celulas.tif` (Imagen de fluorescencia de la muestra).  
  2. `ruido_oscuridad.tif` (Imagen control obtenida sin iluminar a la muestra, para caracterizar el ruido de oscuridad)  
  3. `fluorescent_slide.tif` ( imagen control de un portaobjeto de plástico fluorescente obtenido con el mismo objetivo, canal y configuración de la cámara que la `imagen_celulas.tif`).

⚠️ Nota técnica sobre imágenes de 16 bits

Las imágenes de 16 bits poseen un rango dinámico de **0 a 65,535 niveles de gris**. A diferencia de los formatos de punto flotante (32 bits), los números enteros de 16 bits **no admiten valores negativos**. Si al restar el ruido el resultado de un píxel es menor que 0, el software lo convertirá automáticamente en 0, perdiendo información cuantitativa.  Este fenómeno se conoce como *clipping* o truncamiento.

**Opciones de análisis según las imágenes que se dispongan:** 

* Si sólo dispone de la imagen de la muestra y no tuviese ninguna imagen control, podría mejorar la imagen realizando los pasos 2 y/o 3\.   
* Si además contara con la imagen control de ruido de oscuridad podría realizar el paso 3 y luego seguir con el 2 o el 3\.  
* Si tiene las dos imágenes control puede realizar las correcciones del paso 5\.  

## PASO 2\. Caracterización y corrección del Ruido de Fondo de la imagen (Fondo Constante)

Una forma sencilla de caracterizar el ruido de fondo de la imagen es obtener la intensidad promedio de una región de la imagen sin muestra (en este caso una región que no contenga ninguna célula). Luego el valor de intensidad de fondo se resta a toda la imagen.

Nota: si la muestra no tuviera una región de fondo (como podría pasar en la imagen de una monocapa celular o de un tejido) se puede usar el algoritmo de Rolling Ball mencionado en el punto 4\. para corregir inhomogeneidades de la iluminación. 

Pasos a seguir:

* Seleccione la herramienta **Rectangle** u **Oval**.

* Dibuje una región de interés (**ROI**) en una zona de `imagen_celula.tif` donde **no haya células**.

* Presione **Ctrl \+ M** (**Analyze \> Measure**). Anote el valor de la intensidad promedio (**Mean**).

* Mueva el ROI a otras 3 regiones vacías y repita la medición (**Ctrl \+ M**). Si los valores de **Mean** son muy similares entre sí, el fondo es constante y se resta el valor escalar a todos los píxeles de la imagen. Si la intensidad de fondo varía, es preferible usar el método Rolling Ball de la sección **4\.**

* **IMPORTANTE: antes de realizar la resta del valor de fondo a la imagen,  debe pasar la imagen a 32 bit\!**

  **Image \> Type \> 32 bit**

Si no se pasa la imagen a 32bit, al realizar la resta de un número a todos los píxeles de la imagen, qlos píxeles que contengan valores negativos, serán considerados con valor 0 (notar que las escalas 8 bit y 16 bit admiten enteros positivos\!). 

* Vaya a **Process \> Math \> Subtract…  .** Introduzca el valor promedio de la intensidad de fondo. Haga clic en **OK**. *(Nota: Como su imagen ahora es de 32 bits gracias al paso anterior, no hay riesgo de truncamiento en los píxeles)*.

## PASO 3\. Sustracción de Fondos Variables: **Algoritmo Rolling Ball**

Cuando la señal de fondo no es constante en la imagen o  la iluminación no es uniforme, restar un único valor numérico no es la mejor opción. En estos casos podemos usar el algoritmo de "bola rodante" (*Rolling Ball*).

* Seleccione la imagen `imagen_celula.tif`.  
* Vaya a **Process \> Subtract Background...**  
* Configure los siguientes parámetros clave:

Parámetros de "Subtract Background":

* **Rolling ball radius:** Define el radio de una esfera virtual que rueda bajo el mapa de relieve de la imagen. Debe ser **mayor** que el diámetro del objeto de interés más grande del campo de visión (por ejemplo, si la célula mide 60 píxeles, configure un radio de 80 píxeles).  
  Con la herramienta **Straight Line** (Línea) se puede medir el diámetro o ancho del objeto más grande de tu imagen (por ejemplo, una célula o un cúmulo). El valor del radio debe ser al menos tan grande como ese objeto.   
* **Light background:**	 marcarlo si tu imagen tiene un fondo brillante (blanco o claro) y objetos oscuros. Dejarlo desmarcado si el fondo es oscuro y las señales/objetos son brillantes.  
* **Create background (don't subtract):** Si se activa, muestra la matriz del fondo calculado en lugar de restarlo. Es muy didáctico para verificar qué está interpretando el software como "ruido".  
* **Sliding paraboloid:** Cambia la esfera perfecta por una forma de paraboloide. Trata los bordes de forma más suave y reduce la aparición de líneas o artefactos extraños en los límites de los objetos.  
* **Disable smoothing:** Evita que el software aplique un filtro de suavizado inicial (promedio de 3x3 píxeles) antes de calcular el fondo. Se desmarca por defecto para reducir el ruido.  
* Active la casilla **Preview** para comprobar el resultado en tiempo real y presione **OK**.

## PASO 4\. Corregir el Ruido de Oscuridad del sistema de detección

NOTA: por una cuestión de tiempo, durante la práctica del taller les sugerimos pasar directamente al Paso 5 y realizar la corrección completa de la imagen.

Para corregir el ruido de oscuridad en la imagen de la muestra, se debe restar este valor píxel a píxel; en otras palabras, se realiza una resta entre ambas imágenes . Para ello se puede elegir entre dos herramientas:

**Image Calculator**

Limitado a operar solo **dos imágenes a la vez**.  Solo permite operaciones aritméticas y lógicas estándar (sumar, restar, multiplicar, dividir, AND, OR).

**Image Expression Parser**

Puede procesar **múltiples imágenes simultáneamente** (A, B, C, D...). Soporta funciones matemáticas complejas (sin, cos, log, exp). Permite ecuaciones complejas y avanzadas en un solo paso, por ej. : (A \- B) / (C \- B)

Opción A: Utilizando "Image Calculator"

1. Vaya a **Process \> Image Calculator...**  
2. Configure los parámetros:  
   * **Image1:** `imagen_celulas.tif`  
   * **Operation:** `Subtract`  
   * **Image2:** `ruido_oscuridad.tif`  
3. Marque la casilla **Create new window**.  
4. ⚠️ **Muy importante:** Marque la casilla **32-bit (float) result**. Esto evitará que los píxeles con valores negativos se trunquen a cero, preservando la matemática real de la imagen.  
5. Haga clic en **OK** y renombre el resultado como `celula_corregida.tif`.

Opción B: Utilizando "Image Expression Parser"

1. Vaya a **Process \> Image Expression Parser**.  
2. En la lista de imágenes, asigne variables simples a sus archivos (por ejemplo, marque `imagen_celulas.tif` como **A** y `ruido_oscuridad.tif` como **B**).  
3. En el cuadro de texto **Expression**, escriba la operación matemática básica: `A - B`.  
4. Haga clic en **PARSE**  y renombre el resultado como `celula_corregida.tif`. Note que la imagen obtenida es de 32 bit.

---

## PASO 5\. Corrección de Iluminación No Uniforme y del Ruido de oscuridad

Cuando la iluminación del microscopio no es perfectamente homogénea, para una corrección más rigurosa, se debe adquirir una imagen de una muestra de fluorescencia uniforme de referencia, como por ejemplo un portaobjeto de plástico fluorescente **(fluorescent**  **slide)**. En QUAREP-LiMi recomiendan seguir [éste protocolo](https://www.protocols.io/view/monitoring-field-homogeneity-5qpvojbebg4o/v1?step=1).

Este protocolo realiza una corrección píxel a píxel utilizando tres imágenes:

1. `imagen_celulas.tif`   
2. `ruido_oscuridad.tif`   
3. `fluorescent_slide.tif` ( imagen de fluorescencia del portaobjeto de plástico fluorescente obtenido con el mismo objetivo, canal y configuración de la cámara que la `imagen_celulas.tif`).

📐 La Fórmula Matemática

Para corregir simultáneamente el ruido de oscuridad del sensor y la inhomogeneidad en la iluminación, aplicamos la siguiente ecuación en cada píxel:

Imagen corregida=`imagen celulas` \- ruido  oscuridad fluorescent  slide \- ruido oscuridad factor de escala

*Nota: El "Factor de Escala" (usualmente el promedio de la imagen fluorescent\_slide corregida) se utiliza para devolver los valores resultantes al rango dinámico original de la imagen.*

Debido a que esta operación requiere procesar tres imágenes en una sola ecuación matemática, el **Image Expression Parser** es la herramienta más eficiente para este fin. Notar que para calcular el Factor de escala se puede usar el Image Calculator. 

Pasos a seguir:

1. Abra las tres imágenes en Fiji: `imagen_celulas.tif`, `ruido_oscuridad.tif` y `fluorescent_slide.tif`.

2. Primero, necesitamos averiguar el **Factor de Escala** (la intensidad promedio del Chroma libre de ruido):

   * Vaya a **Process \> Image Calculator...** y reste: `fluorescent_slide.tif` **Minus** `ruido_oscuridad.tif` (active la opción *32-bit float result*).

   * En la imagen resultante,  presione **Ctrl \+ A** para seleccionar toda la imagen y luego presione **Ctrl \+ M** (**Analyze \> Measure**) para abrir la tabla con los resultados de las mediciones y anote el valor de la intensidad media (**Mean**). Supongamos para este ejemplo que el valor es `12500`.

3. Vaya a **Process \> Image Expression Parser**.

   Asigne las variables en la lista de imágenes:

   * `imagen_celulas.tif` → **A**  
   * `ruido_oscuridad.tif` → **B**  
   * `fluorescent_slide.tif` → **C**

4. En el cuadro **Expression**, escriba exactamente la siguiente fórmula (reemplazando `12500` por el valor real medido en el paso 2):  
   `((A - B) / (C - B)) * 12500`

5. Haga clic en **PARSE** y guarde la imagen final corregida. 

Comentario: Para definir la escala espacial de las imágenes, puede usar la imagen 	**grilla\_patron.tif** que corresponde a la imagen de transmisión de una grilla patrón (10μm mínima división). 
