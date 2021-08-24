# Instalación / Configuración entorno ISD / 2020-2021 - Linux y macOS
-------------------------------------------------------------------------------

## Descargar e instalar el SW
  
- [Linux] 
    - Descargar y descomprimir en `$HOME/software` el siguiente software
        - Maven 3.8.x o superior 
            - https://maven.apache.org/download.cgi
            - Descargar el "Binary tar.gz archive".
        - IntelliJ IDEA
            - https://www.jetbrains.com/es-es/idea/download
                + Se puede utilizar la versión Community (libre) o la versión Ultimate
                  (solicitando una licencia para estudiantes según se indica en
                  https://www.jetbrains.com/es-es/community/education/#students).
    - Instalar como paquete
        - AdoptOpenJDK 11
            - Instalar como paquete siguiendo las instrucciones que se 
              indican en la sección "Linux RPM and DEB installer packages" de 
              https://adoptopenjdk.net/installation.html.
            - Instalar la version "Open JDK 11 (LTS)" con la JVM "Hotspot"
              (adoptopenjdk-11-hotspot).
        - MySQL 8
            - Seguir las instrucciones que se indican en 
              https://dev.mysql.com/doc/refman/8.0/en/linux-installation.html
    - Instalar el compilador de Apache Thrift
        > NOTA: La versión recomendada es la 0.14.2, pero se puede instalar cualquier versión
          comprendida entre la 0.9.1 y la 0.14.2
        - Instalarlo como paquete si está disponible para la distribución Linux utilizada
            - Ubuntu 20.04, 18.04, 16.04
               - sudo apt-get update -y
               - sudo apt-get install -y thrift-compiler 
            - Alpine, ALT Linux, Arch Linux, CentOS, Fedora, FreeBSD, Mageia, NetBSD, openSUSE, Slackware
               - Descargar paquete de https://pkgs.org/download/thrift e instalarlo
            - Debian
               - Descargar paquete de https://packages.debian.org/sid/thrift-compiler e instalarlo
        - En otro caso, seguir las instrucciones de https://thrift.apache.org/docs/install/
        - IMPORTANTE: Si no se ha instalado la versión 0.14.2 (para saber la versión instalada basta con ejecutar
          `thrift -version`) es necesario crear o modificar el fichero `$HOME/.m2/settings.xml` para indicar la 
          versión de Thrift que se va a utilizar. Por ejemplo, para la versión 0.13.0, el fichero debería tener el
          siguiente contenido (en caso de que el fichero ya exista, hay que añadir la etiqueta `<activeProfiles>` 
          dentro de la etiqueta `<settings>`):   

            ```shell
            <settings>
                <activeProfiles>
                    <activeProfile>thrift-0.13.0</activeProfile>
                </activeProfiles>
            </settings>
            ```

- [macOS] 
    - Descargar y descomprimir en `$HOME/software`
        - Maven 3.8.x o superior 
            - https://maven.apache.org/download.cgi
            - Descargar el "Binary tar.gz archive"
    - Deescargar e instalar
        - IntelliJ IDEA
            - https://www.jetbrains.com/es-es/idea/download
                + Se puede utilizar la versión Community (libre) o la versión Ultimate
                  (solicitando una licencia para estudiantes según se indica en
                  https://www.jetbrains.com/es-es/community/education/#students).
            - Instalar usando las opciones por defecto.
        - AdoptOpenJDK 11
            - https://adoptopenjdk.net/
            - Seleccionar la version "Open JDK 11 (LTS)" y la JVM "Hotspot".
            - Descargar el instalador .pkg para macOS e instalar usando las opciones por defecto.
        - MySQL 8
            - https://dev.mysql.com/downloads/mysql/
            - Descargar el instalador .dmg para macOS
            - Instalar con las opciones por defecto.
            - Preferencias del sistema -> MySQL -> Elegir "Start MySQL when your computer starts up".
            - Más información: https://dev.mysql.com/doc/refman/8.0/en/osx-installation.html
    - Instalar como paquete el compilador de Apache Thrift:
        - Se puede instalar de forma sencilla usando cualquiera de los dos siguientes gestores de paquetes: MacPorts o Homebrew.
        - Si no tienes ninguno de estos gestores de paquetes en tu macOS, instala uno de ellos (el que prefieras):
            - Instalación de MacPorts: https://www.macports.org/install.php.
            - Instalación de Homebrew: https://brew.sh/index_es.
        - Instalación de Thrift con MacPorts:
            - sudo port install thrift
        - Instalación de Thrift con Homebrew:
            - brew install thrift
        - IMPORTANTE: Si no se ha instalado la versión 0.14.2 (para saber la versión instalada basta con ejecutar
          `thrift -version`) es necesario crear o modificar el fichero `$HOME/.m2/settings.xml` para indicar la
          versión de Thrift que se va a utilizar. Por ejemplo, para la versión 0.13.0, el fichero debería tener el
          siguiente contenido (en caso de que el fichero ya exista, hay que añadir la etiqueta `<activeProfiles>`
          dentro de la etiqueta `<settings>`):

            ```shell
            <settings>
                <activeProfiles>
                    <activeProfile>thrift-0.13.0</activeProfile>
                </activeProfiles>
            </settings>
            ```

- [Linux y macOS] Descargar y descomprimir en `$HOME/software` el siguiente software
    - Tomcat 9.x 
        + https://tomcat.apache.org/download-90.cgi
        + En el apartado "Binary Distributions" / "Core" descargar el .tar.gz.
         
## Descargar y descomprimir los ejemplos de la asignatura
- Descargar en `$HOME/software`

> Disponibles en moodle

```shell
    cd $HOME/software
    tar zxf ws-javaexamples-3.5.0-src.tar.gz
```

## [Linux] Establecer variables de entorno
- Añadir al fichero `$HOME/.bashrc` lo siguiente 

> NOTA: Los valores de las variables MAVEN_HOME, IDEA_HOME y JAVA_HOME deben sustituirse por los 
  directorios donde se haya descomprimido Maven e IntelliJ IDEA, e instalado AdoptOpenJDK, respectivamente

```shell
    # AdoptOpenJDK (Linux)
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

    PATH=$JAVA_HOME/bin:$PATH

    # Maven
    MAVEN_HOME=$HOME/software/apache-maven-3.8.1
    PATH=$MAVEN_HOME/bin:$PATH
    export MAVEN_OPTS="-Xms512m -Xmx1024m"

    # IntelliJ IDEA
    IDEA_HOME=$HOME/software/idea
    PATH=$IDEA_HOME/bin:$PATH
```

- Cerrar todos los terminales y abrir terminales nuevos

- Comprobar que el entorno ha quedado correctamente configurado comprobando 
  las salidas de los siguientes comandos

```shell
    which java
    which mvn
    which idea.sh
```

## [macOS] Establecer variables de entorno
> NOTA: asumiendo que la aplicación Terminal use el shell de inicio de sesión,
  éste será "zsh" (macOS 10.15 o superior) o "bash" (versiones anteriores de macOS).

- Añadir al fichero `$HOME/.zshrc` (macOS 10.15 o superior) o 
  `$HOME/.bash_profile` (versiones anteriores de macOS) lo siguiente:

> NOTA: Los valores de las variables MAVEN_HOME y JAVA_HOME deben sustituirse por los 
  directorios donde se haya descomprimido Maven e instalado AdoptOpenJDK respectivamente

```shell
    # AdoptOpenJDK (macOS)
    export JAVA_HOME=/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home

    PATH=$JAVA_HOME/bin:$PATH

    # Maven
    MAVEN_HOME=$HOME/software/apache-maven-3.8.1
    PATH=$MAVEN_HOME/bin:$PATH
    export MAVEN_OPTS="-Xms512m -Xmx1024m"

    # MySQL.
    MYSQL_HOME=/usr/local/mysql
    PATH=$MYSQL_HOME/bin:$PATH
```

- Cerrar todos los terminales y abrir terminales nuevos

- Comprobar que el entorno ha quedado correctamente configurado comprobando 
  las salidas de los siguientes comandos

```shell
    which java
    which mvn
```
    
## Creación de bases de datos necesarias para los ejemplos

- Arrancar MySQL (sólo si el arranque no es automático)

```shell
    mysqld
```

> NOTA: En los siguientes pasos, al ejecutar los comandos  `mysqladmin` y `myqsl` 
  con la opción `-p` la password que nos solicitarán es la del usuario
  root que se especificó al instalar MySQL.

- Creación de bases de datos ws y wstest (abrir en una consola diferente)

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
    cd $HOME/software/ws-javaexamples-3.5.0
    mvn sql:execute install
```
    
## Finalizar la ejecución de la BD

- Finalizar la ejecución de la BD (sólo si el arranque no es automático)

```shell
    mysqladmin -u root shutdown
```

## Configuración de IntelliJ IDEA
- Se recomienda instalar el plugin de Thrift (lo sugerirá el editor al abrir un fichero .thrift)


## Configuración de Tomcat
> NOTA: Se asume que Tomcat está descomprimido en el directorio `$HOME/software/apache-tomcat-9.0.x`

- Copiar el driver JDBC de MySQL al directorio `$HOME/software/apache-tomcat-9.0.x/lib`
    - El driver JDBC se puede obtener de la siguiente ruta (siempre y cuando se hayan compilado previamente los ejemplos):
     `$HOME/.m2/repository/mysql/mysql-connector-java/8.0.20/mysql-connector-java-8.0.20.jar` 

- Definir un data source con nombre `jdbc/ws-javaexamples-ds`
    - Añadir las siguientes líneas al fichero `$HOME/software/apache-tomcat-9.0.x/conf/server.xml`, 
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
    - Añadir las siguientes líneas al fichero `$HOME/software/apache-tomcat-9.0.x/conf/context.xml`, 
      dentro de la etiqueta `<Context>`

      ```shell
      <ResourceLink name="jdbc/ws-javaexamples-ds" global="jdbc/ws-javaexamples-ds"
                type="javax.sql.DataSource"/>      
	  ```	
> NOTA: Para comprobar que Tomcat está correctamente configurado se puede ejecutar el ejemplo `ws-movies`
>siguiendo los pasos del fichero `README.md` que se encuentra en el directorio raíz de los ejemplos

      
## Instalación y configuración básica de Git
---------------------------------------------------------------------
> NOTA: Este paso no es necesario si ya se utilizó y configuró Git en otras asignaturas

- Instalación en Linux
    - https://git-scm.com/downloads
    - Hacer clic en "Linux/Unix" y seguir las instrucciones según la distribución de linux utilizada.
     
- Instalación en macOS
    - https://git-scm.com/downloads
    - Hacer clic en "MacOs". En la siguiente pantalla hacer clic en "installer"
      dentro de la sección "Binary Installer" y descargar la última versión del instalador .dmg.
    - Instalar con las opciones por defecto.

- Configuración básica (Linux y macOS)

```shell
    git config --global user.email "your_email@udc.es"
    git config --global user.name "Your Name"
```

> El siguiente comando ilustra como configurar Sublime como editor por defecto de Git, aunque se puede utilizar otro editor instalado en el sistema operativo.

```shell
    git config --global core.editor "subl -w"
```

- (Opcional) Instalación de utilidad de autocompletado para Git
    - Seguir las instrucciones indicadas en https://github.com/bobthecow/git-flow-completion/wiki/Install-Bash-git-completion

## Creación y configuración de claves SSH
> NOTA: Este paso no es necesario si ya utilizó Git en otras asignaturas

- Desde un terminal ejecutar:

> Generar las claves en la ruta por defecto ($HOME/.ssh) y con los nombres 
  por defecto
       
```shell
    ssh-keygen -t rsa -b 4096 -C "your_email@udc.es"
```

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
