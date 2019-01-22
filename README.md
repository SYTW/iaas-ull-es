<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Como Desplegar una Aplicación Web en iaas.ull.es](#como-desplegar-una-aplicaci%C3%B3n-web-en-iaasulles)
  - [Acceso al servicio](#acceso-al-servicio)
  - [Panel https://iaas.ull.es/mismaquinas](#panel-httpsiaasullesmismaquinas)
  - [Recuerde estar autenticado en la ULL](#recuerde-estar-autenticado-en-la-ull)
  - [Incidencia en Octubre de 2018](#incidencia-en-octubre-de-2018)
  - [Acceso a https://iaas.ull.es](#acceso-a-httpsiaasulles)
  - [Opciones de Consola](#opciones-de-consola)
  - [Acceso por SSH y Credenciales](#acceso-por-ssh-y-credenciales)
  - [Averiguar la IP de la máquina virtual](#averiguar-la-ip-de-la-m%C3%A1quina-virtual)
  - [Conectarse desde fuera de la ULL: VPN ULL](#conectarse-desde-fuera-de-la-ull-vpn-ull)
  - [Configure la ssh de su portátil/desktop para acceder a su máquina del iaas](#configure-la-ssh-de-su-port%C3%A1tildesktop-para-acceder-a-su-m%C3%A1quina-del-iaas)
  - [Puertos abiertos en las Máquinas Virtuales](#puertos-abiertos-en-las-m%C3%A1quinas-virtuales)
  - [Descripción de las máquinas](#descripci%C3%B3n-de-las-m%C3%A1quinas)
    - [Que suele estar instalado](#que-suele-estar-instalado)
  - [Configure el cliente ssh para trabajar con GitHub](#configure-el-cliente-ssh-para-trabajar-con-github)
  - [Desplegando una aplicación Web en su máquina del iaas](#desplegando-una-aplicaci%C3%B3n-web-en-su-m%C3%A1quina-del-iaas)
    - [Instalando dependencias](#instalando-dependencias)
    - [Ejecución de un servicio](#ejecuci%C3%B3n-de-un-servicio)
    - [Visitando la Aplicación](#visitando-la-aplicaci%C3%B3n)
- [Referencias](#referencias)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Como Desplegar una Aplicación Web en [iaas.ull.es](https://iaas.ull.es)

## Acceso al servicio

Para acceder al servicio, todos los usuarios (tanto los responsables como los usuarios
finales) accederán a [https://iaas.ull.es](https://iaas.ull.es), donde seleccionarán la opción **Portal del usuario**.

## Panel [https://iaas.ull.es/mismaquinas](https://iaas.ull.es/mismaquinas)

En 2018 el STIC ha desplegado un panel accesible en [1], que permite listar las máquinas que son propiedad o que tienen permisos de uso por parte del usuario que se autentica. Tras autenticarse, a dicho usuario le aparecerán las máquinas a las que tiene acceso, conjuntamente con algunos datos, como su estado (encendida, apagada...), el sistema operativo y especialmente las IPs de las tarjetas de red.

Esto sirve para que los usuarios no tengan que entrar al panel de IaaS, conectarse a la máquina virtual y ejecutar algún comando para ver la IP de la máquina para posteriormente conectarse por SSH. Ahora ya les aparecerá en el panel y pueden saber la IP directamente.

Toda esta información ha sido actualizada ya en los documentos correspondientes ([2], [3], [4]).

1. [https://iaas.ull.es/mismaquinas](https://iaas.ull.es/mismaquinas)
2. [https://goo.gl/tAF4am](https://goo.gl/tAF4am)
3. [https://goo.gl/noagxg](https://goo.gl/noagxg)
4. [https://goo.gl/D6Sw7L](https://goo.gl/D6Sw7L)

## Recuerde estar autenticado en la ULL

Recuerde que dentro de la ULL deberá haber **entrado previamente a [https://acceso.ull.es](https://acceso.ull.es)**, esto es estar autenticado como miembro de la ULL  y desde fuera de la ULL usando la VPN de la ULL

## Incidencia en Octubre de 2018

> **¡La IP que aparece en el panel no es la correcta!** Esto es un problema que tienen las Ubuntu 18, y es que
cuando renueva los leases DHCP al parecer no quita las IPs anteriores, con lo
cual las IPs se duplican y varias máquinas tienen la misma IP.

**La solución**:

En la interfaz web deben escribir:

      sudo netplan apply

y después

      ip a

para saber la IP correcta

## Acceso a https://iaas.ull.es

![Autenticandose](ovirtportal.png)

Una vez autenticado le saldrá el menú con su panel de máquinas:

![Panel de las máquinas](panelmaquina.png)

Al pulsar sobre `connect` se abrirá una terminal en el navegador:

![Terminal en el navegador](terminalnavegador.png)

## Opciones de Consola

Si tiene problemas conectándose a la máquina repase las opciones del campo `Console`:

* Se han configurado con el visor VNC (recuerde poner la opción `noVNC` en las
opciones de la cónsola). [VNC](https://en.wikipedia.org/wiki/Virtual_Network_Computing) es un sistema para compartir un desktop gráfico

![Opciones de la Consola](novncconsoleoptions.png)


## Acceso por SSH y Credenciales

* Las credenciales son `usuario/usuario`, se les forzará a cambiar la contraseña ante el primer login.

* Las máquinas tienen IP dinámica por DHCP (en principio tienen keepalive así que no debería cambiarlas)

* Los usuarios tienen disponibilidad para hacer SSH a las máquinas,

    1. Dentro de la ULL **entrando previamente a [https://acceso.ull.es](https://acceso.ull.es)** y
    2. Desde fuera de la ULL usando la VPN


Serán requeridas las credenciales de la ULL; es decir, este servicio será exclusivo para la
comunidad universitaria


## Averiguar la IP de la máquina virtual

* La terminal en el navegador no es útil en absoluto.
* Use la terminal en el navegador para averiguar la IP de la máquina para poder usar `ssh` posteriormente. En la terminal de su navegador conectado a la máquina escriba:

```bash
usuario@SYTW:~/src/sytw$ ifconfig
eth0      Link encaa:Ethernet  direcciónHW aa:aa:aa:aa:aa:aa  
          Direc. inet:55.5.555.55
```

* La IP es el valor que aparece en el campo `inet`
* A partir de ese momento use el cliente `ssh` en su ordenador para conectarse a su máquina virtual

```bash
$ ssh 55.5.555.55
```

Véase la sección incidencia en Octubre de 2018

## Conectarse desde fuera de la ULL: VPN ULL

Tutorial sobre como usar el [Servicio VPN de la ULL](https://usuarios.ull.es/vpn/)

## Configure la ssh de su portátil/desktop para acceder a su máquina del iaas

1. Genere una clave rsa en su portátil (limitación en 2018: tiene que ser rsa)

```bash
~/.ssh(master)]$ ssh-keygen -trsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/casiano/.ssh/id_rsa): miclave
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in miclave.
Your public key has been saved in miclave.pub.
```
2.  Añada una entrada en su fichero `~/.ssh/config`:

```bash
# PL 17/18
# iaas.ull.es
Host pl1718
HostName 11.1.111.111
user usuario
IdentityFile /Users/casiano/.ssh/miclave
## send the signal every four minutes
ServerAliveInterval 240
```
3. Publique la clave en su máquina `iaas`

```bash
usuario@ubuntu:~/.ssh$ cd .ssh/
usuario@ubuntu:~/.ssh$ vi authorized_keys
```
añada los contenidos de `miclave.pub`


## Puertos abiertos en las Máquinas Virtuales

* Creo que ahora mismo las Máquinas Virtuales vienen con estos puertos abiertos:
  -  22, 80, 443 y
  - el rango del 8080 al 8090

## Descripción de las máquinas

* En principio se dispone de una máquina por alumno
* Las máquinas de una misma asignatura comparten el mismo *tag*, por ejemplo  `SISTECWEB` o algo parecido
* Las máquinas tienen 2GB de RAM y 1 procesador de 2 CPUs cada una

### Que suele estar instalado

Lo que está instalado en las máquinas depende del tipo de plantilla
que haya solicitado el profesor para el curso.
De todos modos, como tenemos permisos de administración no es mucho problema instalar cualquier software que nos falte.

Lo que sigue es la descripción de una máquina típica correspondiente
a una plantilla *de servidor*.


* Suele estar instalado `git`:

                  usuario@SYTW:~$ git --version
                  git version 1.9.1

Si queremos actualizar `git`:
```sh
apt-get install git
```

* Suele estar instalado `nodejs`. Recuerde que es Linux Ubuntu: `node` no es  `nodejs`:

            usuario@SYTW:~$ node --version
            El programa «node» puede encontrarse en los siguientes paquetes:
             * node
             * nodejs-legacy
            Intente: sudo apt-get install <paquete seleccionado>

            usuario@SYTW:~$ nodejs --version
            v0.10.25

  Si no está instalado siga las instrucciones en [Installing Node.js via package manager](https://nodejs.org/en/download/package-manager/)
  ```bash
  curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
  sudo apt-get install -y nodejs
  ```
*  *Haga un enlace simbólico* de `node` a `node.js`
  ```bash
  usuario@ubuntu:~$ which nodejs
  /usr/bin/nodejs
  usuario@ubuntu:~$ sudo ln -s /usr/bin/nodejs /usr/bin/node
  usuario@ubuntu:~$ node --version
  v4.2.6
  ```
* O mejor aún instale primero [nvm](https://github.com/creationix/nvm) y luego instale Node.js. nvm es como rvm para NodeJS: Te permite tener varias instalaciones de node y cambiar entre versión y versión

  ```bash
  usuario@ubuntu:~$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
  ```
  El script clona el repositorio de  nvm a `~/.nvm` y añade líneas a nuestro script de arranque de perfil (`~/.bash_profile`, `~/.zshrc`, `~/.profile`, or `~/.bashrc`). Es por eso que tenemos que rearrancar la terminal:

  ```bash
  usuario@ubuntu:~$ command -V nvm
  -bash: command: nvm: no se encontró
  usuario@ubuntu:~$ bash
  usuario@ubuntu:~$ nvm list
              N/A
  node -> stable (-> N/A) (default)
  iojs -> N/A (default)
  ```
  Podemos ahora instalar tantas versiones de node como queramos:
  ```bash
  usuario@ubuntu:~$ nvm install node
  Downloading and installing node v9.4.0...
  usuario@ubuntu:~$ nvm list
  ->       v9.4.0
  default -> node (-> v9.4.0)
  node -> stable (-> v9.4.0) (default)
  stable -> 9.4 (-> v9.4.0) (default)
  iojs -> N/A (default)
  lts/* -> lts/carbon (-> N/A)
  lts/argon -> v4.8.7 (-> N/A)
  lts/boron -> v6.12.3 (-> N/A)
  lts/carbon -> v8.9.4 (-> N/A)
  ```
* [rvm](https://github.com/rvm/ubuntu_rvm)

## Configure el cliente ssh para trabajar con GitHub

* Recuerde poner sus claves privadas de GitHub en `.ssh` para poder trabajar con sus repos

* Actualice `~/.ssh/config` de manera apropiada:

```bash
usuario@ubuntu:~/.ssh$ ssh-keygen -tdsa
...
usuario@ubuntu:~/src/pl$ vi ~/.ssh/config
...
usuario@SYTW:~/src/sytw$ cat ~/.ssh/config
Host github.com
HostName github.com
user git
IdentityFile /home/usuario/.ssh/miclaveparagithub
```

* [Adding a new SSH key to your GitHub account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

## Desplegando una aplicación Web en su máquina del iaas

* ¿Ni idea de como funciona el protocolo HTTP web? Estudia estos vídeos en YouTube:
   - [Basic concepts of web applications, how they work and the HTTP protocol](https://youtu.be/RsQ1tFLwldY) 
   - [Know about HTTP URL](https://youtu.be/ADQ_rhefgEk)
   - [Understanding HTTP Request Response Messages](https://youtu.be/sxiRFwQ1RJ4)
* Cree un directorio de trabajo:

          usuario@SYTW:~$ mkdir src
          usuario@SYTW:~$ cd src
          usuario@SYTW:~/src$ mkdir sytw
          usuario@SYTW:~/src$ cd sytw/

* Clone un repo con una aplicación Web de prueba. Por ejemplo [crguezl/express-start](https://github.com/crguezl/express-start):

          usuario@ubuntu:~/src/PL$ git clone git@github.com:crguezl/express-start.git
          Cloning into 'express-start'...
          The authenticity of host 'github.com (192.30.253.112)' can't be established.
          RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
          Are you sure you want to continue connecting (yes/no)? yes
          Warning: Permanently added 'github.com,192.30.253.112' (RSA) to the list of known hosts.
          Permission denied (publickey).
          fatal: Could not read from remote repository.

          Please make sure you have the correct access rights
          and the repository exists.

  Tienes que añadir una clave a github para poder conectarte.
  En GitHub `settings>SSH and GPG Keys>new SSH key`.
  Una vez hecho:

          usuario@SYTW:~/src/sytw$ git clone git@github.com:crguezl/express-start.git
          Clonar en «express-start»...
          Warning: Permanently added the RSA host key for IP address '555.55.555.555'
                   to the list of known hosts.
          remote: Counting objects: 87, done.
          remote: Total 87 (delta 0), reused 0 (delta 0), pack-reused 87
          Receiving objects: 100% (87/87), 66.93 KiB | 0 bytes/s, done.
          Resolving deltas: 100% (41/41), done.
          Checking connectivity... hecho.

* Este es el contenido de la aplicación de ejemplo [hello/hello.js](https://github.com/crguezl/express-start/blob/master/hello/hello.js):


```javascript
var express = require('express')
var app = express()
var path = require('path');

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

app.use(express.static('public'));

app.get('/', function (req, res) {
  //res.send('Hello World!')
  res.render('index', { title: 'Express' });
})

/*
 var router = express.Router();
  module.exports = router;
*/
app.get('/chuchu', function (req, res) {
  //res.send('Hello Chuchu!')
  res.render('index', { title: 'Chuchu' });
})

app.get('/cat', function (req, res) {
  res.send('Got a GET request'+
    '<br/><img src="kitten.jpg" />'
  );
})

app.get('/dog', function (req, res) {
  res.sendFile(path.join(__dirname, 'public/dog.jpg'));
});

var server = app.listen(8080, function () {

  var host = server.address().address
  var port = server.address().port

  console.log('Example app listening at http://%s:%s', host, port)

})
```

Cambiamos port de escucha a 80:

```bash
usuario@SYTW:~/src/sytw$ cd express-start/
usuario@SYTW:~/src/sytw/express-start$ cd hello/

usuario@SYTW:~/src/sytw/express-start/hello$ vi hello.js
usuario@SYTW:~/src/sytw/express-start/hello$ # cambiamos port de escucha a 80

usuario@SYTW:~/src/sytw/express-start/hello$ cat hello.js
```

### Instalando dependencias

* Instalamos las dependencias:

            usuario@SYTW:~/src/sytw/express-start/hello$ npm install
            npm WARN package.json hello@0.0.0 No description
            npm WARN package.json hello@0.0.0 No repository field.
            ...

### Ejecución de un servicio

* Ejecutamos (El `sudo` es necesario porque estamos usando el puerto 80):

```bash
usuario@ubuntu:~/src/pl/express-start/hello$ which node
/home/usuario/.nvm/versions/node/v9.4.0/bin/node
usuario@ubuntu:~/src/pl/express-start/hello$ sudo /home/usuario/.nvm/versions/node/v9.4.0/bin/node hello.js
Example app listening at http://:::80
```

* Usamos el comando unix [`nohup`](http://linux.101hacks.com/unix/nohup-command/)
si queremos evitar que el programa termine cuando hagas logout
```bash
usuario@ubuntu:~/src/express-start/hello$ nohup sudo -b node hello.js
```
La opción `-b` de [`sudo`](https://www.sudo.ws/man/1.8.13/sudo.man.html) ejecuta el comando en background.
Ahora podemos salir de la cuenta y el servicio continúa ejecutandose.

### Visitando la Aplicación

* A continuación visitamos la página con el navegador usando la URL con la IP:

![Visitando con el navegador la página](browser.png)

# Referencias

* El tutorial [SERVICIO IAAS de la ULL](https://casianorodriguezleon.gitbooks.io/ull-esit-1617/content/recursos/iaas.html)

* Véase [este documento como HTML](https://sytw.github.io/iaas-ull-es/index.html)

* Véase este [repo en GitHub](https://github.com/SYTW/iaas-ull-es)

* Lea el documento ["Manual de administración de Pools de máquinas"](manualDeAdministracionDelPoolsDeMaquinas.pdf).
* Tutorial sobre como usar el [Servicio VPN de la ULL](https://usuarios.ull.es/vpn/)
