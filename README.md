

# webscarpt


### Beautiful Soup



Como se observó en esta unidad, **BeautifulSoup** toma el  contenido de la página web y lo interpreta generando una estructura más  manejable. Si bien nos enfocaremos en las etiquetas principales, vale la pena mencionar los tipos de objetos producidos:

- **Tag**: es el objeto que nos interesa principalmente y corresponde a las etiquetas en el documento de HTML. ([documentación](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#tag))
- **NavigableString**: esta cadena de texto representa el contenido dentro de una etiqueta.  Tiene capacidades extras relacionadas con la navegación dentro de la  estructura. ([documentación](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#navigablestring))
- **BeautifulSoup**: este objeto representa el documento HTML interpretado como una  estructura jerárquica. Para fines convencionales, puede ser considerado  como objeto de tipo tag, soportando los mismos tipos de métodos. ([documentación](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#beautifulsoup))
- **Comment**: este objeto es un tipo especial de NavigatableString, que se representa con un formato especial al momento de impresión.  

En resumen, se ha descrito de forma general el funcionamiento del módulo de BeautifulSoup para, en conjunto de la herramiento de  peticiones requests, interpretar la estructura de un documento HTML. En  las siguientes unidades se expandirá la información sobre las funciones, métodos y atributos relacionados con la extracción de los datos de  interés. Sin embargo, existen propiedades y capacidades que pueden  resultar interesantes como elementos auxiliares en el árbol generado, o  notaciones abreviadas que sirven para crear un código más compacto.

------

*Referencias*:

​       

- Reitz, K. (2020). *Requests: HTTP for Humans.*
  https://docs.python-requests.org/en/master/
- Richardson, L. (2020). *Beautiful Soup Documentation.* 
  https://www.crummy.com/software/BeautifulSoup/bs4/doc/



### Iterando sobre los elementos en **Beautiful Soup**



Hasta el momento se han visto dos principales maneras de cómo se  puede utilizar el árbol jerárquico producido por BeautifulSoup con base a una página web: utilizando métodos de búsqueda, y navegando manualmente por los elementos, cada uno con sus ventajas y desventajas. Sin  embargo, podemos utilizar un enfoque intermedio entre estos dos extremos y es el de utilizar métodos de iteración sobre los elementos. De esta  forma podemos tener la ventaja de la flexibilidad de la búsqueda, pero  con el poder de ajustar la selección de los elementos de forma manual.

Para lograr este objetivo, BeautifulSoup cuenta con diferentes herramientas y métodos,siendo uno de ellos el atributo “**descendants**”. Su comportamiento es similar al de “**children**” pero no solo se limita a los elementos inmediatos, si no que enlista  todos los elementos, es decir, los hijos, los hijos de los hijos, etc.

```python
# Ejemplo 1
import requests
from bs4 import BeautifulSoup


page = requests.get(“https://datolok.github.io/python-edx/
webscrapping/simple.html”)
soup = BeautifulSoup(page.content, ‘html.parser’)


for element in soup.descendants:
     print("Element {}: \n {}".format(item, element))
```

En este código se carga la página que se ha utilizado de ejemplo y se imprimen todos los elementos contenidos por medio de un ciclo for. Esto se puede combinar con otros conceptos, como la de expresiones  regulares, para identificar palabras claves o patrones de información.

Este proceso de búsqueda dentro de un elemento en particular puede  aplicarse al resultado de otros procesos como el de búsqueda o  navegación.

```python
# Ejemplo 2
import requests
from bs4 import BeautifulSoup


page = requests.get(“https://datolok.github.io/python-edx/
webscrapping/simple.html”)
soup = BeautifulSoup(page.content, ‘html.parser’)


for p in soup.find_all(“p”):
    if “contenido” in p.get_text():
        print(p.get_text())
```

En este código se encuentran todos los elementos de tipo párrafo  <p> en el documento, se iteran sobre todos los resultados y se  imprimen aquellos que contengan la palabra “contenido”. Como se puede  observar esto abre un abanico de nuevas posibilidades para automatizar  nuestro proceso de búsqueda y no depender demasiado en que la estructura de la página web no varíe.

Igualmente, no estamos limitados a solamente definir nuestro criterio de selección en la iteración con el contenido del texto, también  podemos acceder a los atributos de los elementos utilizando el método  “get()”.

```python
# Ejemplo 3
import requests
from bs4 import BeautifulSoup


page = requests.get(“https://datolok.github.io/python-edx/
webscrapping/simple.html”)
soup = BeautifulSoup(page.content, ‘html.parser’)


for p in soup.find_all(“p”):
    if p.get("class") and "outer-text" in p.get("class")
        print(p.get(“class”),p.get_text())
```















```python
'''
http://127.0.0.1:8080/songs/all


import requests
cancion={
    'Artista':'dfasfd',
    'Nombre':'jojo'
}

respuesta=requests.get('http://127.0.0.1:8080/songs/all')
print('\n',respuesta.content.decode())

respuesta=requests.post('http://127.0.0.1:8080/songs/new',json=cancion)
print('\n',respuesta.content.decode())

respuesta=requests.get('http://127.0.0.1:8080/songs/all')
print('\n',respuesta.content.decode())

respuesta=requests.patch('http://127.0.0.1:8080/songs/2', json={'Artista':'jijiji'})
print('\n',respuesta.content.decode())

respuesta=requests.get('http://127.0.0.1:8080/songs/all')
print('\n',respuesta.content.decode())


'''


# Ejemplo API
from flask import Flask, request, jsonify
import json

app = Flask(__name__)

song_db = [
    {
    "id": 0,
     "Artista": "Air Supply",
     "Nombre": "Making Love Out of Nothing at All"
     },
     {
     "id": 1,
     "Artista": "Bonnie Tyle",
     "Nombre": "Total Eclipse Of The Heart"
     },
     {
     "id": 2,
     "Artista": "George Michael",
     "Nombre": "Careless Whisper"
     },
     {
     "id": 3,
     "Artista": "Berlin",
     "Nombre": "Take My Breath Away"
     },
     {
     "id": 4,
     "Artista": "Queen",
     "Nombre": "Bohemian Rhapsody"
     }
 ]


# GET | returns the whole catalog
@app.route("/songs/all", methods=["GET"])
def get_all():
    return jsonify(song_db), 200

# GET | returns an entry by an id


@app.route("/songs/<int:song_id>", methods=["GET"])
def get_song(song_id):
    song_result = next(
        (song for song in song_db if song["id"] == song_id), None)

    if song_result:
        return (jsonify(song_result), 200)
    else:
        return "404 Not Found", 404
# GET | returns all songs with the same name


@app.route("/songs/search/<string:song_name>", methods=["GET"])
def search_song(song_name):
    songs = [song for song in song_db if song_name in song["Nombre"]]

    if songs:
        return (jsonify(songs), 200)
    else:
        return "404 Not Found", 404

# POST | creates a new entry in the catalog


@app.route("/songs/new", methods=["POST"])
def post_song():

    if request.data:
        song_data = json.loads(request.data)

        song_id = max([song['id'] for song in song_db])+1
        song_artist = song_data["Artista"]
        song_name = song_data["Nombre"]

        new_song = {
            "id": song_id,
            "Artista": song_artist,
            "Nombre": song_name
  	  }

        song_db.append(new_song)

        return jsonify(new_song), 200
    else:
        return "400 Bad Request", 400

# DELETE | deletes an entry in the catalog by an id


@app.route("/songs/<int:song_id>", methods=["DELETE"])
def delete_song(song_id):

    for idx, song in enumerate(song_db):
        if song["id"] == song_id:
            song_db.pop(idx)

            return "Register Deleted", 200

    return "Not Found", 404

# PUT | replaces an entry in the catalog, creates a new entry if it doesn't exist


@app.route("/songs/<int:song_id>", methods=["PUT"])
def put_song(song_id):
    if request.data:
         song_data = json.loads(request.data)

         song_artist = song_data["Artista"]
         song_name = song_data["Nombre"]

         edit_song = {
             "id": song_id,
             "Artista": song_artist,
             "Nombre": song_name
     	}

         for idx, song in enumerate(song_db):
             if song["id"] == song_id:
                 song_db[idx] = edit_song

                 return (jsonify(edit_song), 200)

         edit_song["id"] = max([song['id'] for song in song_db])+1
         song_db.append(edit_song)

         return (jsonify(edit_song), 200)
    else:
         return "Bad Request", 400
 
  
# PATCH | edits an entry in the catalog, fails if it doesn't exist
@app.route("/songs/<int:song_id>", methods = ["PATCH"])
def patch_song(song_id):
    if request.data:
        for idx, song in enumerate(song_db):
            if song["id"] == song_id:
                song_data = json.loads(request.data)
               
                for edit_key in song_data:
                	song[edit_key] = song_data[edit_key]
               
                return (jsonify(song), 200)
    	
        return "Not Found", 404
    else:
        return "Bad Request", 400

if __name__=='__main__':
    app.run(debug=True, port=8080)



```







# servidor



## links



- [Introduction to Python: Absolute Beginner](https://www.edx.org/es/course/introduction-to-python-absolute-beginner) (edX).
- [Introducción al desarrollo de aplicaciones web](https://learning.edx.org/course/course-v1:UAMx+WebApp+1T2019a/)(edx) ** de aqui es la documentacion

- [Curso de Python Básico Gratis](https://codigofacilito.com/cursos/Python) (Web Código Facilito).



## venv



requirements.txt

```
flask
```



```bash
pk@pk:~$ type vvvv
vvvv () 
{ 
    deactivate;
    python3.8 -m venv ./.venv;
    source ./.venv/bin/activate;
    pip install -r requirements.txt;
    python app.py
}
pk@pk:~$ 
pk@pk:~$ 

```



## flask

app.py

```python
import sys

from flask import Flask,request
app = Flask(__name__)


@app.route('/')
def index():
    user_agent = request.headers.get('User-Agent')
    return '<p>Your browser is %s</p>' % user_agent

@app.route('/processLogin', methods=['POST'])
def process_login():
       missing = []
       fields = ['email', 'passwd', 'login_submit']
       for field in fields:
              value = request.form.get(field, None)
              if value is None:
                  missing.append(field)
       if missing:
              return "Warning: Some fields are missing"

       return '<!DOCTYPE html> ' \
           '<html lang="es">' \
           '<head>' \
           '<link href="static/css/my-socnet-style.css" rel="stylesheet" type="text/css"/>' \
           '<title> Home - SocNet </title>' \
           '</head>' \
           '<body> <div id ="container">' \
           '<a href="/"> SocNet </a> | <a href="home"> Home </a> | <a href="login"> Log In </a> | <a href="signup"> Sign Up </a>' \
           '<h1>Data from Form: Login</h1>' \
           '<form><label>email: ' + request.form['email'] + \
           '</label><br><label>passwd: ' + request.form['passwd'] + \
           '</label></form></div></body>' \
           '</html>'


if __name__ == '__main__':
    if sys.platform == 'ubuntu':  
        app.run(debug=True, port=8080)
    else:
        app.run(debug=True, port=80)
```

index.html

```

```





## json



Las estructuras de JSON tienen una traducción directa a estructuras Python:

- Los objetos JSON son representados como diccionarios en Python
- Los arrays JSON son representados como listas en Python
- true y false en JSON son valores del tipo *boolean* en Python
- Las cadenas en JSON son cadenas en Python
- Los números en JSON son *float* o *integer*, según corresponda, en Python

Por este motivo, las funciones para leer y escribir ficheros JSON son muy sencillas de utilizar. En ambos casos recurrimos a la biblioteca  JSON, así que lo primero que debemos hacer es:

```
import json
```

### Lectura

La función para leer un fichero JSON es json.load. Como parámetro un fichero, así que una forma normal de usarla es:

```
with open(“nombre_fichero.json”, 'r') as f:
   data = json.load(f)
```

Después de ejecutar esto, la variable *data* contendrá un  diccionario con los datos cargados del fichero. Si por ejemplo  hubiéramos cargado el fichero con los datos del usuario *James*  visto en la sección anterior, ahora data[‘user_name’] sería “James” y  data[‘messages’] sería una lista Python con los mensajes publicados por  James.

### Escritura

Volcar una estructura Python a un fichero JSON es igual de sencillo.  Supongamos que tenemos la siguiente inicialización de la variable datos:

```
datos = {  "user_name": "James",  "password": “007”,  "messages": [(1532648502.113984, “mensaje 1”), (1532648642.729385,   “mensaje 1”)],   "email": session['email'],  "friends": session['friends']}
```

Entonces guardar los datos en el fichero correspondiente sería:

```
with open(“james.bond@mi6.uk”, 'w') as f:  json.dump(datos, f)
```







## sesiones



Ya hemos dicho en varias  oportunidades que el protocolo HTTP no tiene estado, que no recuerda.  ¿Qué significa eso en la práctica? Que necesitamos información adicional para implementar una “conversación” entre cliente y servidor. Esta idea de conversación, donde ambas partes recuerdan lo que han hablado hasta  el momento, se llama **sesión**.

Aunque el concepto o la duración de una sesión puede variar en distintos entornos, básicamente es el conjunto de interacciones entre cliente y servidor en un lapso de tiempo razonable.

La primera vez que un cliente realiza una petición, después de un tiempo sin interactuar, el servidor **abre una sesión**. Las subsecuentes peticiones desde ese cliente se consideran dentro de  la misma sesión. Si pasa mucho tiempo sin que el cliente realice una  petición, el servidor asume que ya no está conectado y termina la  sesión.

La biblioteca Flask nos ofrece este concepto de sesión. Pero si HTTP  no tiene información específica que permita identificar al usuario o la  sesión, ¿cómo sabe Flask a qué usuario corresponde una determinada  petición?

Existen al menos 3 formas de que una petición HTTP transporte información que permita identificar al usuario o la sesión:

- **Cookies**: las *cookies* son pequeños ficheros que se adjuntan a una respuesta HTTP, con información de identificación de usuario. En sucesivas peticiones HTTP, el navegador incluye ese  fichero automáticamente, por lo que el servidor podrá tener la  identificación de ese usuario.
- **Campos ocultos**: en los formularios que el servidor envía a cliente para que sean completados, incluyo un campo del tipo *input* que no se muestra al usuario; ese campo lleva información que cuando  los datos del formulario se envíen de vuelta al servidor le servirá para identificar al cliente.
- **Reescritura de URLs**: aunque nos parezcan iguales,  el servidor introduce automáticamente pequeñas modificaciones a las  URLs; de esta forma, la URL específica que solicite permitirá al  servidor identificar al cliente.

La buena noticia es que, generalmente, el mecanismo que se use para  transportar la información de sesión no es visible al desarrollador de  la aplicación web. Esto es exactamente lo que ocurre con el soporte de  sesiones que nos ofrece Flask, y que veremos a continuación.









Vamos a ver cómo podemos  manejar una sesión en Python y Flask para poder almacenar información  entre cada una de las peticiones que el cliente haga al servidor.

### Objeto Flask Session

El objeto que nos guarda la información entre sesiones se llama  session. Lo primero será importarlo dentro de nuestro código Python para luego poder usarlo:

```
from flask import Flask, session
```

Como la información de la sesión viaja del servidor al cliente ida y  vuelta, es importante que nadie en el camino, ni siquiera el propio  cliente, puedan alterar esa información. En particular, se debe evitar  que alguien de fuera “tome control” de la sesión. En seguridad  informática, este tipo de ataque se conoce como CSRF (Cross-site request forgery) o robo de sesión.

Para asegurarse que eso no ocurre, Flask utiliza técnicas de  encriptación para proteger la información. Para eso, necesita una clav  de encriptación/desencriptación. Esta clave, que sólo debe conocer el  servidor y que debe ser distinta para todos los programas servidor, se  crea en el propio código del servidor. Por ejemplo:

```
app.secret_key = 'esto-es-una-clave-muy-secreta'
```

De hecho, en el código de nuestra aplicación SocNet habrás visto una línea similar a esta:

```
app.secret_key = 'A0Zr98j/3yX R~XHH!jmN]LWX/,?RT'
```

Ahora ya sabes para qué sirve. Si quieres tener más información de  este problema de seguridad informática y su solución, puedes leer [aquí](http://flask.pocoo.org/snippets/3/).







Ahora vamos a ver ejemplo de cómo utilizamos las sesiones en nuestra aplicación favorita.

La primera función, ***load_user(email, passwd)\***, intenta cargar los datos desde un fichero.

**Importante**: aquí almacenamos los datos en un fichero por una cuestión de  simplicidad. En aplicaciones con cientos o miles de usuarios lo normal  es almacenar los datos en una base de datos propiamente dicha.

Si el fichero existe, carga los datos y a continuación comprueba que la clave suministrada coincida con la que está almacenada.

**Nota de seguridad**: otra vez, para facilitar las explicaciones y la comprensión de los  conceptos básicos, realizamos algunas simplificaciones que antes de  hacer público un sistema deben ser corregidas. Por ejemplo, nunca se  debe guardar una clave de usuario “en claro”, siempre se debe hacer  codificada con algún algoritmo como por ejemplo SHA-256. Lamentable  estas cuestiones quedan fuera del ámbito de este curso.

Si todas las condiciones son correctas, la función **guarda** los datos relevantes del usuario (nombre, mensajes que ha escrito hasta el momento, la clave, su correo electrónico y la lista de amigos) en la **sesión**, para que estén disponibles para las próxima llamadas desde el cliente.

```python
def load_user(email, passwd):
    """
    It loads data for the given user (identified by email) from the data directory.
    It looks for a file whose name matches the user email
    :param email: user id
    :param passwd: password to check in order to validate the user
    :return: content of the home page (app basic page) if user exists and password is correct
    """
    file_path = os.path.join(SITE_ROOT, "data/", email)
    if not os.path.isfile(file_path):
        return process_error("User not found / No existe un usuario con ese nombre", url_for("login"))
    with open(file_path, 'r') as f:
        data = json.load(f)
    if data['password'] != passwd:
        return process_error("Incorrect password / la clave no es correcta", url_for("login"))
    session['user_name'] = data['user_name']
    session['messages'] = data['messages']
    session['password'] = passwd
    session['email'] = email
    session['friends'] = data['friends']
    return redirect(url_for("home"))
```

De la misma forma, cuando un usuario quiere salir del sistema, debemos guardar los datos en el fichero (base de datos) para que estén disponibles la próxima vez que el  usuario vuelva a nuestra aplicación. Este es el objetivo de la función *save_current_user()*, que vuelca los datos de session en el fichero correspondiente.

Por otra parte, cuando un usuario se da de alta, se debe crear el fichero, previa comprobación que no exista un usuario con el mismo identificador (correo electrónico). Esto es lo que hace la función *create_user_file(name, email, passwd, passwd_confirmation)*.

```

```



```python
def save_current_user():
    datos = {
        "user_name": session["user_name"],
        "password": session['password'],
        "messages": session['messages'], # lista de tuplas (time_stamp, mensaje)
        "email": session['email'],
        "friends": session['friends']
    }
    file_path = os.path.join(SITE_ROOT, "data/", session['email'])
    with open(file_path, 'w') as f:
        json.dump(datos, f)


def create_user_file(name, email, passwd, passwd_confirmation):
    """
    It creates the file (in the /data directory) for storing user data. The file name will match the user email.
    If the file already exists, it returns an error.
    If the password does not match the confirmation, it returns an error.
    :param name: Name or nickname of the user
    :param email: user email, which will be later used for retrieving data
    :param passwd: password for future logins
    :param passwd_confirmation: confirmation, must match the password
    :return: if no error is found, it sends the user to the home page
    """

    directory = os.path.join(SITE_ROOT, "data")
    if not os.path.exists(directory):
        os.makedirs(directory)
    file_path = os.path.join(SITE_ROOT, "data/", email)
    if os.path.isfile(file_path):
        return process_error("The email is already used, you must select a different email / Ya existe un usuario con ese nombre", url_for("signup"))
    if passwd != passwd_confirmation:
        return process_error("Your password and confirmation password do not match / Las claves no coinciden", url_for("signup"))
    datos = {
        "user_name": name,
        "password": passwd,
        "messages": [],
        "friends": []
    }
    with open(file_path, 'w') as f:
        json.dump(datos, f)
    session['user_name'] = name
    session['password'] = passwd
    session['messages'] = []
    session['friends'] = []
    session['email'] = email
    return redirect(url_for("home"))
```
















## javascript









### Estructuras de control

| Código                                                       | Denominación                                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **`if(condición) {   código true }else {   código false}`**  | [Estructura de control condicional](https://www.w3schools.com/js/js_if_else.asp) |
| **`(condición) ? valor_true : valor_false`**                 | [Operador de control condicional](https://www.w3schools.com/js/js_comparisons.asp) |
| **`switch(expresión) {  case condición_1:   código condición_1   [break;]  …  case condición_n:   código condición_n   [break;]  default:   código default}`** | [Estructura de control condicional basada en la evaluación del valor de una expresión](https://www.w3schools.com/js/js_switch.asp) |
| **`for(ini; condición; actualización){  código bucle}`**     | [Bucle condicional](https://www.w3schools.com/js/js_loop_for.asp) |
| **`while(condición) {  código bucle}`**                      | [Bucle incondicional](https://www.w3schools.com/js/js_loop_while.asp) |
| **`do {  código bucle} while(condicion)`**                   | [Bucle incondicional](https://www.w3schools.com/js/js_loop_while.asp) |
| **`break`**                                                  | [Salida de un bloque de código](https://www.w3schools.com/js/js_break.asp) |
| **`continue`**                                               | [Parada de la ejecución de un bucle para volver a evaluar la condición de parada](https://www.w3schools.com/js/js_break.asp) |
| **`try {  código js}catch(err) {  código que gestiona el error}finally {  código a ejecutar haya o no error}`** | [Captura y gestión de errores en el código](https://www.w3schools.com/js/js_errors.asp) |

### Operadores

| Operador                                                     | Denominación                                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **`=`**                                                      | Asignación                                                   |
| **`+, -, \*, /, %`**                                         | [Operadores aritméticos](https://www.w3schools.com/js/js_arithmetic.asp) (actúan sobre valores numéricos) |
| **`++, --`**                                                 | [Incremento, decremento](https://www.w3schools.com/js/js_arithmetic.asp) |
| `**+=, -=, \*=, /=, %=** `                                   | [Operación aritmética + Asignación](https://www.w3schools.com/js/js_assignment.asp) |
| **`+, +=`**                                                  | Concatenación de Strings                                     |
| **`==, !=, >, <, >=, <=`**                                   | [Comparadores de valor](https://www.w3schools.com/js/js_comparisons.asp) |
| `**===** (igual valor e igual tipo)**!==** (distinto valor y distinto tipo)` | [Comparadores de valor y tipo](https://www.w3schools.com/js/js_comparisons.asp) |
| **`&&, ||, !`**                                              | [Operadores lógicos](https://www.w3schools.com/js/js_comparisons.asp) |



```html
<html>
<head>
    <script>
        var a = 3;
        
        function mostrarVariable(valor) {
            alert(valor);   // Aquí mostramos el valor que recibe como parámetro
            alert(a);       // Aquí, el valor de la variable global
        }
    </script>
</head>
<body>
    Ejemplo sencillo de uso de <i>JavaScript</i>
    <input type="button" value="Pulsar..." onclick="mostrarVariable(a)" />
    
    <script>
        alert(a);
        a = 5;
    </script>
</body>
</html>
```



La secuencia de acciones realizadas por el intérprete *JavaScript* al interpretar este código es:

- Crear una variable *a* y asignarle el valor numérico 3.*
  *
- Crear una función *mostrarVariable* que queda a la espera de ser invocada.
- Invocar a la función *alert* para mostrar el valor actual de *a*.
- Cambiar el valor de *a* asignándole el valor numérico 5.

¿Cuándo se invoca la función que hemos creado? En este caso, al  pulsar el botón que hemos añadido en el cuerpo del documento. Para ello  utilizamos su atributo *onclick*, en el que se indica el código *JavaScript* a ejecutar al "hacer clic" sobre el elemento. Lo que en realidad  estamos haciendo con esa definición es asociar una funcionalidad a un  evento, pero de momento no debes preocuparte por ello, en las siguientes secciones trataremos los eventos HTML en detalle.

## eventos



Estos son algunos de los eventos HTML que más habitualmente se  procesan en las aplicaciones web. Aunque procesando estos eventos  podrías desarrollar cualquier aplicación web típica, como ya sabes,  puedes encontrar la lista completa de eventos HTML en la [documentación de referencia de la W3C](https://www.w3schools.com/tags/ref_eventattributes.asp). Recuerda que para procesar un evento tienes que dar valor a la  propiedad correspondiente en el elemento HTML, por ejemplo, para  procesar el evento click debes fijar la propiedad *onclick*:

- *click*: se produce cuando el usuario hace click sobre el elemento HTML.
- *mouseover*: se produce cuando el usuario entra con el ratón dentro del elemento HTML.
- *mouseout*: se produce cuando el usuario sale con el ratón del elemento HTML.
- *focus*: se produce cuando el elemento HTML recibe el foco.
- *blur*: se produce cuando el elemento HTML pierde el foco.
- *change*: se produce cuando el valor del elemento HTML cambia (para <*input*>, <*select*> y <*textarea*>).
- *keydown/keypress*: se producen cuando el usuario presiona  una tecla estando dentro del elemento HTML. Aunque realmente son eventos distintos, en la mayoría de las ocasiones se pueden considerar  equivalentes.
- *keyup*: se produce cuando el usuario suelta una tecla estando dentro del elemento HTML.
- *submit*: se produce cuando se hace el submit de un formulario (para <*form*>).
- *load*: se produce cuando finaliza la carga del elemento HTML. Típicamente se utiliza para validar la carga del documento.
- *unload*: se produce cuando se descarga la página (para <*body*>).
- *error*: se produce cuando se produce un error al cargar un fichero externo.

A continuación, podéis ver un ejemplo ilustrativo de cómo vincular código *JavaScript* con alguno de estos eventos. El fichero [eventos.html](https://courses.edx.org/asset-v1:UAMx+WebApp+1T2019a+type@asset+block/eventos.html) contiene el código con el que se trabaja.



# Introducción a AJAX

La clave de todo este proceso de **llamadas asíncronas** en segundo plano al servidor es el objeto ***XMLHttpRequest\***. Al tratarse de un objeto *JavaScript*, la sintaxis para crearlo dentro de nuestro código JS es:

```
xmlhttp=new XMLHttpRequest();
```

Una vez instanciado, ofrece dos métodos para hacer una petición al servidor:

- ***open(tipo, url, asíncrona)***: Permite configurar los datos de la petición. En cierta medida, podemos considerar que la configuración de la llamada es análoga a la especificación de los datos de envío de un formulario  en el documento HTML. El tipo de petición (atributo *type* del elemento <*form*>) podrá ser "GET" o "POST". En el parámetro *url* indicaremos la url del servicio encargado de procesar la petición en el servidor (atributo *action* del elemento <*form*>). Y el tercer parámetro, sin equivalencia en el caso de un formulario en  el que las peticiones al servidor conllevan la carga síncrona de un  nuevo documento, es el indicador de si la petición debe procesarse de  forma asíncrona (*true*) o síncrona (*false*). Cuidado que es un indicador de asincronismo, no de sincronismo, lo que en ocasiones suele inducir a error a los desarrolladores.

- **send([query_string])**

  : Realiza el 

  envío de la petición al servidor

  . Si no se le pasan parámetros, se realiza una petición GET en la que los parámetros de la llamada se han debido informar como 

  query string

   en la url indicada en la llamada al método 

  open

  . Para peticiones POST, el método 

  send

   recibe un 

  query string

   con los parámetros que enviar al servidor (p.ej.,  "param1=valor1&param2=valor2"). En este último caso, para que los  parámetros se envíen en la cabecera HTTP en lugar de en la URL,  adicionalmente se debe incluir la siguiente línea antes del envío: 

  `xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");`

Así, para realizar una petición GET asíncrona que descargue la página de acceso al sitio web de la Universidad Autónoma de Madrid, haríamos  lo siguiente:

```
xmlhttp = new XMLHttpRequest();xmlhttp.open("GET","http://www.uam.com",true);xmlhttp.send();
```

# jQuery





<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>

```
$(selector).accion()
```

```
$("div#contenedor_lista input:checked").parent().hide()
```

```
$(document).ready(function() {   // Tú código irá aquí});
```

Esto se utiliza tan habitualmente que incluso existe una sintaxis abreviada para asignar funcionalidad al evento *ready()* del *document*. Lo siguiente es completamente equivalente a la asignación estándar:

```
$(function() {   // Tú código irá aquí});
```




Para reforzar los conocimientos adquiridos durante la lección 2, le recomendamos visitar los siguientes sitios: 

Página del gestor de paquetes que incluye Python

https://pypi.org 

https://realpython.com/what-is-pip/ 

https://realpython.com/pipenv-guide/ 

https://docs.scipy.org/doc/numpy-1.14.5/reference/ https://matplotlib.org 

https://www.edureka.co/blog/python-libraries/ 

https://packaging.python.org/tutorials/installing-packages/ 

https://packaging.python.org/tutorials/packaging-projects/ 

https://numpy.org/doc/stable/user/quickstart.html 

https://numpy.org/doc/stable/user/absolute_beginners.html 

https://www.geeksforgeeks.org/__init__-in-python/ 

https://www.w3schools.com/python/python_classes.asp 

https://www.w3schools.com/python/python_inheritance.asp https://es.wikipedia.org/wiki/Clase_(informática) 

https://es.wikipedia.org/wiki/Instancia_(informática) 

https://es.wikipedia.org/wiki/Constructor_(informática) 

https://www.w3schools.com/python/python_classes.asp 

https://www.w3schools.com/python/python_inheritance.asp 

https://www.programiz.com/python-programming/matrix https://matrix.reshish.com/es/multiplication.php 

https://realpython.com/python-modules-packages/#python-modules-overview 

https://www.geeksforgeeks.org/python-classes-and-objects/ 

Python Modules and Packages – An Introduction – Real Python

Python packages: How to create and import them?

Python Tutorial Python pip 

Python PIP Tutorial

pandas: powerful Python data analysis toolkit

numpy · PyPI

El Ingeniero González en cada video iba mostrando y explicando código de Python, lo mas recomendable es que usted vaya replicando el mismo. 

A continuación se comparte el código que se utilizó y explicó en los videos de lección 5 (es importante que usted ya haya instalado Python para correr este código):

Dar clic aquí para descargar código

Dar clic aquí para descargar código

Dar clic aquí para descargar código

Dar clic aquí para descargar código

Importante: el código fue creado siguiendo el siguiente formato: 

 PrintX_DIAPOSITIVAY_PZ

Donde X es el orden del programa en cuanto a que momento se utiliza, Y es la diapositiva donde se usa y Z la presentación.



