**Versión testing de la práctica final del curso de DevOps.**

Se trata de una aplicación de SpringBoot con un único servicio REST que responde un mensaje de bienvenida en la url: http://localhost:8081

Para ponerlo en marcha, se proporciona un Dockerfile que utiliza una versión de **openjdk16** para generar el ejecutable, 
y lo copia a una imagen con un un **alpine con jre 16** donde se levanta la aplicación.
Comandos a ejecutar:
* `docker build -t hello-final:latest .`  Para construir la imagen
* `docker run hello-final:latest.`         Para instanciar la imagen creada

Se proporciona también un fichero *docker-compose.yaml*, con el que puede instanciarse la imagen construida utilizando el comando: `docker-compose up -d`

También se incluye un *Jenkinsfile* con el cual se automatiza este proceso desde [Jenkins Pipelines](https://www.jenkins.io/doc/book/pipeline/).

---

En esta versión se han añadido pruebas de varios tipos:
* Pruebas de mutación con [Pitest](http://pitest.org/). Pueden ejecutarse con el comando: `./gradlew pitest`
* Pruebas unitarias con [JUnit5](https://junit.org/junit5/). Pueden ejecutarse con el comando: `./gradlew test`
* Pruebas de cobertura de código con [JaCoCo](https://www.eclemma.org/jacoco/).

Estas pruebas se han automatizado en el **Jenkinsfile**. Adicionalmente, en la fase de *QA* se realiza un `./gradlew check` cuyos resultados se recogen mediante el plugin [WarningsNG](https://plugins.jenkins.io/warnings-ng/), para analizar la calidad y seguridad del código. Los datos recogidos son analizados a su vez por los siguientes plugins:
* [PMD](https://pmd.github.io/) Para el análisis de la calidad del código.
* [SpotBugs](https://spotbugs.github.io/) Para detectar erorres, o partes del código que puedan llegar a serlo potencialmente.
* Pitest

Finalmente, tras la construcción de la imagen de docker, se comprueba que no tenga vulnerabilidades o problemas de configuración utilizando [Trivy](https://github.com/aquasecurity/trivy)