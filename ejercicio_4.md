## Ejercicio 4

## Inspección de Metadatos y Calidad de la Imagen. 

Los metadatos son "datos sobre los datos". En bioimagen, contienen información crítica registrada por el microscopio durante la adquisición. Acceder a ellos es fundamental para validar la calidad de la imagen, garantizar la reproducibilidad y realizar análisis cuantitativos correctos. En este ejercicio aprenderemos a extraer parámetros clave como el **objetivo utilizado**, la **magnificación** y la **apertura numérica (NA)**.

### **Opción A: Inspección Básica en Fiji / ImageJ**

1. Abra la imagen biológica de muestra que esté utilizando (por ejemplo, `organ of corti (4D stack)`).

2. Vaya a **Image \> Show Info...** (o use el atajo `Ctrl + I` / `Cmd + I`).

3. Se abrirá una ventana de texto. Desplácese por la información y busque las líneas que hagan referencia a:

   * **Magnification** (ej. 40x, 60x, 100x).

   * **Numerical Aperture** o **NA** (ej. 1.30, 1.40).

   * **Objective Name** o **Lens** (indica el tipo de objetivo, por ejemplo, Plan Apochromat, y si es de inmersión en aceite, agua o seco).

4. **Análisis:** Anote estos tres valores. Recuerde que una mayor apertura numérica (NA) permite capturar más luz y ofrece una mejor resolución espacial, lo cual es un indicador directo de la calidad y el detalle de la imagen.

### **Opción B: Uso del Plugin Bio-Formats (Para formatos propietarios)**

Cuando los microscopios guardan imágenes en formatos nativos complejos (como `.czi`, `.lif`, `.nd2`), la lectura simple puede omitir datos. Para ello se utiliza **Bio-Formats**.

Si no tenés imágenes en esos formatos, te dejamos algunos enlaces para que descargues imágenes (te sugerimos elegir las menos pesadas\!)

archivos de nikon, extensión nd2:  podés usar alguna de las imágenes que bajaste para el  ejercicio 1 [**https://zenodo.org/records/15175309**](https://zenodo.org/records/15175309) 

Archivos de leica, extensión lif  
[https://www.ebi.ac.uk/biostudies/BioImages/studies/S-BSST749](https://www.ebi.ac.uk/biostudies/BioImages/studies/S-BSST749)

archivos de zeiss,  extensión czi:  
[https://www.ebi.ac.uk/biostudies/bioimages/studies/S-BSST429](https://www.ebi.ac.uk/biostudies/bioimages/studies/S-BSST429) 	

1. Vaya a **Plugins \> Bio-Formats \> Bio-Formats Importer**.

2. Seleccione su archivo de imagen.

3. En la ventana de opciones que aparece, asegúrese de marcar la casilla **Display metadata** y haga clic en OK.

4. Se abrirán dos ventanas adicionales: una con la imagen y otra con una tabla interactiva llena de metadatos detallados (Metadata Window).

5. Utilice la barra de búsqueda en la parte inferior de la ventana de metadatos y filtre por palabras clave como `Objective`, `Magnification` o `Lens`.

6. Compare si la estructura de metadatos de un formato propietario ofrece más detalles técnicos sobre el estado del microscopio que un archivo `.tif` genérico.

### **💡 Preguntas de reflexión para los alumnos:**

* ¿Por qué es importante verificar la Apertura Numérica (NA) antes de comparar cuantitativamente la intensidad de dos imágenes distintas?

* Si la magnificación es alta (ej. 100x) pero la apertura numérica es baja (ej. 0.5), ¿cómo afectará esto a la calidad y resolución real de la bioimagen?

## Optativo: Filtrado Gaussiano matcheando PSF

Diferentes tipos de ruido requieren diferentes estrategias de filtrado. Aquí investigaremos el ruido aleatorio aditivo (Gaussiano) y su eliminación mediante un filtro de suavizado Gaussiano.

Toda adquisición de imágenes conlleva un ruido en la medición. Si uno siguió un sampleo de Nyquist al adquirir las imágenes, es decir que al menos dos píxeles entran en la PSF del microscopio, uno puede disminuir el ruido utilizando la estadística de los píxeles. Para ello podemos calcular el ancho de la PSF en píxeles usando el valor de la apertura numérica obtenida  y el tamaño de un píxel.  

Para calcular el valor teórico del ancho de la PSF, necesitamos saber la AN de la lente objetivo, y la longitud de onda de la luz emitida 𝜆 (que podemos aproximar por 500nm) y según el criterio de Rayleigh, se estima a partir de:

dPSF \=0.61 𝜆NA

Para obtener el ancho de la PSF en píxeles, sólo se debe dividir por el tamaño del píxel de la imagen:

dPSF pixeles \= dPSFtamaño pixel

> 1. Aplique un Filtro Gaussiano, **Process \> Filters \> Gaussian Blur...** con el valor de sigma calculado.

> 2. Discuta si “ve” que la imagen perdió calidad. Aunque parece que se ve menos nítido, nuestra vista es muy sensible al contraste y ver que bajó la cantidad de detalles (ruido de alta frecuencia espacial) nos hace sospechar que bajó la calidad. Sin embargo, esa información de alta frecuencia no estaba realmente ahí ya que estaba por debajo de la resolución del microscopio.
