# Crear una API sencilla con SAM
29-30 de DIC de 2018.

A continuación, se describe las acciones realizadas como parte del estudio y 
experimentación de la arquitectura serverless con AWS.

## Creación de usuario
Para tener acceso a los recursos se creó un usuario en la consola de 
administración:
- Se creó un grupo en IAM llamado 'Administrators' con acceso completo
- Se adjuntó la política 'AdministratorAccess' al grupo recién creado
- Se creó un usuario 'sa' y se agregó al grupo recién creado
- Por último se creó un IDE en Cloud9 llamado 'sc-ide'

## Creación de IDE
Para desarrollar las tareas de codificación se creó un IDE con las siguientes 
especificaciones:
- Región: us-east-1 correspondiente a N. Virgina
- Nombre: 'sc-ide'
- Instancia EC2 nueva
- Tipo de instancia: t2.micro (1 GiB RAM + 1 vCPU)
- Apagar después de 30 minutos
- Rol por defecto
- VPC por defecto

 
## Experimentar con una aplicación de ejemplo con SAM
SAM (Serverless Application Model) es un framework mantenido por AWS para el 
desarrollo de aplicaciones serverless.
Para obtener el conocimiento de este framework se siguió un ejemplo disponible 
en la documentación oficial.
El proyecto de ejemplo es una aplicación sencilla que comprende una API y la 
ejecución de una función.
https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/
serverless-quick-start.html

Las acciones para este experimento fueron las siguientes:

### Requisitos
Antes de comenzar se verificaron los siguientes requisitos:
- Verificar que se tiene instalado python
- Verificar que se tiene instalado pip
- Verificar que se tiene instalado aws-sam-cli 
- Verificar que se tiene instalado docker
- Verificar que se tiene instalado aws-cli
- Verificar que se tiene instalado python3
- Verificar que se tiene instalado virtualenv

Para verificar cada requisito se ejecutan los siguientes comandos en la terminal:
````
sa:~/environment $ python --version
Python 2.7.14
sa:~/environment $ pip --version
pip 9.0.3 from /usr/lib/python2.7/dist-packages (python 2.7)
sa:~/environment $ sam --version
SAM CLI, version 0.8.1
sa:~/environment $ docker --version
Docker version 18.06.1-ce, build e68fc7a215d7133c34aa18e3b72b4a21fd0c6136
sa:~/environment $ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
sa:~/environment $ aws --version
aws-cli/1.16.67 Python/2.7.14 Linux/4.14.77-70.59.amzn1.x86_64 botocore/1.12.57
sa:~/environment $ python3 --version
Python 3.6.5
sa:~/environment $ virtualenv --version
16.1.0

````
Según lo experimentado, todo estaba ya instalado en Cloud9 de forma 
predeterminada.

### Iniciar el proyecto
Para continuar, se crea una carpeta para el proyecto y se inicializa con SAM 
especificando un runtime, para el caso sería python3. Los comandos en la terminal
serían los siguientes:
````
sa:~/environment $ project=~/environment/hello-world-api
sa:~/environment $ mkdir $project
sa:~/environment $ cd $project
sa:~/environment/hello-world-api $ sam init --runtime python3.6                                                            
[+] Initializing project structure...
[SUCCESS] - Read sam-app/README.md for further instructions on how to proceed
[*] Project initialization is now complete

````
Estos comandos crearon un nuevo directorio 'hello-world-api' con los archivos 
básicos para iniciar el desarrollo de una API. Para verificar pueden ejecutarse
los siguientes comandos:
````
sa:~/environment/hello-world-api $ $project
bash: /home/ec2-user/environment/hello-world-api: Is a directory
sa:~/environment/hello-world-api $ tree $project
/home/ec2-user/environment/hello-world-api
└── sam-app
    ├── hello_world
    │   ├── app.py
    │   ├── app.pyc
    │   ├── __init__.py
    │   ├── __init__.pyc
    │   └── requirements.txt
    ├── README.md
    ├── template.yaml
    └── tests
        └── unit
            ├── __init__.py
            ├── __init__.pyc
            ├── test_handler.py
            └── test_handler.pyc

4 directories, 11 files
````
Nota: para generar la estructura de archivos del bloque anterior se instaló una 
herramienta con los siguientes comandos en la terminal:
````
sa:~/environment/hello-world-api $ sudo yum install tree -y

````

### Compilar (build)
El proceso de compilar es para generar todos los archivos binarios necesarios 
para la ejecución, incluyendo sus dependiencias, que en muchos casos son descargadas
con un manejador de dependiencias como Npm en caso de Nodejs o Pip en caso de Python.

Para compilar la aplicación se ejecutaron los siguientes comandos en la terminal.
````
sa:~/environment/hello-world-api $ sam build --use-container
2018-12-31 16:49:13 Starting Build inside a container
Error: Template file not found at /home/ec2-user/environment/hello-world-api/template.yml
sa:~/environment/hello-world-api $ cd sam-app/
sa:~/environment/hello-world-api/sam-app $ sam build --use-container
2018-12-31 16:49:52 Starting Build inside a container
2018-12-31 16:49:52 Found credentials in shared credentials file: ~/.aws/credentials
2018-12-31 16:49:52 Building resource 'HelloWorldFunction'

