# Scraping-de-informacio-n-de-productos-de-Amazon
Raspado de información de productos de Amazon con Beautiful Soup

![image](https://github.com/user-attachments/assets/8a0ef067-42e3-4ea2-88b2-6df743439647)


El web scraping es un método de extracción de datos que se utiliza para recopilar exclusivamente datos de sitios web. Es ampliamente utilizado para la minería de datos o la recopilación de información valiosa de grandes sitios web. El raspado web también es útil para uso personal. Python contiene una biblioteca increíble llamada BeautifulSoup para permitir el raspado web. Lo usaremos para raspar la información del producto y guardar los detalles en un archivo CSV.

En este artículo, se necesitan los siguientes requisitos previos.

url.txt: Un archivo de texto con algunas urls de páginas de productos de Amazon para raspar

Id del elemento: Necesitamos identificadores de los objetos que deseamos raspar en la red, lo cubriremos pronto...

Aquí, nuestro archivo de texto se ve así.


![image](https://github.com/user-attachments/assets/22814499-f3c2-4c0b-a509-aa78e611a488)



Módulo necesario e instalación:

BeautifulSoup: Nuestro módulo principal contiene un método para acceder a una página web a través de HTTP.

pip install bs4
lxml: Librería auxiliar para procesar páginas web en lenguaje python.

pip install lxml
requests: Hace que el proceso de envío de solicitudes HTTP sea impecable.

pip install requests
Importante:

Primero, vamos a importar nuestras bibliotecas requeridas.
A continuación, tomaremos la URL almacenada en nuestro archivo de texto.
Alimentaremos la URL a nuestro objeto soup, que luego extraerá información relevante de la URL
dada en función del id del elemento, le proporcionamos y la guardará en nuestro archivo CSV.
Echemos un vistazo al código, veremos qué sucede en cada paso significativo.

Paso 1: Inicializando nuestro programa.

Importamos nuestra sopa hermosa y solicitudes, creamos / abrimos un archivo CSV para guardar nuestros datos recopilados. Declaramos Header y agregamos un agente de usuario. Esto asegura que el sitio web de destino al que vamos a hacer web scrape no considere el tráfico de nuestro programa como spam y finalmente sea bloqueado por ellos. Hay muchos agentes de usuario disponibles aquí.


from bs4 import BeautifulSoup
import requests

File = open("out.csv", "a")

HEADERS = ({'User-Agent':
           'Mozilla/5.0 (X11; Linux x86_64) 
                AppleWebKit/537.36 (KHTML, like Gecko) 
                    Chrome/44.0.2403.157 Safari/537.36',
                           'Accept-Language': 'en-US, en;q=0.5'})

webpage = requests.get(URL, headers=HEADERS)
soup = BeautifulSoup(webpage.content, "lxml")




Paso 2: Recuperar los identificadores de los elementos.

Identificamos los elementos viendo las páginas web renderizadas, pero no se puede decir lo mismo de nuestro script. Para identificar nuestro elemento objetivo, tomaremos su id de elemento y lo introduciremos en el script.

Obtener el id de un elemento es bastante simple. Supongamos que necesito el id del elemento del nombre del producto, todo lo que tengo que hacer

Vaya a la URL e inspeccione el texto
En la consola, tomamos el texto junto a id=
getelementbyid-copy-2


Lo alimentamos a soup.find y convertimos la salida de la función en una cadena. Eliminamos las comas de la cadena para que no interfiera con el formato de escritura CSV try-except.


try:
        title = soup.find("span", 
                          attrs={"id": 'productTitle'})
       title_value = title.string

        title_string = title_value
                    .strip().replace(',', '')
          
except AttributeError:

        title_string = "NA"

        print("product Title = ", title_string)


        
Paso 3: Guardar la información actual en un archivo de texto

Usamos nuestro objeto de archivo y escribimos la cadena que acabamos de capturar, y terminamos la cadena con una coma "," para separar su columna cuando se interpreta en formato CSV.


File.write(f"{title_string},")

Haciendo los 2 pasos anteriores con todos los atributos que deseamos capturar de la web
, como el precio del artículo, la disponibilidad, etc.



Paso 4: Cerrar el archivo.


File.write(f"{available},\n")

# closing the file
File.close()
Mientras escribimos la última parte de la información, observe cómo agregamos "\n" para cambiar la línea. Si no lo hacemos, obtendremos toda la información requerida en una fila muy larga. Cerramos el archivo usando File.close(). Esto es necesario, si no lo hacemos podríamos obtener un error la próxima vez que abramos el archivo.



Paso 5: Llamar a la función que acabamos de crear.


if __name__ == '__main__':
  # opening our url file to access URLs
    file = open("url.txt", "r")

    # iterating over the urls
    for links in file.readlines():
        main(links)
Abrimos el url.txt en modo lectura e iteramos sobre cada una de sus líneas hasta llegar a la última. Llamando a la función principal de cada línea.


Salida:

Título del producto = Dremel DigiLab 3D40 Flex Impresora 3D con suministros adicionales 30 planes de lecciones Curso de desarrollo profesional Placa de construcción flexible Nivelación automatizada de 9 puntos PC Y MAC OS Chromebook  iPad Productos compatibles 
 precio = $1699.00 
Calificación general = 4.1 de 5 estrellas 
Reseñas totales = 40 calificaciones 
Disponibilidad = En stock. 
Título del producto = Impresora 3D Comgrow Creality Ender 3 Pro con placa de superficie de construcción extraíble y fuente de alimentación certificada por UL 220x220x250mm 
Precio del producto = NA 
Calificación general = 4.6 de 5 estrellas 
Reseñas totales = 2509 calificaciones 
Disponibilidad = NA 
Título del producto = Dremel Digilab 3D20 Creador de ideas de impresora 3D para nuevos aficionados y manitas 
Precio de los productos = $679.00 
Calificación general = 4.5 de 5 estrellas 
Reseñas totales = 584 calificaciones 
Disponibilidad = En stock. 
Título del producto = Dremel DigiLab 3D45 Impresora 3D galardonada con filamento para PC y Mac OS Chromebook Compatible con iPad Cámara HD incorporada compatible con la red Placa de construcción calefactada Nylon ECO ABS PETG PLA Capacidad 
 de impresión Productos precio = $1710.81 
Calificación general = 4.5 de 5 estrellas 
Reseñas totales = 351 calificaciones 
Disponibilidad = En stock. 

Así es como se ve nuestro out.csv.

![image](https://github.com/user-attachments/assets/f533426b-704e-4a91-b0d3-8328d3a236b0)




