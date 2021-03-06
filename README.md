# Servicio de búsqueda para jugadores de videojuegos

---

**Autor: Alejandro Campoy Nieves**

**Asignatura: Cloud Computing (Máster Profesional en Ingeniería Informática)**

**Universidad: Universidad de Granada (UGR)**

se puede acceder también a través de este [enlace](https://alejandrocn7.github.io/Proyecto-Cloud-Computing/).

## Descripción del problema

---

Cada vez es más común encontrarse videojuegos en el mercado totalmente centrados
en el modo **online** cooperativo o competitivo con otros jugadores. Los videojuegos
offline o de modo historia siguen teniendo éxito coexistiendo con los
anteriormente mencionados. Es muy común encontrarse con gente que le atrae un
videojuego pero simplemente no puede disfrutarlo al máximo porque no tiene con
quien jugarlo y esto reduce su experiencia como jugador.

Aunque siga habiendo de todo, los juegos online y los [deportes
electrónicos](https://es.wikipedia.org/wiki/Deportes_electr%C3%B3nicos) de jugadores en este
proyecto.

## Descripción de la solución

---

La idea consiste en crear una **plataforma** en la cual puedan **registrarse** los
jugadores de videojuegos especificando sus datos (nombre, edad, plataforma de juego, etc).
Entonces, el jugador en cuestión tendrá la posibilidad de especificar a qué videojuegos
está jugando en la actualidad y con qué frecuencia. A su vez, se desarrollará un
**buscador** de otros perfiles mediante distintos criterios de búsqueda, como un
videojuego en concreto al que esté jugando. Con ello permitiremos que jugadores
con gustos y objetivos similares dentro de un videojuego puedan ponerse en **contacto**
y disfrutar de la experiencia extra que supone degustar un videojuego en compañía.

## Arquitectura

---

Se pretende realizar un despliegue en la nube utilizando para ello una arquitectura basada en
[microservicios](https://www.redhat.com/es/topics/microservices). De esta forma tenemos la posibilidad de dar un servicio grande
presentándolo como un conjunto de pequeños servicios (microservicios) que funcionan
de una forma totalmente independiente (aunque luego se comuniquen y colaboren entre ellos). En función de las necesidades que se han especificado en la descripción de la solución, inicialmente, planteo el desarrollo de los siguientes microservicios:

- Gestión de usuarios (Sing up, log in, modificación del perfil de usuario...).

- Gestión de la base de datos MongoDB.

- Microservicio de búsqueda de jugadores por criterio.

- Microservicio para mostrar la información de una forma determinada.

## Comunicación entre microservicios y Servicio web

---

Tal y como se ha mencionado en clase, la idea es comunicar los microservicios con un broker llamado [RabbitMQ](https://www.rabbitmq.com/).

Las peticiones al servidor se realizarán utilizando [API REST](https://bbvaopen4u.com/es/actualidad/api-rest-que-es-y-cuales-son-sus-ventajas-en-el-desarrollo-de-proyectos). Implementando las peticiones HTTP usuales como GET, POST, DELETE y PUT. Veremos más adelante como poder hacer esto.

## Desarrollo

---

El lenguaje que vamos a utilizar para implementar cada uno de los microservicios mencionados anteriormente va a ser, en principio, [Python](https://www.python.org/). Va a ser vinculado a una base de datos no relacional llamada [MongoDB](https://www.mongodb.com/es) a través de [Pymongo](https://api.mongodb.com/python/current/), que es una distribución de Python la cual contiene herramientas para trabajar con esta base de datos. Además, vamos a hacer uso de un framework para desarrollo web llamado [Flask](http://flask.pocoo.org/) cuya finalidad es facilitarnos en cierta medida el trabajo de desarrollo y montaje del servicio web. Además, haremos uso de un microframework específico dentro de lo que es Flask para poder diseñar la API REST de una forma más cómoda, se llama [Flask RESTful](https://flask-restful.readthedocs.io/en/latest/)

## Pruebas y test

---

En principio, para realizar un desarrollo basado en pruebas, haremos uso de [Unittest](https://docs.python.org/3/library/unittest.html) para Python, aunque no se descarta utilizar algún marco que nos ayude a crearlos en un alto nivel como puede ser [Pocha](https://github.com/rlgomes/pocha) ([Mocha](https://mochajs.org/) para Python). Además, debemos tener en cuenta que queremos automatizar el proceso todo lo posible, de tal forma que cuando actualicemos el repositorio de Github se comprueben los tests realizados en Unittest y nos notifique cuando haya algún tipo de problema. Siempre intentando manterner el mayor porcentaje de cobertura posible en el software que se diseña.

## Despliegue

---
Despliegue: https://pruebacc.herokuapp.com/

El despliegue del servivio web se llevará a cabo utilizando [Heroku](https://devcenter.heroku.com/), siguiendo la filosofía de plataforma como un servicio (Paas) en la nube. Esto nos permite tener a nuestra disposición un servidor en el que poder desplegar nuestro proyecto en la nube de forma gratuita. El ser gratuito implica que tenemos limitaciones a la hora de hacerlo, pero no nos supone un problema para realizar los primeros pasos de este proyecto.

Todos los datos que se reciben desde el servidor están en formato [JSON](https://es.wikipedia.org/wiki/JSON). El enlace de despliegue que se acaba de mostrar nos permite hacer un GET (o cualquier otra orden, pero esta es la que hace un navegador web por defecto) a la raíz de nuestro servicio web con la finalidad de obtener el recurso. Los recursos que tenemos ahora mismo desplegados son:

- [/](https://pruebacc.herokuapp.com/) : Nos permite acceder a la raíz en la que se muestra status:OK en caso de que este levantado el servidor.
- [/principal](https://pruebacc.herokuapp.com/principal): Hace exactamente lo mismo que la raíz, es una prueba que realice para comprobar si podía poner el mismo recurso en más de una ruta.
- [/jugadores](https://pruebacc.herokuapp.com/jugadores): Muestra datos de un conjunto de 3 jugadores que se han creado como clase para este servicio.
- [/jugadores/jugador1](https://pruebacc.herokuapp.com/jugadores/jugador1): Muestra únicamente los datos del primer jugador.
- [/jugadores/jugador2](https://pruebacc.herokuapp.com/jugadores/jugador2): Muestra únicamente los datos del segundo jugador.
- [/jugadores/jugador3](https://pruebacc.herokuapp.com/jugadores/jugador3): Muestra únicamente los datos del tercer jugador.

### Funcionalidad

#### Model.py

El archivo [model.py](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/model.py) se encuentra en la raíz de este repositorio. Aquí he implementado la clase Jugador con la que generaré los datos con los que trabajará al servicio web que desarrollaremos más adelante. Básicamente una instancia de la clase Jugador tiene los siguientes datos:

- **Nick** : Es el alias característico de cada uno de los jugadores (tipo str).
- **Nombre**: Nombre real del jugador (tipo str).
- **Apellidos**: Apellidos del jugador (tipo str).
- **Edad**: Edad del jugador (tipo int).
- **Videojuegos**: Videojuegos a los que suele jugar este jugador (lista de Python).
- **Competitivo**: Indica si el jugador compite en algún equipo de deportes electrónicos o no (tipo bool)

A parte tiene definidas algunos métodos como añadir un nuevo videojuego a la lista o eliminarlo, poder modificar el Nick y poder convertir los datos de la instancia en un diccionario. El objetivo de esto último es poder tener las instancias de jugadores representadas por diccionarios de Python debido a que son muy parecidos al formato JSON, de tal modo que Flask RESTful trabaja muy bien con estos contenedores y los transforma automáticamente a JSON, añadiendo incluso la cabecera.

#### principal.py

El archivo [principal.py](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/principal.py) también se encuentra en la raíz del repositorio y en el hemos creado el API REST con el que damos el servicio web. Para ello, como ya hemos mencionado anteriormente, hemos utilizado la ayuda de Flask y Flask RESTful. Con esta implementación ya podemos realizar el despliegue como localhost en el computador sin la necesidad de nada más y poder probar como se comporta el modelo con peticiones al servicio. Dependiendo de la ruta hemos diseñado distintas peticiones:

- Para la raíz un GET que devuelva status:ok en JSON.
- Para */jugadores* un GET que nos devuelve un JSON de los tres jugadores creados y un POST para añadir un nuevo jugador a la lista de jugadores (creando un recurso nuevo en una ruta nueva)
- Para */jugadores/jugador?* hemos implementado un GET para obtener los datos concretos de ese jugador en formato JSON, un DELETE para borrar el recurso y, por tanto, la ruta y un PUT que, dependiendo de si la ruta ya existe o no, crea o modifica un recurso (vamos, un jugador).

Cabe destacar que Flask RESTful crea las cabeceras de los paquetes de respuesta del servidor automáticamente. Se especifica que el tipo MIME es JSON y el código de estado también lo devuelve de una forma automática, aunque podemos especificar nosotros mismo que código determinado queremos que muestre en la cabecera dependiendo de la situación (ver algunos returns del archivo). Esto me parece muy útil porque no hay necesidad de implementar cada una de las cabeceras típicas que se devuelven, solo cuando queremos especificar un detalle concreto es necesario especificar este tipo de cosas.

Las líneas:
~~~
if (__name__ == '__main__'):
    # Esto es para que pueda abrirse desde cualquier puerto y direccion(de esta forma en heroku no nos da error).
    port = int(os.environ.get("PORT", 5000))
    app.run(host="0.0.0.0", port=port,debug=True)
~~~

Se han realizado de esta forma debido a un problema que tuve durante el desarrollo. Resulta que al tener el puerto 5000 y el host 127.0.0.1, Heroku no podía desplegar la aplicación correctamente.

#### test_model.py & test_web.py

Ya tenemos tanto el modelo, como la API REST con la que los clientes van a poder realizar peticiones a ese servicio. El siguiente paso que he llevado a cabo consiste en realizar unos test ayudándome de Unittest con la finalidad de dar la máxima cobertura al software desarrollado, tanto en la clase modelo ([test_model.py](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/test_model.py)), como en la API REST realizada ([test_web.py](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/test_web.py)). De esta forma si en el futuro se intenta actualizar la versión de Python o cualquier otro tipo de cambio es producido en el entorno, podemos saber de una forma rápida y bastante precisa como puede influir negativamente esto en lo que tenemos hecho, y poder amoldarlo de nuevo para que continúe funcionando. Se ha intentado testear todo.

### Definición de la Infraestructura

Recordemos que el objetivo es desplegar todo lo que se esta desarrollando en Heroku. La definición de la infraestructura es fundamental. Aunque vincule Heroku a mi repositorio de Github, no ocurrirá nada si no especifico los datos necesarios para que Heroku sepa como debe realizar ese despliegue, eso es lo que se entiende por infraestructura.

Entonces, he creado y añadido a la raíz del repositorio los siguientes archivos:

- [requirements.txt](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/requirements.txt): He creado un entorno virtual en Python ayudándome con [virtualenv](https://virtualenv.pypa.io/en/latest/) para instalar justo lo que necesitaba para este despliegue. Básicamente es Flask, Flask RESTful y Gunicorn (hablaré de Gunicorn más adelante). Aunque para crear el archivo de requirements he hecho uso de [pipreqs](https://github.com/bndr/pipreqs) en lugar de pip freeze dado que este analiza el proyecto y pone justo lo que necesitamos.
- [runtime.txt](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/runtime.txt): En este archivo solo tenemos que especificar la versión de Python que queremos que se utilice en el despliegue y que, por tanto, utilice el servidor (por lo tanto, no tiene por qué ser exactamente el mismo con el que hemos realizado la implementación necesariamente). En mi caso he seleccionado Python 3.6.6 porque es el que tenía instalado en mi computador y seleccionado en el entorno virtual con el que desarrollé toda el código.
- [Procfile](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/Procfile): Solo tengo especificado en su interior la siguiente línea:
`web: gunicorn principal:app`
La palabra "web" sirve para especificar que se trata de un proceso que tiene que realizar Heroku y que puede recibir tráfico externo en forma HTTP (justo con lo que consiste una API REST). Lo siguiente es el comando que se va a ejecutar. Le especificamos que ejecute [Gunicorn](https://gunicorn.org/), que es un servidor WSGI HTTP para Python que nos permite correr el servidor. He seleccionado este porque es complatible con Flask. Lo último es especificar que para saber lo que tiene que correr tiene que mirar en *principal.py*, concretamente la instancia llamada "app" de Flask que hemos creado.

### Test con Travis

Como ya sabemos, realizamos los test con Unittest. Sin embargo, queremos automatizar la ejecución de los mismos de tal forma que cada vez que actualicemos el repositorio de Github se ejecuten para comprobar si hay algún problema.

Para conseguir esto, he vinculado la cuenta de Github con Travis a través de su página web y he especificado los repositorios que quiero que revise cada vez que los actualice como se aprecia en la siguiente imagen.

![Travis enlace Github](docs/figuras/hito2/TravisConfig.png)

Sin embargo, esto no es suficiente. Debemos de especificarle a Travis donde están los archivos que contienen los tests y como debe ejecutarlos para poder comprobarlos. Supongo que esto se puede considerar también parte de la infraestructura. Para ello he creado el archivo [.travis.yml](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/.travis.yml) en la raíz del repositorio. De esta forma Travis siempre ejecuta este archivo y ve que tiene que utilizar Python para ejecutar test_model.py t test_web-py con unittest.

Cada vez que actualizamos el repositorio a partir de este momento debería de aparecer en la página web de Travis algo parecido a la siguiente imagen si todo ha salido bien.

![Test de Travis](docs/figuras/hito2/Travis.png)

### Vinculación con Heroku

Para vincular con Heroku podemos hacerlo de dos formas principalmente: desde su interfaz en su página web o desde la terminal (yo lo he hecho en Ubuntu 18.04 LTS) instalando Heroku CLI. El tema de crear el proyecto lo he realizado de las dos formas y es algo sencillo.

Una vez tenemos creado el proyecto de Heroku debemos de vincularlo con nuestra Github, especificando el repositorio concreto con el que queremos que se vincule. Entonces, tendremos la posibilidad de activar el despliegue automático cada vez que actualicemos el repositorio. Sin embargo, es muy importante marcar la opción en la que pone "Wait for CI to pass before deploy" para que no se despliegue en caso de que no pase los tests en Travis. ¿Sino para que he empleado tiempo en hacerlo?

![Despliegue automático](docs/figuras/hito2/HerokuDespliegue.png)

A partir de ahora, cada vez que actualicemos el repositorio debería de desplegarse en Heroku. Podemos ver el proceso de este despliegue desde la interfaz de Heroku o desde Github accediendo en la pestaña llamada "environments" como se aprecia en la siguiente imagen:

![Github Environments](docs/figuras/hito2/Entornos.png)

## Provisionamiento

---
En este apartado hablaremos del provisionamiento automático de máquinas virtuales a través de [Ansible](https://www.ansible.com/). Entiendo a Ansible como una herramienta software de administración y manejo de máquinas virtuales. En este punto se pretende ir un paso más hayá y provisionarlas de las herramientas necesarias para poder proporcionar el servicio web que se pretende en este proyecto. He decidido utilizar Ansible y no otra alternativa debido a que nuestro profesor de Cloud Computing, JJ Melero, decidió hacer un seminario sobre esta herramienta ([enlace al seminario](https://www.youtube.com/watch?v=gFd9aj78_SM&t=1277s)). Este seminario se realizó más o menos en un margen temporal cercano al comienzo de este hito, por lo que me ha resultado de gran utilidad.

### Mejoras realizadas en el proyecto

En este punto del desarrollo, y antes de entrar en el tema de provisionamiento, se han realizado una serie de mejoras.

Se ha incluido el uso de una base de datos (MongoDB) como un nuevo microservicio añadido ([ver mongoDB.py](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/mongoDB.py)). La forma de desplegar la base de datos ha sido a través de [mlab](https://mlab.com/), sigue la filosofía "Database-as-a-Service". He elegido esto porque hace que no tengamos que preocuparnos del despliegue de la misma. Es una forma de acceder a servicios que proporciona la nube; uno de los objetivos de esta asignatura.

Se han realizado tests acorde al uso de esta base de datos desplegada en mlab. Se puede ver los tests realizados en [test_mongo.py](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/tests/test_mongo.py). Básicamente se comprueba que podemos insertar, eliminar, actualizar, etc, la base de datos desplegada en mlab de forma correcta y cumpliendo con especificaciones funcionales concretas que he decidido diseñar de esa forma. Por ejemplo, que no se pueda insertar dos jugadores con el mismo Nick, ya que lo utilizo como identificador para luego dar el servicio web (los nombres de las rutas ahora corresponden con el Nick de los jugadores).

He tenido que realizar pequeñas [modificaciones en principal.py](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/commit/a758285dfb726f4a80b5ed745ae6e2baa43dc6a1). Como ya he mencionado antes, ahora el nombre de las rutas en el servicio web depende del nick de los jugadores (algo que tiene mayor lógica). Utilizamos directamente la base de datos en lugar de un diccionario de Python para almacenar los datos. Incluida la petición REST DELETE para el conjunto de jugadores (ruta <ip>/jugadores), esto dejaría la base de datos vacía (comprobado en los tests).

Finalmente, se ha [modificado test_web.py](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/commit/6b6dbe458b0f0f52527f0f20cec18076301febfe#diff-b946cfc9c7d6cd8ba6ce887aa8cdcd44), ya que aquí es donde comprobabamos las rutas y peticiones REST de nuestro servicio web. Es importante destacar que estos cambios son realizados a causa de la actualización del nombre de las rutas, no de añadir una base de datos, ya que la idea es que se encuentre de forma aislada en microservicios. En definitiva, se han tenido que cambiar las comprobaciones en las rutas a la hora de realizar las distintas peticiones, simplemente.

### Vagrant

Tal y como se pide en el hito de esta práctica, se han realizado primero pruebas con máquinas virtuales locales para realizar el provisionamiento desde ansible y probarlo en localhost. Para ello he utilizado la herramienta [vagrant](https://www.vagrantup.com/docs/index.html) ya que es una forma muy sencilla de crear máquinas virtuales a través de virtualbox, en mi caso, con el cual ya estaba familiarizado previamente y fue otro de los factores por el que decidí utilizarlo y que explicó JJ en el seminario de Ansible. Los archivos de configuración de Ansible pueden apreciarse en la carpeta [vagrant](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/tree/master/provision/vagrant) dentro de [provision](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/tree/master/provision).

Para comenzar, [ansible.cfg]https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/provision/vagrant/ansible.cfg) es un archivo que hemos creado para que Ansible no utilice su configuración por defecto, sino que vamos a establecer una configuración concreta cuando lo ejecutemos dentro de este subdirectorio. Como estamos trabajando con vagrant, vagrant crea un directorio oculto (.vagrant) en el que guarda información de la máquina virtual. Aquí especificamos que no queremos que se realicen comprobaciones de la clave del host, esto nos permite la posibilidad de meternos en diferentes máquinas virtuales con diferentes nombres y misma dirección MAC, o con el mismo nombre y diferentes MAC, sin que haya ningún problema y no te haga este tipo de comprobación SSH.

La segunda parte del archivo sirve para especificar el inventario. Le decimos a Ansible el archivo en el que se encuentra la información que necesita para saber como conectarse a las distintas máquinas virtuales que tenemos creadas. Este archivo es [ansible_hosts](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/provision/vagrant/ansible_hosts).

En su interior le especificamos una única máquina llamada debian9Vagrant la cual Ansible sabe a partir de este momento que tiene que comunicarse por el puerto 2222. La clave privada para poder conectarse se encuentra dentro de la carpeta oculta de vagrant que mencionamos anteriormente (.vagrant), de lo contrario ansible no sería capaz de acceder a ella para realizar su provisionamiento automático. Finalmente, especificamos en este archivo el host (localhost) y el usuario con el que tiene que acceder a la máquina llamado "vagrant".

De esta forma ansible tiene todo lo necesario para comunicarse y acceder a la máquina virtual que hemos creado, aunque podría ser más de una.

![acceso a la máquina vagrant a través de Ansible](docs/figuras/hito3/vagrant.png)

El siguiente paso es realizar un guión con el que ansible pudiera hacer un provisionamiento automático a la o las máquinas.

Para ellos se utilizan los playbooks, en mi caso he creado [playbook.yml](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/provision/vagrant/playbook.yml) en el cual hacemos básicamente las siguientes acciones dentro de la máquina:

- Instalar git si no se encuentra.
- Instalar herramientas como curl y pip para python3.
- Clonar este repositorio de github (sin permisos de superusuario)
- Instalar dependencias de los servicios que proporcionamos para pip ([requirements.txt](https://github.com/AlejandroCN7/Proyecto-Cloud-Computing/blob/master/requirements.txt))

![Provisionamiento de vagrant con Ansible](docs/figuras/hito3/provisionamiento-vagrant.png)

Mencionar que salen estados OK porque ya lo había ejecutado antes, por lo que ya lo tiene instalado en la máquina y no lo vuelve a hacer, solo lo comprueba.

Una vez hecho esto podemos entrar en la máquina, ejecutar el servicio con gunicorn y comprobar que funciona en localhost, sin embargo veremos su funcionamiento ya desplegado directamente a continuación.

### Azure

Lo interesante de este hito es la posibilidad de desplegar nuestra máquina virtual a través de la herramienta [Azure](https://azure.microsoft.com/es-es/) y de esta forma provisionar con Ansible algo que se encuentra en la nube en ese momento. Esto me resulta muy interesante, por ejemplo, se me ocurre la posibilidad de ejecutar nuevos playbooks mientras la máquina funciona para realizar nuevos cambios, modificar el servicio o sustituirlo por otro.

He elegido Azure para realizar el despliegue de la máquina debido a que JJ nos proporcionó dolares para poder utilizar esta herramienta y quería aprovecharlo.

Lo primero que hay que hacer una vez nos registramos en Azure es crear nuestra máquina virtual "virgen", es decir, sin nada instalado. Para ello he utilizado la propia interfaz de la página. En un principio, había instalado en ella el sistama operativo [Debian9]() para servidores. Sinceramente, no lo hice por ningún motivo especial, simplemente porque aun estaba realizando pruebas y fue el primero que se me vino a la cabeza.

Entonces, como se puede ver en el [antiguo estado de playbook.yml para azure]() me dió muchos problemas para poder redirigir el puerto 5000 al 80. Y aún así, seguía teniendo problemas con eso. Por ello, decidí cambiarme a [Ubuntu Server 16.04 LTS]().

Esta vez si me pensé mejor cual era el SO que quería correr en mi máquina virtual. Las principales cuestiones por las que elegí Ubuntu Server es porque estoy familiarizado a su funcionamiento gracias a la asignatura de Ingeniería de Servidores en el Grado en Ingeniería Informática de la Universidad de Granada. Además, es estos sistemas ya tenemos instalado python2 y python3 preinstalado, por lo que nos ahorra parte del trabajo.

El archivo [ansible.cfg]() es igual al explicado anteriormente.

## Orquestación

Hitos futuros.

## Contenedores

Hitos futuros.

## Licencia

---

Este proyecto está bajo la licencia de [GNU GENERAL PUBLIC LICENSE](https://es.wikipedia.org/wiki/GNU_General_Public_License)