Fetching lambci/lambda:build-python3.6 Docker container image......
2018-12-31 16:49:52 Mounting /home/ec2-user/environment/hello-world-api/sam-app/hello_world as /tmp/samcli/source:ro inside runtime container
Running PythonPipBuilder:ResolveDependencies
Running PythonPipBuilder:CopySource

Build Succeeded

Built Artifacts  : .aws-sam/build
Built Template   : .aws-sam/build/template.yaml

Commands you can use next
=========================
[*] Invoke Function: sam local invoke
[*] Package: sam package --s3-bucket <yourbucket>
    
````
Después de estos comandos se creó dentro de la carpeta 'sam-app' otra carpeta 
'.aws-sam' y dentro de esta una carpeta más 'build' con todo el código fuente y 
todas sus dependencias.

### Probar la aplicación localmente
Para probar la aplicación de forma local se ejecutan los siguientes comandos en 
la terminal teniendo en cuenta que se ejecuten en la ubicación donde se 
encuentra el archivo 'template.yml':
````
sa:~/environment/hello-world-api/sam-app $ ls template.yaml 
template.yaml
sa:~/environment/hello-world-api/sam-app $ sam local start-api
2018-12-31 16:58:44 Found credentials in shared credentials file: ~/.aws/credentials
2018-12-31 16:58:45 Mounting HelloWorldFunction at http://127.0.0.1:3000/hello [GET]
2018-12-31 16:58:45 You can now browse to the above endpoints to invoke your functions. You do not need to restart/reload SAM CLI while working on your functions changes will be reflected instantly/automatically. You only need to restart SAM CLI if you update your AWS SAM template
2018-12-31 16:58:45  * Running on http://127.0.0.1:3000/ (Press CTRL+C to quit)
````
Para probar la API se puede hacer una petición Http con cURL ejecutando el siguiente
comando en otra terminal:
```
sa:~/environment $ curl http://127.0.0.1:3000/hello
{"message": "hello world", "location": "18.232.105.67"}
```
**TODO:** hacer un tunel vpn para hacer una petición desde Postman (instalado en Windows)
y/o un web browser.

 
Como ejercicio se cambia el mensaje para verificar que se re-compila, es necesario
detener el proceso, volver a compilar y volver a arrancar el api:
 - Matar el proceso en la terminal presionando CTRL+C
 - Abrir el archivo ~/environment/hello-world-api/sam-app/hello_world/app.py
 - Cambiar el mensaje que retorna la función 'lambda_handler'
 - Recompilar: sam build --use-container
 - Inicial la API: sam local start-api
 - Probar nuevamente: curl http://127.0.0.1:3000/hello
 

### Empaquetar la aplicación
Una vez que se ha probado la aplicación, se debe empaquetar para posteriormente 
publicarla.

Es necesario tener un bucket en S3 con un nombre globalmente único para generar 
un archivo .zip. Para crearlo se ejecuta el siguiente comando en la terminal:
````
sa:~/environment/hello-world-api/sam-app $ bucketname=sam-hello-world-api
sa:~/environment/hello-world-api/sam-app $ aws s3 mb s3://$bucketname
make_bucket: sam-hello-world-api
````
 
Empaquetar la aplicación y subirla a S3 ejecutando el siguiente comando en la 
terminal:
````
sa:~/environment/hello-world-api/sam-app $ sam package --output-template-file packaged.yaml --s3-bucket $bucketname
Uploading to 6b834d4e844eca6aa1df4c853a4a10ea  526107 / 526107.0  (100.00%)
Successfully packaged artifacts and wrote output template to file packaged.yaml.
Execute the following command to deploy the packaged template
aws cloudformation deploy --template-file /home/ec2-user/environment/hello-world-api/sam-app/packaged.yaml --stack-name <YOUR STACK NAME>
````
El comando anterior además de subir un .zip a S3 creó un archivo 'packaged.yaml', 
el cual contiene las especificaciones para el deploy que se hará posteriormente.

### Publicar la aplicación
Ya que se tiene listo el pack se debe publicar (deploy) usando los archivos 
generados en la sección anterior. Para realizarlo ejecutamos los siguientes
comandos en la terminal:
````
sa:~/environment/hello-world-api/sam-app $ sam deploy --template-file packaged.yaml --stack-name sam-app --capabilities CAPABILITY_IAM --region us-east-1                                                                                                                                    

Waiting for changeset to be created..
Waiting for stack create/update to complete
Successfully created/updated stack - sam-app
````
La region especificada puede cambiarse.
Con el comando anterior se creó un stack en AWS CloudFormation con el nombre 
especificado en el parámetro stack-name. Se puede buscar el stack en:
https://console.aws.amazon.com/cloudformation/

Para probar la API hay que entrar al link anterior y buscar el stack recién 
creado, ir a la sección Outputs y hacer clic en el link correspondiente al API Gateway 
'HelloWorldApi'. Para el caso particular de este experimento la Url generada fue:

https://bq7q82fhld.execute-api.us-east-1.amazonaws.com/Prod/hello/

### Eliminar todos los recursos
En la sección anterior, con el stack de CloudFormation se crearon varios recursos.
Para eliminar estos recursos, y no caer en cargos no desados, se pueden borrar 
todos los recursos relacionados al un stack espeífico ejecutando el siguiente 
comando en la terminal:
````
sa:~/environment $ aws cloudformation delete-stack --stack-name sam-app
````
