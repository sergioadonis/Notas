# Manejar multiples entornos y versiones de NodeJS con NVM
NodeJS es un entorno de programación que ha ido avanzando muy rapidamente, y 
regularmente se han lanzado nuevas versiones.

Al día de hoy según el sitio oficial https://nodejs.org/es/ la versión LTS 
(Long Time Support) es la versión 10, es la versión "recomendada", pero ya está
también la versión 11 con las últimas características.

Muchos tienen proyectos funcionales con la versión 8 o con la versión 6. 
Lo importante es que en un proyecto todos los participantes tengan la versión 
exacta del entorno, además, que coincida con el entorno de producción para 
evitar comportamientos raros. 

Otra situación es cuando nos dan un entorno de programación con una versión que 
no es la exacta. Por ejemplo al día de hoy (01-enero-18) cuando creamos un IDE
en AWS Cloud9, este viene con la versión 6, pero para muchos proyectos la versión
mínima es la versión 8.

Podemos verificar la versión de nodejs con el siguiente comando en la terminal:
````
sa:~/environment $ node --version
v6.15.0
````

Para manejar las versiones de NodeJS existe una herramienta conocida como NVM, 
Node Version Manager, la cual se puede encontrar en este sitio 
https://github.com/creationix/nvm. Es muy útil para tener varios entornos de NodeJS
en la misma máquina y evitarnos la necesidad de virtualizar para aislar los paquetes
que vayamos instalando con npm.


## Instalar NVM
Se puede instalar con el siguiente comando en la terminal:
````
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
````
Este comando descarga un script bash que a su vez descarga nvm desde un repositorio
de git. En algunas ocasiones hay que cerrar y abrir la terminal para que se vea 
reflejado la nueva herramienta.

Para verificar que se ha instalado correctamente podemos ejecutar el siguiente comando
en la terminal:
```
sa:~/environment $ nvm --version
0.31.7
```
## Comandos básicos de NVM
Una vez que tengamos instalado esta herramienta, podemos comenzar a usarla de 
distintas formas, acá las que me han sido de utilidad:
Instalar la útlima versión de Node:
```
sa:~/environment $ nvm install node
Downloading https://nodejs.org/dist/v11.6.0/node-v11.6.0-linux-x64.tar.xz...
######################################################################## 100.0%
Now using node v11.6.0 (npm v6.5.0-next.0)
```
Instalar una versión específica de node, por ejemplo la versión 8.10:
```
sa:~/environment $ nvm install 8.10
Downloading https://nodejs.org/dist/v8.10.0/node-v8.10.0-linux-x64.tar.xz...
######################################################################## 100.0%
Now using node v8.10.0 (npm v5.6.0)
```
Verificar que versión estamos usando:
```
sa:~/environment $ node --version
v8.10.0
```
Ver las diferentes versiones instaladas:
```
sa:~/environment $ nvm ls
        v6.15.0  
->      v8.10.0  
        v11.6.0  
         system  
default -> 6 (-> v6.15.0)
node -> stable (-> v11.6.0) (default)
stable -> 11.6 (-> v11.6.0) (default)
iojs -> N/A (default)
lts/* -> lts/argon (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.16.0 (-> N/A)
lts/carbon -> v8.15.0 (-> N/A)
lts/dubnium -> v10.15.0 (-> N/A)
```
Activar una versión distinta:
```
sa:~/environment $ nvm use 11.6.0
Now using node v11.6.0 (npm v6.5.0-next.0)
sa:~/environment $ node --version
v11.6.0
```
Hay que tener en cuenta que cuando iniciemos nuestra terminal, esta iniciará con
la versión por defecto de NodeJS.
Podemos verificar la versión por defecto con el siguiente comando:
```
sa:~/environment $ nvm run default node --version
Running node v6.15.0 (npm v3.10.10)
```
*default* solo es un alias en nvm, podemos cambiarlo para que siempre nuestra 
terminal siempre inicie con una versión específica con el siguiente comando:
```
sa:~/environment $ nvm alias default 8.10.0
default -> 8.10.0 (-> v8.10.0)
```
Hay que tener en cuenta que cuando tengamos una versión activa e instalemos 
paquetes con npm, estos no estarán disponibles para los otros entornos, pero esa
es la idea tener un entorno completamente aislado uno del otro.

