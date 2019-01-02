# Agregar clave SSH a Github
31-DIC-2018

Cuando se intenta enviar o recibir código de un repositorio de Github, podemos 
tener problemas con la autenticación. Por ejemplo, ejecutando el siguiente 
comando en la terminal:
````
sa:~/environment/Notas (master) $ git remote add origin git@github.com:sergioadonis/Notas.git
sa:~/environment/Notas (master) $ git push -u origin master
The authenticity of host 'github.com (192.30.253.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
RSA key fingerprint is MD5:16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.253.112' (RSA) to the list of known hosts.
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
````
Esto se debe a que el remote configurado usa el protocolo SSH y no hemos configurado
nuestras claves en el servidor.

## Generar clave SSH 
Primeramente debemos generar nuestras claves SSH en la computador desde la cual 
vamos a ingresar, ejecutando el siguiente comando en la terminal:
````
sa:~/environment/Notas (master) $ ssh-keygen -t rsa -b 4096 -C "sergio.adonis@hotmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ec2-user/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ec2-user/.ssh/id_rsa.
Your public key has been saved in /home/ec2-user/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:60X5jn4TWFQpjLFLCpqUgdz0Cnk5IXkeFoAU2+tI1vs sergio.adonis@hotmail.com
The key's randomart image is:
+---[RSA 4096]----+
|.=+*=.    .+ ... |
|. *o=*    ..+ .  |
| .o=*.o   o. .   |
|  .+o= . o o.    |
| o o+   S +o     |
|o o .    o...    |
| . o    . . ..   |
|    .  . . oo    |
|     E  ..o...   |
+----[SHA256]-----+
````

## Guardar la clave en Github
Posteriormente vamos a guardar la clave recién creada en el sitio de Github, con
esto le indicamos que la computadora que contenga la clave está autorizada para
enviar comandos con nuestra identidad.

Para guardar la clave, entramos al sitio de Github e iniciamos sesión. Una vez 
iniciada la sesión vamos a Settings y buscamos la sección SSH and GPG keys:
https://github.com/settings/keys

Una vez en dicha sección hacemos clic en el botón New SSH key.
En el formulario ingresamos un título y copiamos el contenido de la clave 
pública, el cual podemos obtener con el siguiente comando en la terminal:
````
sa:~/environment/Notas (master) $ cat ~/.ssh/id_rsa.pub 
````
Damos clic en el botón Add SSH key.

## Probar
Probamos el comando que nos dio error en la sección inicial.
````
sa:~/environment/Notas (master) $ git push -u origin master
Warning: Permanently added the RSA host key for IP address '192.30.253.113' to the list of known hosts.
Counting objects: 3, done.
Writing objects: 100% (3/3), 219 bytes | 219.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:sergioadonis/Notas.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
````
