# Instalación / Configuración entorno ISD / 2026-2027 - Windows
-------------------------------------------------------------------------------

## Descargar e instalar el SW

> NOTA: Se recomienda utilizar un usuario de Windows sin espacios en el nombre 
  para evitar problemas con Maven.

- Descargar y descomprimir en `C:\software` el siguiente software:
    - Maven 3.9.x o superior:
        + https://maven.apache.org/download.cgi
        + Descargar el "Binary zip archive"
    - Tomcat 11.x.y:
      + https://tomcat.apache.org/download-11.cgi
      + En el apartado "Binary Distributions" / "Core" descargar el zip.

- Descargar e instalar Temurin JDK 25 LTS:
    - https://adoptium.net/es/temurin/releases?version=25&os=any&arch=any
    - Descargar el instalador .msi para Windows e instalar usando las opciones por defecto.

- Descargar e instalar IntelliJ IDEA:
    - https://www.jetbrains.com/es-es/idea/download
        + Se puede utilizar la versión libre o solicitar una licencia para
          estudiantes en https://www.jetbrains.com/es-es/community/education/#students). 
    - Instalar usando las opciones por defecto.
	 
- Descargar e instalar MySQL 9.7.x LTS:
    - https://dev.mysql.com/downloads/mysql/
        + Descargar el instalador .msi para Windows de la versión 9.7.x
    - Instalar en la ruta por defecto.
    - Elegir "Typical" y usar las opciones por defecto.
    - Después de la instalación, se ejecutará el wizard de Configuración de 
      MySQL Server. 
         + Utilizar las opciones por defecto excepto la contraseña para el 
         usuario `root`, que no debe dejarse vacía (puede usarse, por ejemplo, 
         `root`).
         + Comprobar que la opción "Start the MySQL Server at System Startup"
         está marcada, para que se instale como servicio Windows.

- Descargar el compilador de Apache Thrift:
     - https://www.apache.org/dyn/closer.cgi?path=/thrift/0.24.0/thrift-0.24.0.exe
     - Renombrar el ejecutable a `thrift.exe` y copiarlo dentro del directorio `bin` que 
       hay dentro del directorio donde se descomprimió Maven.

- Descargar e instalar Git
     - https://git-scm.com/downloads
     - Hacer clic en "Windows" e instalar como se indica.
     
## Descargar y descomprimir los ejemplos de la asignatura 

> Disponibles en el campus online

- Descargar en `C:\software`
  
## Establecer variables de entorno

- Ir a "Panel de Control > Sistema > Configuración avanzada del sistema > Variables de entorno ..."

- En la sección "Variables de usuario para `<user>`", crear las siguientes
  variables de entorno (para cada una pulsar en "Nueva ...", introducir el 
  nombre y el valor, y pulsar "Aceptar").
    - Nombre: `JAVA_HOME`
        + Valor: Directorio donde se instaló Temurin JDK 25
        + Por ejemplo:`C:\Program Files\Eclipse Adoptium\jdk-25.0.2.10-hotspot`
    - Nombre: `MAVEN_HOME`
        + Valor: Directorio donde se descomprimió Maven
        + Por ejemplo: `C:\software\apache-maven-3.9.15`
    - Nombre: `MAVEN_OPTS`
        + Valor: `-Xms512m -Xmx1024m`
    - Nombre: `MYSQL_HOME`
        + Valor: Directorio donde se instaló MySQL
        + Por ejemplo: `C:\Program Files\MySQL\MySQL Server 9.7`

- En la sección "Variables de usuario para `<user>`", modificar la variable de
  entorno `PATH`. Para ello hay que seleccionarla, pulsar en "Editar..." y 
  añadir al principio de su valor (sin borrar su valor antiguo):
  
  `%JAVA_HOME%\bin;%MAVEN_HOME%\bin;%MYSQL_HOME%\bin;`
  
> NOTA: Si la variable de entorno PATH no existiese, entonces habría que 
    crearla procediendo de igual forma que se hizo con las variables anteriores.
    
- Cerrar todos los terminales y abrir terminales nuevos.

- Comprobar que el entorno ha quedado correctamente configurado comprobando 
  salidas de los siguientes comandos:
  
```shell 
	java -version
	mvn -version
	mysqld --version
	thrift --version
```

## Creación de bases de datos necesarias para los ejemplos
- Arrancar MySQL.
  - Si se ha instalado como servicio seguramente se haya iniciado de forma 
    automática. En otro caso habría que iniciar el servicio manualmente.
    
> NOTA: En Panel de Control, Servicios Locales se puede configurar arranque 
  automático o manual. También se puede arrancar y detener.
           
> NOTA: En los siguientes pasos, al ejecutar los comandos  `mysqladmin` y `myqsl` 
  con la opción `-p` la password que nos solicitarán es la del usuario
  root que se especificó al instalar MySQL.

- Creación de bases de datos ws y wstest (ejecutar desde una consola):

```shell
    mysqladmin -u root create ws -p
    mysqladmin -u root create wstest -p
```

- Creación de usuario ws con password con permisos sobre ws y wstest:

