# Clima-mysql
Este repositorio contiene un flow en NodeRed el cual esta basado en el flow5 y además guarda información recopilada en una base de datos

## Introducción

El flow 6 es un ejercicio hecho en Node-Red usando una base de datos para almacenar información, usando Mysql.

## Material Necesario

Para realizar este flow necesitas lo siguiente:

- [Ubuntu 20.04](https://releases.ubuntu.com/20.04/)
- [NodeJS](https://nodejs.org/es/)
    - NPM
    - NodeRed
    - Nodos Dashboard
    - Cuenta de OpenWeather

## Material de referencia

En los siguientes enlaces puedes encontrar cursos en la plataforma de edu.codigoiot.com que te permitirán realiar las configuraciones necesarias

- [Instalación de Virutal Box y Ubuntu 20.04](https://edu.codigoiot.com/course/view.php?id=812)
- [Instalación de NodeRed](https://edu.codigoiot.com/course/view.php?id=817)
- [Introducción a NodeRed](https://edu.codigoiot.com/course/view.php?id=278)
- [Introducción a MQTT](https://edu.codigoiot.com/enrol/index.php?id=851)
- [API de OpenWeather](https://openweathermap.org/api)

## Instrucciones

### Requisitos previos

Para que este flow funcione, debes cumplir con los siguientes requisitos previos

1. Instalación de NodeJS. Se recomienda tener instalado NodeJS en alguna versión LTS. Al momento de creación de este documento, se usó la versión 18.12LTS. Esta instalación debe incluir las Build-Tools para hacer uso de NPM
2. Instalación de NodeRed. La instalación se realiza por NPM. Al momento de la creación de este contenido, se usó la versión 3.0.2
3. Cuenta de OpenWeather donde tendremos información 
4. Coordenadas de un lugar.
5. Iniciar grafana con la instrucción sudo /bin/systemctl start grafana-server

### Instrucciones de preparación del entorno

1. Arrancar NodeRed con el el comando node-red
2. Clonar el flow 5 
3. Agregar los nodos siguientes 
- 1 nodo TimeStamp
- 1 nodo Function
- 1 nodo MySql
4. Para agregar el nodo MySql debemos ir a la pestaña "Manage palatte" menú derecho, e instalar node-red-node-mysql
5. En el nodo function debemos agregar el siguiente codigo:

msg.topic = "INSERT INTO clima (`nombre`, `temperatura`,`humedad`) VALUES ('diana'," + global.get("tempAPI") + "," + global.get("humAPI") + ");"; return msg;

### Instrucciones para la creación de la Base de Datos
1. Instalar MySql -> `sudo apt install mysql-server`
2. Entrar a MySql -> `sudo mysql`
3. Crear una Base de Datos ->  `create databases CodigoIoT;`
4. Seleccionar la Base de Datos creada -> `use CodigoIoT;`
5. Crear una tabla que tenga los siguientes datos: id, fecha, nombre, temperatura y humedad con el siguiente comando: 

`create table clima (id INT (6) UNSIGNED AUTO_INCREMENT PRIMARY KEY, fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP, nombre CHAR (248) NOT NULL, temperatura FLOAT (4,2), humedad INT (3));; `

6. Crear un usuario y contraseña nuevo para usarlo en Node Red
`CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';;` 
7. Para otorgar permisos de usuario 
`;GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';;` 

### Notas 
- Para consultar la Base de Datos usa el comando  `show databases;`
- Para consultar la tabla en el interior de una base de datos seleccionada con el comando  `show tables; ;`
- Consultar la forma de la tabla con  `describe clima;`
- Y para consultar todos los datos de la tabla utiliza  `SELECT * FROM clima;`

### Instrucciones para instalar grafana
`sudo apt-get install -y adduser libfontconfig1`

`wget https://dl.grafana.com/enterprise/release/grafana-enterprise_9.1.3_amd64.deb`

`sudo dpkg -i grafana-enterprise_9.1.3_amd64.deb`

### Instrucciones de operación

1. Eliminar el nodo TimeStamp de la base de datos agregar cuatro nodos template.
2. Agregar el siguiente codigo al nodo function:
`msg.topic = "INSERT INTO clima (`nombre`,`temperatura`,`humedad`) VALUES ('" + msg.payload.id + "'," + msg.payload.temp + "," + msg.payload.hum + ");"; return msg;`
3. Abrir el navegador y colocar en la URL localhost:3000
4. Iniciar sesión por primera vez: usuario admin y modificar la contraseña. 
5. Agregar la base de datos desde configuración en la pestaña Data Source:
- Seleccionar MySql
- Host: localhost:3306
- Database: Nombre de la base creada (clima)
- User & Password: Nombre y contraseña que se crearon
- TimeZone: Coordinated Universal Time y guardar con Save&test asegurarse que se haya conectado exitosamente.
6. Agregar un panel que se llame temperatura y la segunda humedad.
7. Información personal agregar otro panel y en la opcion WHERE indicar solo la humedad de un usuario. 
8. Modificar el archivo de grafana.ini donde se encuentra en la dirección /etc/grafana se abre terminal para ejecutar `sudo nano grafana.ini` donde llegaremos a la opción security y modificar `;allow_embedding = false a allow_embedding = true` guardar cambios y reiniciar grafana  con `sudo /bin/systemctl restart grafana-server`
9. Copiar la dirección de la grafica dando clic en share, embed y pegarlo en el template de node red, dar clic y en Deploy e ir al navegador y colocar localhost:1880/ui para ver los resultados. 

## Resultados

A continuación puede verse una vista previa de los resultado de este flow.

![](https://github.com/Cecilia-X-M/Clima-mysql/blob/main/Diagrama-bd.png)
![](https://github.com/Cecilia-X-M/Clima-mysql/blob/main/InfBD.png)
![](https://github.com/Cecilia-X-M/Clima-mysql/blob/main/templates.png)
![](https://github.com/Cecilia-X-M/Clima-mysql/blob/main/Captura%20de%20pantalla%20(109).png)

# Créditos

Desarrollado por Diana Laura Aragon 

- [GitHub](https://github.com/LauraGx)
