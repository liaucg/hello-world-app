## Compose multi-container application

### Create a simple "Hello World" application in Python

#### Python application project structure:
```
python
├── app
    ├── hello-world.py
    ├── Dockerfile
```
### Use Docker to create a container image of the "Hello World" application

```
$ docker build -t python-hello-world python/app

$ docker images
REPOSITORY               TAG       IMAGE ID       CREATED          SIZE
python-hello-world       latest    53d46cfc0884   10 minutes ago   116MB

$ docker run python-hello-world
Hello World from Python!
```

### Create a Docker Compose file that runs the "Hello World" application and a separate container running a web server (such as NGINX or Apache)

#### Apache/PHP application project structure:
```
apache-php
├── app
    ├── index.php
    ├── Dockerfile
```

[_compose.yaml_](compose.yaml)
```
services:
  python:
    build:
      context: python/app
  web:
    build:
      context: apache-php/app
      target: builder
    ports: 
      - '80:80'
    volumes:
      - ./apache-php/app:/var/www/html/

```

## Use the command line to start the multi-container application and verify that the application and web server are working as expected

```
$ docker-compose up
...
...
Recreating hello-world-app-python-1 ... done
Recreating hello-world-app-web-1    ... done
Attaching to hello-world-app_python_1, hello-world-app_web_1
python_1  | ==Hello World from Python!==
web_1     | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.20.0.3. Set the 'ServerName' directive globally to suppress this message
web_1     | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.20.0.3. Set the 'ServerName' directive globally to suppress this message
web_1     | [Tue Apr 25 16:06:47.622069 2023] [mpm_prefork:notice] [pid 1] AH00163: Apache/2.4.48 (Debian) PHP/8.0.9 configured -- resuming normal operations
web_1     | [Tue Apr 25 16:06:47.622263 2023] [core:notice] [pid 1] AH00094: Command line: 'apache2 -D FOREGROUND'
hello-world-app_python_1 exited with code 0
```
## Use curl to check web server is working
```
$ curl localhost:80

<h1>Hello World from Apache/PHP!</h1>
```