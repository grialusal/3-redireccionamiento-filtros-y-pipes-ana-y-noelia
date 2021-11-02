# Sesión III - Redireccionamiento, filtros y pipes

Herramientas computacionales para bioinformática: UNIX, expresiones regulares y shell script

Edita esta plantilla en formato markdown [Guía aquí](https://guides.github.com/features/mastering-markdown/) como se pide en el guión. 
Cuando hayas acabado, haz un commit de tus cambios y súbelos al repositorio antes de la fecha de entrega señalada. 

======================================


## Ejercicio 1
`sort -R` (random) desordena las líneas de un fichero. Prueba a desordenar el fichero `gene-2.bed` y crea un nuevo fichero llamado `gene-2-desordenado.bed`.

Trata ahora de ordenar este fichero de acuerdo a los siguientes criterios: 
1. Sin cortar los elementos
2. En orden descendente
3. Usando a la vez la tercera y la segunda columna (en este orden de prioridad). Consulta el manual para ver la opción -k. 

### Respuesta ejercicio 1
 Primero creamos un nuevo fichero llamado `gene-2-desordenado.bed` que contenía los datos desordenados del fichero `gene-2.bed`. Para esto usamos el comando:
 
 `sort -R gene-2.bed > gene-2-desordenado.bed`
 
![gen desordenado](https://user-images.githubusercontent.com/92113066/139421879-437f94a7-6f5f-46c6-a967-fc79c33cfdc5.png)

**1.** Para ordenar los ficheros sin cortar los elementos empleamos la orden `sort -n gene-2-desordenado.bed` que nos ordena los elementos de cada columna de menor a mayor.

![sort1](https://user-images.githubusercontent.com/92113066/139426601-6f373dca-a596-4b07-9712-eeae8d3e9d0a.png)

**2.** En este caso empleamos el comando `sort -nr gene-2-desordenado.bed` que nos ordena las columas por orden descendiente.

![sort2](https://user-images.githubusercontent.com/92113066/139427791-d7baad27-df76-42b2-9159-9a1780c45edd.png)

**3.** 

**3.** La opción `-k`, seguida del número correspondiente al campo que queremos ordenar, permite empezar a ordenar por la columna que deseemos. Para ordenar nuestro archivo fijándonos en la tercera y segunda columna, la orden a ejecutar sería: `sort -k3,3 -k2,2 -n gene-2-desordenado.bed`. Con esto, le estamos pidiendo que nos ordene "empezando y terminando" por la 3ª columna, y posteriormente, "empezando y terminando" por la segunda, razón por la que indicamos dos veces el mismo campo separado por una coma.

![orden por tercera y segunda columna](https://user-images.githubusercontent.com/92091175/139713578-cfe6435b-01d6-456d-9749-a63610fb0ba2.png)


## Ejercicio 2

Cuáles son y cuántos tipos distintos de "features" hay en `Drosophila_melanogaster.BDGP6.28.102.gtf` y en `Homo_sapiens.GRCh38.102.gtf.gz`? Nota: para trabajar con ficheros .gunzip sin descomprimir puedes usar `zcat`.

### Respuesta ejercicio 2

Primeramente, ejecutaremos `cat` sobre el archivo que nos interesa ver por pantalla para poder trabajar con él. Eliminaremos los metadatos mediante el comando `grep -v "^#"` y seleccionaremos la columna que nos interesa, la que contiene "features", mediante `cut -f3`. Ordenamos los datos con `sort` para ejecutar posteriormente `uniq -c`y conocer así los diferentes tipos de features existentes. Nuestro pipeline quedaría así: 

`cat Drosophila_melanogaster.BDGP6.28.102.gtf | grep -v "^#" | cut -f3 | sort | uniq -c`

Posteriormente, podemos ejecutar `wc -l` para contar el número de features.

![features Drosophila](https://user-images.githubusercontent.com/92091175/139582871-313cbd9b-f1f7-4e34-b38c-da09a706362b.png)

Para trabajar con el archivo `Homo_sapiens.GRCh38.102.gtf.gz` podemos utilizar el mismo pipeline, modificando `cat` por `zcat`:

`zcat Homo_sapiens.GRCh38.102.gtf.gz | grep -v "^#" | cut -f3 | sort | uniq -c`


![features homo sapiens](https://user-images.githubusercontent.com/92091175/139583070-67c8e6fb-38fe-4338-962b-46d1b8a98473.png)



## Ejercicio 3

Recuerdas `covid-samples.fasta`? Localízalo en tu HOME, y extrae, usando un pipeline, los nombres de las secuencias contenidas en este fichero. Luego saca la primera palabra de cada una, ordénalas y guárdalas en un fichero `covid-seq-names.txt`.

### Respuesta ejercicio 3



## Ejercicio 4

Encuentra, usando una sola línea, el número de usuarias diferentes que tienen al menos una carpeta a su nombre en el '/home' de CPG3.

### Respuesta ejercicio 4

Para obtener la respuesta utilizando una sola línea hay que hacer uso de pipelines. Nos situamos en '/home' y ejecutamos `ls -1` para que nos de una lista, en una sola columna, de los directos existentes. Posteriormente, añadimos `sort` y `uniq -c` para que nos cuente si hubiera algún directorio con un nombre repetido, y `wc -l` para saber el número de directorios existentes.

`ls -1 | sort | uniq -c | wc -l`

Nos aparecen 38 directorios, pero no se distinguen cuáles pertenecen a los usuarios.

