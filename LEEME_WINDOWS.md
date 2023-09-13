# Instalación / Configuración entorno ISD / 2023-2024 - Windows
-------------------------------------------------------------------------------

## Descargar e instalar el SW

> NOTA: Se recomienda utilizar un usuario de Windows sin espacios en el nombre 
  para evitar problemas con Maven.

- Descargar y descomprimir en `C:\software` el siguiente software
    - Maven 3.9.x o superior 
        + https://maven.apache.org/download.cgi
        + Descargar el "Binary zip archive"
    - Tomcat 10.x.y 
      + https://tomcat.apache.org/download-10.cgi
      + En el apartado "Binary Distributions" / "Core" descargar el zip.

- Descargar e instalar la última versión LTS de Eclipse Temurin (JDK 17)
    - https://adoptium.net
    - Descargar el instalador .msi para Windows e instalar usando las opciones por defecto.

- Descargar e instalar IntelliJ IDEA
    - https://www.jetbrains.com/es-es/idea/download
        + Se puede utilizar la versión Community (libre) o la versión Ultimate 
          (solicitando una licencia para estudiantes según se indica en 
          https://www.jetbrains.com/es-es/community/education/#students). 
    - Instalar usando las opciones por defecto.
	 
- Descargar e instalar MySQL 8.0.x:
    - https://dev.mysql.com/downloads/mysql/
        + Descargar el instalador .msi para Windows de la versión 8.0.x
    - Instalar en la ruta por defecto.
    - Elegir "Server only" o "Custom" (para instalar Server + Workbench) y usar 
     las opciones por defecto.
    - Después de la instalación, se ejecutará el wizard de Configuración de 
     MySQL Server. 
         + Utilizar las opciones por defecto excepto la contraseña para el 
         usuario `root`, que no debe dejarse vacía (puede usarse, por ejemplo, 
         `root`)
         + Comprobar que la opción "Start the MySQL Server at System Startup"
         está marcada, para que se instale como servicio Windows.

- Descargar e instalar el compilador de Apache Thrift:
     - http://www.apache.org/dyn/closer.cgi?path=/thrift/0.19.0/thrift-0.19.0.exe
     - Renombrar el ejecutable a thrift.exe y copiarlo dentro del directorio bin que hay 
     dentro del directorio donde se descomprimió Maven 
     
## Descargar y descomprimir los ejemplos de la asignatura 

> Disponibles en moodle

- Descargar en `C:\software`
  
## Establecer variables de entorno

- Ir a "Panel de Control > Sistema > Configuración avanzada del sistema > Variables de entorno ..."

- En la sección "Variables de usuario para `<user>`", crear las siguientes
  variables de entorno (para cada una pulsar en "Nueva ...", introducir el 
  nombre y el valor, y pulsar "Aceptar")
    - Nombre: `JAVA_HOME`
        + Valor: Directorio donde se instaló Eclipse Temurin (JDK 17)
        + Por ejemplo:`C:\Program Files\Eclipse Adoptium\jdk-17.0.8.101-hotspot`
    - Nombre: `MAVEN_HOME`
        + Valor: Directorio donde se descomprimió Maven
        + Por ejemplo: `C:\software\apache-maven-3.9.4`
    - Nombre: `MAVEN_OPTS`
        + Valor: `-Xms512m -Xmx1024m`
    - Nombre: `MYSQL_HOME`
        + Valor: Directorio donde se instaló MySQL
        + Por ejemplo: `C:\Program Files\MySQL\MySQL Server 8.0`

- En la sección "Variables de usuario para `<user>`", modificar la variable de
  entorno `PATH`. Para ello hay que seleccionarla, pulsar en "Editar..." y 
  añadir al principio de su valor (sin borrar su valor antiguo):
  
  `%JAVA_HOME%\bin;%MAVEN_HOME%\bin;%MYSQL_HOME%\bin;`
  
> NOTA: Si la variable de entorno PATH no existiese, entonces habría que 
    crearla procediendo de igual forma que se hizo con las variables anteriores.
    
- Cerrar todos los terminales y abrir terminales nuevos

- Comprobar que el entorno ha quedado correctamente configurado comprobando 
  salidas de los siguientes comandos:
  
```shell 
	java -version
	mvn -version
	mysqld --version
```

## Creación de bases de datos necesarias para los ejemplos
- Arrancar MySQL
  - Si se ha instalado como servicio seguramente se haya iniciado de forma 
    automática. En otro caso habría que iniciar el servicio manualmente.
    
> NOTA: En Panel de Control, Servicios Locales se puede configurar arranque 
  automático o manual. También se puede arrancar y detener.
           
> NOTA: En los siguientes pasos, al ejecutar los comandos  `mysqladmin` y `myqsl` 
  con la opción `-p` la password que nos solicitarán es la del usuario
  root que se especificó al instalar MySQL.

- Creación de bases de datos ws y wstest (ejecutar desde una consola)

```shell
    mysqladmin -u root create ws -p
    mysqladmin -u root create wstest -p
```

- Creación de usuario ws con password con permisos sobre ws y wstest

```shell
    mysql -u root -p
        CREATE USER 'ws'@'localhost' IDENTIFIED BY 'ws';
        GRANT ALL PRIVILEGES ON ws.* to 'ws'@'localhost' WITH GRANT OPTION;
        GRANT ALL PRIVILEGES ON wstest.* to 'ws'@'localhost' WITH GRANT OPTION;
        exit
```

- Comprobar acceso a BD

```shell
    mysql -u ws --password=ws ws
        exit

    mysql -u ws --password=ws wstest
        exit
```

## Inicialización de datos de ejemplo y compilación de los ejemplos

- Inicialización de la base de datos y compilación de los ejemplos

```shell
    cd C:/software/ws-javaexamples-3.7.0
    mvn sql:execute install
```
	
## Configuración de IntelliJ IDEA
- Se recomienda instalar el plugin de Thrift (lo sugerirá el editor al abrir un fichero .thrift)
- Se recomienda configurar "Command Prompt" como el terminal por defecto. Se puede hacer en el wizard 
  `File > Settings > Tools > Terminal` seleccionando `C:\Windows\system32\cmd.exe` como valor de `Shell path`


## Configuración de Tomcat

- Copiar el driver JDBC de MySQL al directorio `C:\software\apache-tomcat-10.x.y\lib`
    - El driver JDBC se puede obtener de la siguiente ruta (siempre y cuando se hayan compilado previamente 
    los ejemplos):
     `%HOME%\.m2\repository\com\mysql\mysql-connector-j\8.0.33\mysql-connector-j-8.0.33.jar`

- Definir un data source con nombre `jdbc/ws-javaexamples-ds`
    - Añadir las siguientes líneas al fichero `C:\software\apache-tomcat-10.x.y\conf\server.xml`, 
      dentro de la etiqueta `<GlobalNamingResources>`
 
      ```shell
      <!-- MySQL -->
      <Resource name="jdbc/ws-javaexamples-ds"
                auth="Container"
                type="javax.sql.DataSource"
                driverClassName="com.mysql.jdbc.Driver"
                url= "jdbc:mysql://localhost/ws?useSSL=false&amp;allowPublicKeyRetrieval=true&amp;serverTimezone=Europe/Madrid"
                username="ws"
                password="ws"
                maxActive="4"
                maxIdle="2"
                maxWait="10000"
                removeAbandoned="true"
                removeAbandonedTimeout="60"
                logAbandoned="true"
                validationQuery="SELECT 1"/>
	  ```	
    - Añadir las siguientes líneas al fichero `C:\software\apache-tomcat-10.x.y\conf\context.xml`, 
      dentro de la etiqueta `<Context>`

      ```shell
      <ResourceLink name="jdbc/ws-javaexamples-ds" global="jdbc/ws-javaexamples-ds"
                type="javax.sql.DataSource"/>      
	  ```	
> NOTA: Para comprobar que Tomcat está correctamente configurado se puede ejecutar el ejemplo `ws-movies`
>siguiendo los pasos del fichero `README.md` que se encuentra en el directorio raíz de los ejemplos

    
## Instalación y configuración básica de Git
> NOTA: Este paso no es necesario si ya se utilizó y configuró Git en otras asignaturas

- Descargar e instalar Git
    - https://git-scm.com/downloads
    - Hacer clic en "Windows" para descargar.
    - Instalar con las opciones por defecto.

- Configuración básica

> NOTA: `$GIT_HOME` debe sustituirse por la ruta donde se instaló git.

    - Ejecutar git-bash (`$GIT_HOME/git-bash.exe`) y desde ese intérprete de comandos ejecutar:
    
```shell
    git config --global user.email "your_email@udc.es"
    git config --global user.name "Your Name"
```

> El siguiente comando ilustra como configurar Sublime como editor por defecto de Git, aunque se puede utilizar otro editor instalado en el sistema operativo.
    
```shell
    git config --global core.editor "'C:\Program Files\Sublime Text\sublime_text.exe' -w"
```

## Creación y configuración de claves SSH
> NOTA: Este paso no es necesario si ya utilizó Git en otras asignaturas

- Desde el intérprete de comandos git-bash ejecutar:

> Genera las claves en la ruta por defecto (%USERPROFILE%/.ssh) y con los nombres  por defecto 
      
```shell
    ssh-keygen -t rsa -b 4096 -C "your_email@udc.es"
```    
## Añadir clave SSH a GitHub
> NOTA: Este paso no es necesario si ya se utilizó GitHub con SSH en otras asignaturas
> 
- Acceder a [https://github.com/settings/keys](https://github.com/settings/keys).
- Clic en "New SSH Key" para añadir una nueva clave SSH.
- En el campo "Title" ponerle un nombre.
- En el campo "Key" copiar la clave pública, es decir, el contenido del fichero
  `$HOME/.ssh/id_rsa.pub`
- Clic en "Add SSH key".

- Comprobar conexión SSH con el servidor de git y añadirlo a la lista de hosts
  conocidos

> Contestar "yes" a "Are you sure you want to continue connecting (yes/no)?"

```shell
    ssh -T git@github.com
```

## Instalación de una herramienta cliente gráfica para Git (opcional)

- Puede utilizarse cualquier herramienta cliente (https://git-scm.com/downloads/guis)
    
