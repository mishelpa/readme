## Leyes Abiertas

---

### Nombre

- Español: Leyes Abiertas
- Inglés: Open Laws

### Descripción y contexto

---

**Leyes Abiertas** es una plataforma digital de elaboración colaborativa de normas que vincula a diputados/as con la ciudadanía. Su principal funcionalidad está destinada a mejorar los proyectos de ley a partir de aportes ciudadanos, respondiendo a los usos y costumbres de la Cámara de Diputados.

_1. Objetivos Principales:_

La plataforma fue creada para cubrir dos aspectos muy importantes:

- Abrir el trabajo legislativo dentro de la Cámara de Diputados a la ciudadanía, para facilitar y potenciar la participación ciudadana y la co-creación en este ámbito.

- Mejorar el contenido de las leyes, permitiendo profesionalizar el estudio de los proyectos de ley con esta instancia de revisión por parte de especialistas en las diversas temáticas e incorporar la perspectiva de personas y organizaciones involucradas y con experiencia en dichas temáticas.

_2. Objetivos Secundarios_

- Exhibición de los proyectos.
- Involucramiento de la ciudadanía en el armado de las leyes aumentando sus capacidades cívicas.

### Guía de usuario

---

La herramienta pone el foco en la visualización de anteproyectos de ley de forma directa, con descripciones sencillas y vídeos claros sobre su contenido. Cada uno cuenta con dos espacios de participación: un módulo de comentarios generales y otro de aportes sobre el articulado. Los/as diputados/as tienen sus cuentas verificadas, pueden responder de forma directa los comentarios y crear diferentes versiones del anteproyecto incorporando los aportes ciudadanos.

_¿Cómo funciona?_

Los Diputados suben propuestas y proyectos de ley para que puedan ser enriquecidas: se pueden hacer aportes, comentarios y sugerencias

A través de esta plataforma, la ciudadanía puede hacer comentarios generales, para dar su opinión o postura general sobre la propuesta de ley. Además, puede realizar aportes puntuales seleccionando una parte específica del texto y haciendo un aporte particular.

Los diputados analizarán los aportes. En la medida en se realicen cambios a la propuesta original se generarán nuevas versiones de la propuesta. Así, las y los usuarios cuyos aportes fueran incorporados, serán colaboradores en la redacción de la propuesta.

