![Portada](https://github.com/agalvezcorell/Project.4-Creating_an_Stranger_Things_API/blob/master/input/portada.jpg)

# Creando una API de Stranger Things
Analizando sentimientos de un chat con TextBlob y realizando recomendaciones con NLP (procesamiento del lenguaje natural) en Python.

En este proyecto he creado una API de Stranger Things que permite obtener información de una base de datos que contiene mensajes que se han enviado los personajes en sus grupos de mensajería.

La API está desplegada en Heroku. El enlace es:
```
https://strangerapi.herokuapp.com
```

### ¿Cómo funciona?

## @post

- /new/user

Se pueden insertar personajes en la base de datos realizando una request.post a la API como en el siguiente ejemplo. El sistema checkeará que no existan los personajes.

```
user ={"name":"Mike Wheeler"}
url = "https://strangerapi.herokuapp.com/new/user"
requests.post(url, data=user)
```
- /new/chat

Se pueden insertar chats utilizando este comando. Necesitas tener los datos en forma de diccionario.
```
chat = { "chat_name": "friends",
           "participants": ["Mike Wheeler","Dustin Henderson","Will","Lucas"]
}
url = "https://strangerapi.herokuapp.com/new/chat"
requests.post(url_chat, data=dchat)
```
- /new/message

De esta manera insertamos un mensaje en la base de datos. 
```
mensaje = {'name': 'Once', 'chat_name': 'friends_new', 'message': 'Los amigos no mienten'}
url = "https://strangerapi.herokuapp.com/new/message"
requests.post(url, data=mensaje)
```
## @get

- /users

Con este endpoint obtenemos todos los usuarios.
```
url = "https://strangerapi.herokuapp.com/users"
requests.get(url).json()
```
- /chats

Con este endpoint podemos saber todos los grupos que hay creados
```
url = "https://strangerapi.herokuapp.com/chats"
requests.get(url).json()
```
- /user/chat/name

Con este endopoint obtenemos los chats en los que participa el usuario que le indiquemos.
```
url_chats = "https://strangerapi.herokuapp.com/user/chat/"
name = "Mike Wheeler"
requests.get(url_chats + name).json()
```
- /user/message/name

Con este endpoint obtenemos todos los mensajes que ha escrito un usuario.
```
url = "https://strangerapi.herokuapp.com/user/message/"
name = "Once"
requests.get(url + name).json()
```
- /chat/message/name

Con este endpoint obtenemos todos los mensajes que se han escrito en un chat.
```
url = "https://strangerapi.herokuapp.com/chat/message/"
grupo = "hawkins"
requests.get(url + grupo).json()
```

## Análisis de sentimientos
Con requests a la API podemos analizar los sentimientos de los mensajes que se han escrito en un chat, los sentimientos de una frase random de un usuario o de todos los mensajes de un usuario para saber si se expresa feliz o triste.

- /chat/sentiments/name

Analizamos la polaridad y subjetividad de los mensajes de este chat.
```
url = "https://strangerapi.herokuapp.com/chat/sentiments/"
grupo =  "hawkins"
requests.get(url_sentchat + grupo).json()
```
- /chat/sentiments/users/name

Analizamos la polaridad y subjetividad de un grupo y obtenemos también la polaridad y subjetividad de cómo se expresa cada usuario de ese grupo.
```
url= "https://strangerapi.herokuapp.com/chat/sentiments/users/"
gruporuser = "friends"
requests.get(url + gruporuser)
```

- /user/random/sentiments/name

Elegimos un usuario y el sistema nos devuelve el análisis de sentimientos de un mensaje elegido de forma aleatoria de entre todos los mensajes que ha escrito.
```
url= "https://strangerapi.herokuapp.com/user/random/sentiments/"
hopper =  "Jim Hopper"
requests.get(url + hopper).json()
```
- /user/sentiments/name

Con este comando analizamos la polaridad y subjetividad de los mensajes de un usuario.
```
url = "https://strangerapi.herokuapp.com/user/sentiments/"
dustin ="Dustin Henderson"
requests.get(url + dustin).json()
```

## Sistema de recomendaciones (NLP)
Mediante el procesado de lenguaje realizamos un sistema de recomendaciones basado en la temática de la que habla cada usuario.
En este caso, para obtener una recomendación de amistad, introduces un nombre en la request como en el siguiente ejemplo.
```
url= "https://strangerapi.herokuapp.com/recommend/"
name = "Kali"
requests.get(url + name).json()
```

## Funcionamiento en local ejecutando el .py o el contenedor de Docker
La api puede correr en local mediante un contenedor docker conectado a Atlas aplicando este comando en la terminal:
```
docker run -p 5000:5000 --env DB_URL="mongodb+srv://USER:PASS@cluster0-u7oa3.gcp.mongodb.net/apidb?retryWrites=true&w=majority" --env PORT=5000 strangerapi
```

O correr en local conectado a la db local con el siguiente comando:
```
docker run -p 5000:5000 --env DB_URL="mongodb://host.docker.internal/apidb" --env PORT=5000 strangerapi
```