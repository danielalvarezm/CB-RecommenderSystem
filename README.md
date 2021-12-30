<a name="item0"></a>
# Sistemas de recomendación. Modelos Basados en el Contenido

#### Autor: Daniel Álvarez Medina
#### Correo: alu0101216126@ull.edu.es
#### [Acceda al sistema de recomendación](https://alu0101216126.github.io/CB-RecommenderSystem/)

## Índice 

* [Introducción](#item1)
* [Directorios](#item2)
* [Instrucciones](#item3)

  * [Ejemplo de uso](#item3.1)
 
* [Descripción del código](#item4)

  * [index.html](#item4.1) 
  * [form.js](#item4.2)
  * [recommender.js](#item4.3) 

<a name="item1"></a>
## 1. Introducción :rocket:

Implementación de un sistema de recomendación siguiendo el sistema de Modelos Basados en el Contenido.

La práctica se ha llevado a cabo en el lenguaje **Javascript**. A su vez, la pedida de datos la hemos realizado en HTML y procesado posteriormente en Javascript.
Cabe destacar que para dar estilo al HTML, empleamos hojas de estilo **CSS**, y también las proporcionadas por el framework de CSS [Materialize](https://materializecss.com).

[↑](#item0)

<a name="item2"></a>
## 2. Directorios :file_folder:

Si accedemos al directorio [/docs](https://github.com/alu0101216126/CB-RecommenderSystem/tree/main/docs), encontraremos una organización como la siguiente:

* [/css](https://github.com/alu0101216126/CB-RecommenderSystem/tree/main/docs/css): Directorio que contiene las hojas de estilo empleadas. 
  * [style.css](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/css/style.css): Hoja de estilo para el archivo [index.html](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/index.html)

* [/examples](https://github.com/alu0101216126/CB-RecommenderSystem/tree/main/docs/examples): Directorio que contiene una serie de matrices de utilidad de ejemplo para introducir en el sistema de recomendación.
  * [documents-01.txt](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/examples/documents-01.txt): Fichero que contiene un documento por línea.
  * [documents-02.txt](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/examples/documents-02.txt): Fichero que contiene un documento por línea.
  * [documents-03.txt](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/examples/documents-03.txt): Fichero que contiene un documento por línea.

* [/media](https://github.com/alu0101216126/CB-RecommenderSystem/tree/main/media): Directorio que contiene imágenes y gifs empleados en el README.md.
* [/src](https://github.com/alu0101216126/CB-RecommenderSystem/tree/main/docs/src): Directorio que contiene los scripts empleados para llevar a cabo el sistema de recomendación
  * [form.js](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/src/form.js): Procesamos los datos obtenidos en el formulario, y mediante un objeto de la clase **Recommender**, obtenemos y mostramos los resultados.
  * [recommender.js](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/src/recommender.js): Contiene la clase **Recommender**, dicha clase almacena los datos obtenidos en el formulario y realiza los cálculos correspondientes para obtener la matriz de similitud coseno entre los distintos documentos, y por cada documento, obtener para cada de sus palabras: TF, IDF, TF-IDF.
* [index.html](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/index.html): Fichero **HTML** que contiene el formulario a procesar mediante el fichero [form.js](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/src/form.js)

[↑](#item0)

<a name="item3"></a>
## 3. Instrucciones :page_with_curl:

Para acceder al sistema recomendador lo haremos a través del enlace que aparece en la parte superior, o haciendo clic [aquí](https://alu0101216126.github.io/CB-RecommenderSystem/).

![Formato inicial del formulario](./media/CB-RecommenderSystem.png)

Veremos que tenemos un formulario con una serie de campos (fichero index.html):

* En primer lugar, se nos indica que seleccionemos un documento. Dicho documento lo debemos de subir en formato `.txt`, al pusar el botón de **Seleccionar arhivo**. En [/examples](https://github.com/alu0101216126/CB-RecommenderSystem/tree/main/docs/examples) podemos encontrar algunos ficheros de ejemplo. Se aprecia que cada fila representará un documento distinto.

Posteriormente, pulsaremos el botón de **GENERAR PREDICCIÓN** para mostrar los resultados.

<a name="item3.1"></a>
### 3.1 Ejemplo de uso 

Un ejemplo de ejecución sería el siguiente:

<p align="center">
 <a href="https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/media/Example.gif">
  <img src="./media/Example.gif" style="max-width:100%; width: 150%">
 </a>
</p>

[↑](#item0)

<a name="item4"></a>
## 4. Descripción del código :computer:

<a name="item4.1"></a>
### 4.1. index.html 

El fichero [index.html](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/index.html), contiene el formulario a rellenar por parte del usuario, donde posteriormente será analizado.

La información del contenido de este fichero lo puede encontrar en las [Instrucciones](#item3). 

[↑](#item0)

<a name="item4.2"></a>
### 4.2. form.js

En [form.js](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/src/form.js), procesamos los datos obtenidos en el formulario, para posteriormente almacenarlos en variables, y mediante un objeto de la clase **Recommender**, obtener y mostrar los resultados.

Para esto, primeramente, creamos un objeto de la clase Recommender que explicaremos más adelante. Lo llamaremos **recommender**:
```javascript
const recommender = new Recommender();
```
Una vez creado el objeto, podremos trabajar con los eventos para procesar y mostrar datos. Para conseguir esto, empleamos dos tipos de eventos:

* Evento `change`: Este evento se activará cada vez que el input del campo que estamos observando cambie.

```javascript
const fileInput = document.getElementById('documents');
fileInput.addEventListener('change', fileToDocuments);
```
Se puede observar en el ejemplo, que lo que haremos será obtener el campo que queremos analizar, que en este caso es el archivo .txt con los documentos a analizar. Posteriormente, cuando se active el evento, llamaremos al método `fileToDocuments`:

```javascript
    function fileToDocuments(e) {

    // Manejo de errores
    if (e.target.files.length < 1) {
      alert("Tienes que subir un fichero de documentos");
    }

    const file = fileInput.files[0];
    let reader = new FileReader();
    reader.readAsText(file);

    reader.onload= function() { 
        // Cada línea del fichero se guarda en un array
        let lines = reader.result.split('\n');

        // Formalizamos los documentos
        for (let i = 0; i < lines.length; i++) {

            if (lines[i] == '') { 
                lines.splice(i, 1); 
                continue;  
            }

            lines[i] = lines[i].trim(); // Eliminamos los caracteres en blanco al principio y al final de la línea
            lines[i] = lines[i].replace(/[^a-zA-Z0-9\s\']/g, ''); // ^ = Negación -> Eliminamos aquellos caracteres que no sean letras, números o espacios.
            lines[i] = lines[i].replace(/\s+/g, ' '); // Reemplazamos los espacios dobles por uno solo  
            lines[i] = lines[i].toLowerCase(); // Convertimos a minúsculas
            
            // Por cada línea, separamos por espacios	
            lines[i] = lines[i].split(' ');
            console.log(lines[i]);
        }

        // Asignamos los documentos a la matriz de documentos de la clase Recomendador
        recommender.setDocuments(lines);
    }

}
```
En el método `fileToDocuments`, creamos un objeto de la clase `FileReader()` que denominaremos `reader`. Una vez hecho esto, leemos el fichero recibido como texto.

Cuando se acabe de leer el fichero correctamente, se activará el evento `load`. En este evento, separamos el archivo recibido por líneas, donde cada línea representará un documento. Por cada línea: convertimos el texto a minúsculas, eliminamos caracteres que no sean alfanuméricos, eliminamos espacios innecesarios, y separamos cada línea por espacios. Finalmente, esta matriz de documentos se almacenará en el objeto recommender a través del método `setDocuments(lines)`.

* Evento `click`: Este evento se activará cada vez que pulsemos el botón del campo que estamos observando.

```javascript
const generate = document.getElementById('generate');
generate.addEventListener('click', function(e) {
```
Se puede observar en el ejemplo, que lo que haremos será obtener el campo que queremos analizar, que en este caso es el botón a pulsar. Posteriormente, cuando se active el evento, lo que haremos será realizar todos los cálculos necesarios con los valores almacenados hasta el momento, para finalmente mostrarlos.

También existen otros métodos que simplemente se encargar de mostrar mediante HTML los resultados, estos son: `showResults(documents, tf, idf, tf_idf)`, `printTables(tf, idf, tf_idf)` y `showPredictionsCalculation(array, prediction)`.

[↑](#item0)

<a name="item4.3"></a>
### 4.3. recommender.js

El fichero [recommender.js](https://github.com/alu0101216126/CB-RecommenderSystem/blob/main/docs/src/recommender.js), contiene la clase **Recommender**, dicha clase almacena los datos obtenidos en el formulario y realiza los cálculos correspondientes para obtener la matriz de similitud coseno entre los distintos documentos, y por cada documento, obtener para cada de sus palabras: TF, IDF, TF-IDF.

### **_Constructor_**

El constructor por defecto de la clase es el siguiente:

```javascript
constructor() {
        this.documents = [];
        this.TF = [];
        this.IDF = [];
        this.TFIDF = [];
        this.similarityMatrix = [];
        this.vectorLength = [];
        this.normalizedTF = [];
      }
```
Como los valores de los atributos cambian dinámicamente mediante el evento explicado `click`, todos los atributos tendrán valores por defecto hasta que estos cambien.

* `this.documents`: Representa una matriz de documentos. Cada documento es una lista de palabras.
* `this.TF`: Representa una matriz de frecuencias de cada palabra en cada documento.
* `this.IDF`: Representa una matriz de frecuencias inversas de cada palabra en cada documento.
* `this.TFIDF`: Matriz numérica que expresa cuán relevante es una palabra para un documento en una colección.
* `this.similarityMatrix`: Matriz de similitud coseno entre documentos.
* `this.vectorLength`: Vector de longitud de cada documento.
* `this.normalizedTF`: Matriz de frecuencias normalizadas de cada palabra en cada documento.

[↑](#item0)

### **_Setter_**

Mediante los setters, podemos establecer los valores de los atributos, en este caso sólo contamos con un setter:

* `setDocuments(documents)`: Establecemos la matriz de documentos.

### **_Getters_**

Asimismo, también contamos con una serie de getters que nos devuelven los valores de algunos atributos.

* `getDocuments()`: Devuelve la matriz de documentos.
* `getTF()`: Devuelve la matriz de términos más frecuentes de cada documento.
* `getIDF()`: Devuelve la matriz IDF.
* `getTFIDF()`: Devuelve la matriz TFIDF.
* `getSimilarityMatrix()`: Devuelve la matriz de similitud.
* `getVectorLength()`: Devuelve el vector de longitud de cada documento.

[↑](#item0)