Para más detalles sobre las funcionalidades básicas de esta plataforma descargue/consulte el Manual de usuario[https://leyesabiertas.hcdn.gob.ar/static/files/congreso_manual_de_usuario.pdf]

### Guía de instalación

Hay dos formas de instalar leyes abiertas: Una es de forma clasica, descargando, haciendo setup y corriendo cada uno de los 3 repositorios en una terminal x/ repositorio.

La segunda forma es utilizando Docker y `docker-compose`. En solo un paso se instala y conectan todos. Si queres, podes explorar mas acerca de esta instalacion bajo el titulo "Instalacion (Docker)"

#### Instalacion (Clasica)

> ⚠️ _IMPORTANTE:_ Es necesario que instales mongo 3.6 en tu computadora, con una base de datos llamada "leyesabiertas". No hace falta crear alguna collection, eso lo hace la app en inicio.
> La instalacion requiere de instalar 3 repositorios, como minimo.
> Para eso, ubicados en la carpeta donde les gustaria clonar los repos, supongamos una carpeta `/dev` hacen en la terminal:

```bash
$ cd dev
dev/:$ git clone https://github.com/DemocraciaEnRed/leyesabiertas-web
dev/:$ git clone https://github.com/DemocraciaEnRed/leyesabiertas-core
dev/:$ git clone https://github.com/DemocraciaEnRed/leyesabiertas-notifier
```

Una vez hecho, hay que hacer un "Setup" de cada una de los repositorios que acabamos de clonar.

### Setup leyesabiertas-web

Ir a la carpeta del repo y instalar las dependencias.

```
dev/:$ cd leyesabiertas-web
dev/leyesabiertas-web:$ npm install
```

Ahora tenemos que crear un archivo `.env` que son nuestras variables de entorno

```env
API_URL=http://localhost:4000
AUTH_SERVER_URL=https://keycloak.democraciaenred.org/auth
REALM=leyes-abiertas
RESOURCE=leyes-abiertas-web-dev
SSL_REQUIRED=external
PUBLIC_CLIENT=true
CONFIDENTIAL_PORT=0
```

Comando para ejecutar:

```
dev/leyesabiertas-web:$ npm run dev
```

### Setup leyesabiertas-core

Ir a la carpeta del repo y instalar las dependencias.

```
dev/:$ cd leyesabiertas-core
dev/leyesabiertas-core:$ npm install
```

Ahora tenemos que crear un archivo `.env` que son nuestras variables de entorno

```env
PORT=4000
SESSION_SECRET=faQ0hgRgLyahbQVsCrHkgg5ahJgoawdRKIPWB9H9
MONGO_URL=mongodb://localhost/leyesabiertas
AUTH_SERVER_URL=https://keycloak.democraciaenred.org/auth
AUTH_REALM=leyes-abiertas
AUTH_CLIENT=leyes-abiertas-core-dev
NOTIFIER_URL=http://localhost:5000/api
```

Comando para ejecutar:

```
dev/leyesabiertas-core:$ npm run dev
```

### Setup leyesabiertas-notifier

Ir a la carpeta del repo y instalar las dependencias.

```
dev/:$ cd leyesabiertas-notifier
dev/leyesabiertas-web:$ npm install
```

Ahora tenemos que crear un archivo `.env` que son nuestras variables de entorno

```env
PORT=5000
MONGO_URL=mongodb://localhost
DB_COLLECTION=leyesabiertas
ORGANIZATION_EMAIL=guillermo@democracyos.io
ORGANIZATION_NAME='Portal de Leyes Abiertas'
ORGANIZATION_URL=https://fake.org
ORGANIZATION_API_URL=http://localhost:4000
NODEMAILER_HOST=my.fake.smtp.io
NODEMAILER_PASS=changeMe
NODEMAILER_USER=changeMe
NODEMAILER_PORT=25
NODEMAILER_SECURE=true
NODE_ENV=development
BULK_EMAIL_CHUNK_SIZE=100
```

Comando para ejecutar:

```
dev/leyesabiertas-notifier:$ npm run dev
```

### Instalacion Docker

```yml
version: "3"
services:
  core:
    image: democraciaenred/leyesabiertas-core:1.7.1
    environment:
      - PORT=3000
      - SESSION_SECRET=faQ0hgRgLyahbQVsCrHkgg5ahJgoawdRKIPWB9H9
      - MONGO_URL=mongodb://mongo/leyesabiertas
      - AUTH_SERVER_URL=https://keycloak.democraciaenred.org/auth
      - AUTH_REALM=leyes-abiertas
      - AUTH_CLIENT=leyes-abiertas-core-dev
      - NOTIFIER_URL=http://notifier:3000/api
    ports:
      - "4000:3000"
    depends_on:
      - mongo
    tty: true
  web:
    image: democraciaenred/leyesabiertas-web:1.7.2
    environment:
      - API_URL=http://localhost:4000
      - AUTH_SERVER_URL=https://keycloak.democraciaenred.org/auth
      - REALM=leyes-abiertas
      - RESOURCE=leyes-abiertas-web-dev
      - SSL_REQUIRED=external
      - PUBLIC_CLIENT=true
      - CONFIDENTIAL_PORT=0
    ports:
      - "3000:3000"
    depends_on:
      - core
    tty: true
  notifier:
    image: democraciaenred/leyesabiertas-notifier:1.7.3
    environment:
      - PORT=3000
      - MONGO_URL=mongodb://mongo:27017
      - DB_COLLECTION=leyesabiertas
      - ORGANIZATION_EMAIL=guillermo@democracyos.io
      - ORGANIZATION_NAME='Portal de Leyes Abiertas'
      - ORGANIZATION_URL=https://fake.org
      - ORGANIZATION_API_URL=http://core:3000
      - NODEMAILER_HOST=my.fake.smtp.io
      - NODEMAILER_PASS=changeMe
      - NODEMAILER_USER=changeMe
      - NODEMAILER_PORT=25
      - NODEMAILER_SECURE=true
      - NODE_ENV=development
      - BULK_EMAIL_CHUNK_SIZE=100
    ports:
      - "5000:3000"
    depends_on:
      - core
    tty: true
  mongo:
    image: mongo:3.6
    ports:
      - 32000:27017
    volumes:
      - ./mongo_db:/data/db
```

Una vez creado el archivo de docker-compose.yml, seguir las siguientes referencias de algunos comando utiles:

```bash
# Levantar servicios. Se puede detener si se hace Ctrl+C
$ docker-compose up
# Si los containers siguen corriendo y se quieren detener:
$ docker-compose stop
# Si se quiere eliminar solo los containers
$ docker-compose rm
# Si se quiere eliminar todo lo que el comando "up" inicializo (containers, volumenes, imagenes, etc)
$ docker-compose down
```

### Dependencias

- Librerias

        "dotenv": "^6.2.0",
        "get-selection-range": "^0.1.0",
        "immutable": "^3.8.2",
        "isomorphic-unfetch": "^2.1.1",
        "jimp": "^0.5.6",
        "jump.js": "^1.0.2",
        "keycloak-js": "4.4.0",
        "next": "^6.1.2",
        "react": "^16.8.6",
        "react-datepicker": "^2.7.0",
        "react-dom": "^16.8.6",
        "react-file-base64": "^1.0.3",
        "react-icons-kit": "^1.3.1",
        "react-masonry-component": "^6.2.1",
        "react-onclickoutside": "^6.8.0",
        "react-router": "^4.3.1",
        "react-router-dom": "^4.3.1",
        "slate": "0.43.0",
        "slate-react": "0.20.1",
        "styled-components": "^3.4.10",
        "video.js": "^7.6.5"

### Cómo contribuir

---

Esta sección explica a desarrolladores cuáles son las maneras habituales de enviar una solicitud de adhesión de nuevo código (“pull requests”), cómo declarar fallos en la herramienta y qué guías de estilo se deben usar al escribir más líneas de código. También puedes hacer un listado de puntos que se pueden mejorar de tu código para crear ideas de mejora.

### Código de conducta

---

El código de conducta establece las normas sociales, reglas y responsabilidades que los individuos y organizaciones deben seguir al interactuar de alguna manera con la herramienta digital o su comunidad. Es una buena práctica para crear un ambiente de respeto e inclusión en las contribuciones al proyecto.

La plataforma Github premia y ayuda a los repositorios dispongan de este archivo. Al crear CODE_OF_CONDUCT.md puedes empezar desde una plantilla sugerida por ellos. Puedes leer más sobre cómo crear un archivo de Código de Conducta (aquí)[https://help.github.com/articles/adding-a-code-of-conduct-to-your-project/]

### Autor

---

Fundación Democracia en Red.

### Información adicional

---

Esta es la sección que permite agregar más información de contexto al proyecto como alguna web de relevancia, proyectos similares o que hayan usado la misma tecnología.

### Licencia

---

[LICENCIA MIT](https://github.com/EL-BID/Plantilla-de-repositorio/blob/master/LICENSE.md)
