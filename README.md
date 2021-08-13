**Versión CICD de la práctica final del curso de DevOps.**

Se trata de una aplicación de SpringBoot con un único servicio REST que responde un mensaje de bienvenida en la url: http://localhost:8081

Para ponerlo en marcha, se proporciona un Dockerfile que utiliza una versión de **openjdk16** para generar el ejecutable, 
y lo copia a una imagen con un un **alpine con jre 16** donde se levanta la aplicación.
Comandos a ejecutar:
* `docker build -t hello-final:latest .`  Para construir la imagen
* `docker run hello-final:latest.`         Para instanciar la imagen creada

Se proporciona también un fichero *docker-compose.yaml*, con el que puede instanciarse la imagen construida utilizando el comando: `docker-compose up -d`

También se incluye un *Jenkinsfile* con el cual se automatiza este proceso desde [Jenkins Pipelines](https://www.jenkins.io/doc/book/pipeline/).

---

En esta versión, además de las pruebas añadidas al branch "testing", se añade en el **Jenkinsfile** integración y entrega/despliegue continuos (CI-CD) apoyándose en GitLab. Cada vez que se haga un commit se iniciará el pipeline en Jenkins, y, si se ejecuta correctamente, la imagen generada se subirá al registry de Gitlab, desde donde se podrá descargar mediante `docker pull` y ejecutar con `docker compose` desde cualquier máquina que tenga acceso a este repositorio.