```shell
    mysql -u root -p
        CREATE USER 'ws'@'localhost' IDENTIFIED BY 'ws';
        GRANT ALL PRIVILEGES ON ws.* to 'ws'@'localhost' WITH GRANT OPTION;
        GRANT ALL PRIVILEGES ON wstest.* to 'ws'@'localhost' WITH GRANT OPTION;
        exit
```

- Comprobar acceso a BD:

```shell
    mysql -u ws --password=ws ws
        exit

    mysql -u ws --password=ws wstest
        exit
```

## Inicialización de datos de ejemplo y compilación de los ejemplos

- Inicialización de la base de datos y compilación de los ejemplos:

```shell
    cd C:/software/ws-javaexamples-3.10.0
    mvn sql:execute install
```
	
## Configuración de IntelliJ IDEA
- Se recomienda instalar el plugin de Thrift (lo sugerirá el editor al abrir un fichero .thrift).
- Se recomienda configurar "Command Prompt" como el terminal por defecto. Se puede hacer en el wizard 
  `File > Settings > Tools > Terminal` seleccionando `C:\Windows\system32\cmd.exe` como valor de `Shell path`.


## Configuración de Tomcat

- Copiar el driver JDBC de MySQL al directorio `C:\software\apache-tomcat-11.x.y\lib`.
    - El driver JDBC se puede obtener de la siguiente ruta (siempre y cuando se hayan compilado previamente 
    los ejemplos):
     `%HOME%\.m2\repository\com\mysql\mysql-connector-j\9.7.0\mysql-connector-j-9.7.0.jar`

- Definir un data source con nombre `jdbc/ws-javaexamples-ds`:
    - Añadir las siguientes líneas al fichero `C:\software\apache-tomcat-11.x.y\conf\server.xml`, 
      dentro de la etiqueta `<GlobalNamingResources>`:
 
      ```shell
      <!-- MySQL -->
      <Resource name="jdbc/ws-javaexamples-ds"
                auth="Container"
                type="javax.sql.DataSource"
                driverClassName="com.mysql.cj.jdbc.Driver"
                url= "jdbc:mysql://localhost/ws?useSSL=false&amp;allowPublicKeyRetrieval=true&amp;serverTimezone=Europe/Madrid"
                username="ws"
                password="ws"
                maxTotal="4"
                maxIdle="2"
                maxWaitMillis="10000"
                removeAbandonedOnBorrow="true"
                removeAbandonedTimeout="60"
                logAbandoned="true"
                testOnBorrow="true"
                validationQuery=""
                validationQueryTimeout="5"/>      
	  ```	
    - Añadir las siguientes líneas al fichero `C:\software\apache-tomcat-11.x.y\conf\context.xml`, 
      dentro de la etiqueta `<Context>`:

      ```shell
      <ResourceLink name="jdbc/ws-javaexamples-ds" global="jdbc/ws-javaexamples-ds"
                type="javax.sql.DataSource"/>      
	  ```	
> NOTA: Para comprobar que Tomcat está correctamente configurado se puede ejecutar el ejemplo `ws-movies`
> siguiendo los pasos del fichero `README.md` que se encuentra en el directorio raíz de los ejemplos.

    
## Configuración básica de Git
> NOTA: Este paso no es necesario si ya se utilizó y configuró Git en otras asignaturas.

> NOTA: `$GIT_HOME` debe sustituirse por la ruta donde se instaló git.

  - Ejecutar git-bash (`$GIT_HOME/git-bash.exe`) y desde ese intérprete de comandos ejecutar:
    
```shell
    git config --global user.email "your_email@udc.es"
    git config --global user.name "Your Name"
```

  - El siguiente comando ilustra como configurar Sublime como editor por defecto de Git, aunque se puede utilizar otro 
    editor instalado en el sistema operativo:
    
```shell
    git config --global core.editor "'C:\Program Files\Sublime Text\sublime_text.exe' -w"
```

## Creación y configuración de claves SSH
> NOTA: Este paso no es necesario si ya utilizó Git en otras asignaturas.

- Desde el intérprete de comandos git-bash ejecutar:

```shell
    ssh-keygen -t rsa -b 4096 -C "your_email@udc.es"
```    

> Genera las claves en la ruta por defecto (%USERPROFILE%/.ssh) y con los nombres  por defecto.

## Añadir clave SSH a GitHub
> NOTA: Este paso no es necesario si ya se utilizó GitHub con SSH en otras asignaturas.
 
- Acceder a [https://github.com/settings/keys](https://github.com/settings/keys).
- Clic en "New SSH Key" para añadir una nueva clave SSH.
- En el campo "Title" ponerle un nombre.
- En el campo "Key" copiar la clave pública, es decir, el contenido del fichero
  `$HOME/.ssh/id_rsa.pub`.
- Clic en "Add SSH key".
- Ejecutar el siguiente comando para comprobar conexión SSH con el servidor de git
  y añadirlo a la lista de hosts conocidos:

```shell
    ssh -T git@github.com
```
> Contestar "yes" a "Are you sure you want to continue connecting (yes/no)?".

## Instalación de una herramienta cliente gráfica para Git (opcional)

- Puede utilizarse cualquier herramienta cliente (https://git-scm.com/downloads/guis).
    
