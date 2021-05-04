# INSTALACION EN LINUX

`sudo curl -fsSL https://get.docker.com -o get-docker.sh`

`sh get-docker.sh`

# Agregue su cuenta de usuario al grupo de Docker Para evitar este error de permiso:

`sudo usermod -aG docker <current-user-name>`

example:

$ `sudo usermod -aG docker infinity`

requires log-off in order to make effective

=============

# Docker Machine:
https://github.com/docker/machine/releases/
``` curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine &&
    chmod +x /tmp/docker-machine &&
    sudo cp /tmp/docker-machine /usr/local/bin/docker-machine ```

`docker-machine --version`


# Docker Compose:

`sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose  `


`sudo chmod +x /usr/local/bin/docker-compose`


`docker-compose --version`


==============

# Clone my Github repo

`git clone https://github.com/BretFisher/udemy-docker-mastery`


`docker version`
`docker info `
`docker container run --publish 80:80 nginx`  >> " correr una imagen"

`docker container run --publish 80:80 --detach nginx`>> " se ejecuta en segundo plano"

`docker container ls` <<>> " mostrar los contenedores run"
`docker container ls -a` <<>> "muestra todos los contenedores apagados"  
`docker container stop <ID>`  "EL id solo puede colocar 3 digitos"
`docker container run --publish 80:80 --detach --name <NAME> nginx` <<>> Darle nombre al docker
`docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql` >> "corre un docker de mysql que requiere variable de entorno"
`docker container logs webhost` <<>> "ver los logs"
`docker container rm 89c 881 ab8` <<>> "borrar docker"
`docker container rm -f ab8` <<>> "borrar docker si esta run"
`docker ps` <<>> "listar containesr"

`docker container top webhost ` <<>> "ver los procesos del docker"
`docker container inspect NAME ` <<>> " inpeciona el json del docker"

`docker container stats --all`
`docker container stats` <<>> muestra una estadistica de docker corriendo
`ps aux` <<>> "lista procesos internos en docker"

##  conexiones interfas  <<-it>>
`docker container run -it --name proxy nginx bash` <<>> "correr y conexion con terminal"

`docker container run -it --name ubuntu ubuntu` <<>> "correr maquina ubuntu en docker"
`docker container start -ai ubuntu` <<>> "Volver a conectarce a ubuntu"

`docker container exec -it mysql bash ` <<>> "Saltar y conectarce a un docker existente"

## alpine

`docker image ls`  <<>> "lista images descargadas"
`docker container run -it alpine sh`

## docker conexion
docker container run -p 80:80 --name webhost -d nginx

`docker container port webhost` <<>> "ver la salida del puerto"
`docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost` <<>> "ver la ip de docker"  ej: 172.17.0.2`

## docker network 

`docker network ls`  <<>> "Muestra todas las redes creadas"
`docker network inspect bridge`  <<>> "muestra la configuracon de ip bridge"
`docker network create <name>` <<>> "crea una conexion nueva con un nombre" 
`docker network create --help`  <<>> "ver todas las configuracion"
`docker network --help`  <<>> ""Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

`docker network connect 597c77d11f3f ac6da3b5a394`  <<>> "" conectarce a un network


`docker network disconnect`  <<>> " desconectarce"

## crea una nueva conexion nginx
`docker container run -d --name my_nginx --network my_app_net nginx`

``` actualizar docker my_nginx
docker container exec -it my_nginx bash
apt-get update && apt-get install -y iputils-ping
```
`docker containe rexec -it my_nginx ping new_nginx` <<>> "ping desde new_nginx a my_nginx"

#### tareas
`docker container run --rm -it centos:7 bash` 
<<>> `yum update curl`
`docker container run --rm -it ubuntu:14.04 bash`
<<>> `apt-get update && apt-get install -y curl`

### DNS Round robin test
`docker network create dude`  <<>> "Creamos una red llamada dude"
`docker container run -d --net dude --net-alias search elasticsearch:2`
`docker container run -d --net dude --net-alias search elasticsearch:2` <<>> "se crea dos conexiones sin nombre alias elasticsearch:2"
`docker container run --rm --net dude alpine nslookup search` nslookup search.<<>> "buscar entradas DNS"
`docker container run --rm --net dude centos curl -s search:9200`<<>volvera el servidor de busqueda elastico

# IMAGENES DOCKER

`docker history nginx:latest` <<>> "historial de la ultima version nginx"
`docker image inspect nginx`  <<>> "inspesionar inf json"
`docker pull nginx:mainline` <<>> hacer una copia de un imagen con us tag
`docker image tag nginx bretfisher/nginx`<<>> "crea una etiqueta a la imagen"
`docker login` <<>> "login en docker hub"
cat .docker/config.json
`docker image push bretfisher/nginx` <<>> "sube una imagen como git hub"
`docker image tag bretfisher/nginx bretfisher/nginx:testing` <<>> "crea una imgen con un tag" ej:
docker image tag nginx-with-html:latest bretfisher/nginx-with-html:latest

# buil Docker

`vim docker` <<>> "ver el contenido"
`docker image build -t customnginx .` <<>> "crea una imgen"
` docker image ls`  <<>> "lista las images"
ej 2
`docker image build -t nginx-with-html .`

# Construir un Docker

` docker build -t testnode .` <<>> "crea el dockerfile nombre testnode"
`docker container run -p 80:3000 testnode` <<>> "correr el docker creado"
`docker tag testnode bretfisher/testing-node` <<>> "crea una etiqueta al docker"
`docker push bretfisher/testing-node` <<>> "crea una imagen en docker hub"
`docker image rm bretfisher/testing-node` <<>> "crea la image subida"
`socker container run --rm -p 80:3000 bretfisher/testing-node` <<>> "correr la imagen"
`docker image prune` - para limpiar solo imágenes "colgantes"
`docker system prune` - limpiará todo
`docker image prune -a` - eliminará todas las imágenes que no esté utilizando
`docker system df` - para ver el uso del espacio.

# VOLUMENES EN DOCKER
` docker volume ls`  <<>> "ver los volumes asignados"
`docker volume inspect 19b8c7e3******` <<>> "detalles del volumen "

`docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-bd:/var/lib/mysql mysql ` >> `-v mysql-bd:`/var/lib/mysql mysql "crea un volumen con nombre 'mysql-bd' "
`docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx` <<>> "corre un docker en un lugar espesificado del pc O MI CARPETA ACTUAL "

## BASES DATOS VOLUMEN IN DOCKER

`docker container run -d --name psql -v psql:/var/lib/postgresql/data postgres:9.6.1`
`docker container logs -f psql`
`docker container run -d --name psql2 -v psql:/var/lib/postgresql/data postgres:9.6.2`

# DOCKER COMPOSE

`docker-compose up` <<>> "corre los contenedores programados en el archivo .yml"
`docker-compose up -d` <<>> "Se ejecuta en segundo plano sin mostrar los get"
`docker-compose logs`  <<>> "ver los resgistros"
`docker-compose --help`  <<>> " todos los comando"
`docker-compose ps`  <<>> "ver los que estan corriendo"
`docker-compose top`  <<>> "ver lo que corre dentro"
`docker-compose down`  <<>> "Stop y limpia procesos"

`docker-compose down -v` <<>> "borrar los volumes creados"

`docker image rm nginx-custom` <<>> "borra la imagen nginx-custom"

`docker-compose down --rmi local` <<>> "borra imagen local que nombro automaticas"

Docker Compose, debe establecer una contraseña con la variable de entorno:
`POSTGRES_PASSWORD=mypasswd`
O dígale que ignore las contraseñas con la variable de entorno:
`POSTGRES_HOST_AUTH_METHOD=trust`


# SWARN

`docker swarm init`  <<>> "Iiniciamos el codigo para el ejambre"
 docker swarm join --token SWMTKN-1-1zisrk3s7nl576dy7usfcf5w0aasouiefdjw5wbdvp4393rxvm-0o07hbyti50nv6t374ui8mxj8 192.168.0.122:2377

 `docker node ls`
ID HOSTNAME STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
ld6tk infinity-Inspiron-N4010 Ready Active Leader           20.10.6

`docker node --help`
Usage:  docker node COMMAND
Manage Swarm nodes
Commands:
  demote      Demote one or more nodes from manager in the swarm
  inspect     Display detailed information on one or more nodes
  ls          List nodes in the swarm
  promote     Promote one or more nodes to manager in the swarm
  ps          List tasks running on one or more nodes, defaults to current node
  rm          Remove one or more nodes from the swarm
  update      Update a node

`docker node update --role manager node2` <<>> "hacer alministrador nodo2"

`docker swarm join-token manager`
To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-1zisrk3s7nl576dy7usfcf5w0aasouiefdjw5wbdvp4393rxvm-764nz9mknb0aqy6caj4pzfmbj 192.168.0.122:2377

`docker service --help`
Usage:  docker service COMMAND
Manage services
Commands:
  create      Create a new service
  inspect     Display detailed information on one or more services
  logs        Fetch the logs of a service or task
  ls          List services
  ps          List the tasks of one or more services
  rm          Remove one or more services
  rollback    Revert changes to a service's configuration
  scale       Scale one or multiple replicated services
  update      Update a service

`docker service create alpine ping 8.8.8.8`
  5xdz5x9zvxxl9mbv91iy2s542

`docker service ls`
ID             NAME             MODE         REPLICAS   IMAGE           PORTS
5xdz5x9zvxxl   vibrant_yonath   replicated   1/1        alpine:latest 

`docker service ps vibrant_yonath`   <<>> "mostrara las tares del servicio"
ID  NAME  IMAGE     NODE    DESIRED STATE   CURRENT STATE  ERROR   PORTS
uauhrf5xjfyy   vibrant_yonath.1   alpine:latest   infini  Running  Running 3 min 

`docker service update 5xdz5x9zvxxl --replicas 3` <<>> "5xdz5x9zvxxl es el ID del servicio ; "--replicas" la cantida de replicas"
5xdz5x9zvxxl
overall progress: 3 out of 3 tasks 
1/3: running   [==================================================>] 
2/3: running   [==================================================>] 
3/3: running   [==================================================>] 

`docker service ps vibrant_yonath `
ID NAME IMAGE NODE DESIRED STATE CURRENT STATE ERROR PORTS
uauhrf5xjfyy   vibrant_yonath.1   alpine:latest   infinity-Inspiron-N4010   Running         Running 16 minutes ago             
llhr80iqpxiu   vibrant_yonath.2   alpine:latest   infinity-Inspiron-N4010   Running         Running 3 minutes ago              
g855in3kr3uq   vibrant_yonath.3   alpine:latest   infinity-Inspiron-N4010   Running         Running 3 minutes ago 

`docker update --help` <<>> "listado de opt para cargar configuraciones sin apagar el docker"
Options:
      --blkio-weight uint16        Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --cpu-period int             Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int              Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int          Limit the CPU real-time period in microseconds
      --cpu-rt-runtime int         Limit the CPU real-time runtime in microseconds
  -c, --cpu-shares int             CPU shares (relative weight)
      --cpus decimal               Number of CPUs
      --cpuset-cpus string         CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string         MEMs in which to allow execution (0-3, 0,1)
      --kernel-memory bytes        Kernel memory limit
  -m, --memory bytes               Memory limit
      --memory-reservation bytes   Memory soft limit
      --memory-swap bytes          Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --pids-limit int             Tune container pids limit (set -1 for unlimited)
      --restart string             Restart policy to apply when a container exits

`docker service update --help` <<>> "listado de opt para cargar configuraciones sin apagar los servicios"

`docker node `
# docker-machine

`docker-machine create node1` <<>> "crea el entorno nodo1"

`docdocker-machine ssh node1` <<>> "conectarce al nodo1"

`docker-machine env node1` <<>> "docker de entorno"

`docker network create --driver overlay mydrupal` <<>> "crea una red"
rnoa5yqi09dv6d9r2ox5pgkxh

`docker network ls`
NETWORK ID     NAME              DRIVER    SCOPE
0fc90639d9ef   bridge            bridge    local
354becce15ff   docker_gwbridge   bridge    local
ac6da3b5a394   host              host      local
fcngrbsysm33   ingress           overlay   swarm
rnoa5yqi09dv   mydrupal          overlay   swarm
702f30f66df0   none              null      local

`docker service create --name psql --network mydrupal -e POSTGRES_PASSWORD=mypass postgres` <<>> "crea un primer service"
1054u8t1shv60ghd4c4zvhv7f
overall progress: 1 out of 1 tasks 

`docker service ps psql` <<>> "darle nombre al service"
ID  NAME  IMAGE   NODE DESIRED STATE   CURRENT STATE    ERROR     PORTS
g7ax8ki68zwv   psql.1    postgres:latest   infinity-Inspiron-N4010   Running         Running 2 mi

`docker service create --name drupal --network mydrupal -p 80:80 drupal` <<>> "crea un segundo service""
mgs8mfxcls555slrw7tvevukr
overall progress: 0 out of 1 task

`watch docker service ls`  <<>> "monitorear los services"

`docker service rm psql` <<>> " Elimina es service"

`docker service create --name search --replicas 3 -p 9200:9200 elasticsearch:2` 

# STACK

`docker stack deploy -c example-voting-app-stack.yml voteapp` <<>> "Correr el archivo.yml"
`docker stack`
Options:
      --orchestrator string   Orchestrator to use (swarm|kubernetes|all)
Commands:
  deploy      Deploy a new stack or update an existing stack
  ls          List stacks
  ps          List the tasks in the stack
  rm          Remove one or more stacks
  services    List the services in the stack

`docker stack ps voteapp` <<>> "ver los services corriendo"
`docker stack services voteapp` <<>> "ver las tares corriendo"

# SECRET

`docker secret create psql_user psql_user.txt`
q00r7qhn43fpq3ysh2*******

`echo "myDBpassWORD" | docker secret create psql_pass -`
748801e****krlnytjlw89rdco

`docker secret ls`  <<>> "lista las id secret"

`docker secret inspect psql_user`
[
    {
        "ID": "q00r7qhn43fp****c765cvv",
        "Version": {
            "Index": 339
        },
        "CreatedAt": "2021-04-26T21:14:11.560227828Z",
        "UpdatedAt": "2021-04-26T21:14:11.560227828Z",
        "Spec": {
            "Name": "psql_user",
            "Labels": {}
        }
    }
]

`docker service create --name psql --secret psql_user --secret psql_pass -e POSTGRES_PASSWORD_FILE=/run/secrets/psql_pass -e POSTGRES_USER_FILE=/run/secrets/psql_user postgres` 
0k9o95elsxvr7c530law92iq1

`docker service ps psql` <<>> "listar service"

`docker exec -it psql.1.qnyjer6t5erhi7u8jbsl49a59 bash` <<>> "ingresar consola "

# SECRET-COMPOSE

`docker stack deploy -c docker-compose.yml mydb` <<>> "correr el compose docker"
`docker secret ls`  <<>> "lista las id secret"
`docker stack rm mydb` <<>> "eliminar los stack secret"

`echo "ruber1234" | docker secret create psql-pw -`

`docker-compose -f docker-compose.yml -f docker-compose.test.yml up -d` <<>> "expesificar que compose arrancar en la producion"

`docker-compose -f docker-compose.yml -f docker-compose.prod.yml config --help`
Validate and view the Compose file.
Usage: config [options]
Options:
    --resolve-image-digests  Pin image tags to digests.
    --no-interpolate         Don't interpolate environment variables.
    -q, --quiet              Only validate the configuration, don't print
                             anything.
    --profiles               Print the profile names, one per line.
    --services               Print the service names, one per line.
    --volumes                Print the volume names, one per line.
    --hash="*"               Print the service config hash, one per line.
                             Set "service1,service2" for a list of specified services
                             or use the wildcard symbol to display all services.

`docker-compose -f docker-compose.yml -f docker-compose.prod.yml config` <<>> "merge con los compose update"
secrets:
  psql-pw:
    external: true
    name: psql-pw
services:
  drupal:
    image: custom-drupal:latest
    ports:
    - published: 80
      target: 80
    volumes:
    - drupal-modules:/var/www/html/modules:rw
    - drupal-profiles:/var/www/html/profiles:rw
    - drupal-sites:/var/www/html/sites:rw
    - drupal-themes:/var/www/html/themes:rw
  postgres:
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/psql-pw
    image: postgres:12.1
    secrets:
    - source: psql-pw
    volumes:
    - drupal-data:/var/lib/postgresql/data:rw
version: '3.1'
volumes:
  drupal-data: {}
  drupal-modules: {}
  drupal-profiles: {}
  drupal-sites: {}
  drupal-themes: {}

  # SWAM UPDATES

  `docker service create -p 8088:80 --name web nginx:1.13.7` <<>> crea un servicio
  `docker service ls`  <<>> "ver el service"
  `docker service scale web=5` <<>> "escalar n veces el servicio
  `docker service update --image nginx:1.13.6 web` <<>> "actualizara de a uno pero no el puerto"
  `docker service update --publish-rm 8088 --publish-add 9090:80 web` <<>> "Actualiza los puertos"
  `docker service update --force web` <<>> "equilibrara los services que estan corriendo"
  `docker service rm web` <<>> "eliminar el servicio web"

  # Docker HEALTHCHECKS
`docker container run --name p1 -d postgres`

`docker container run --name p2 -d --health-cmd="pg_isready -U postgres || exit 1" postgres`
bcb8d90a43cfb93d1654943a7d1cae294afb587268199fb96f183da03f4d0eb7

`docker container inspect p2`

`docker service create --name p1 postgres` <<>> "creamos un service sin chequeo de health"
`docker service create --name p2 --health-cmd="pg_isready -U postgres || exit 1" postgres` <<>> " espera 30 segundo y chequeara y espera a un estado de health

# Docker Registry

`docker container run -d -p 5000:5000 --name registry registry`

`docker image ls`  <<>> "lista images"

`docker pull hello-world` <<>> " envia un image"

`docker tag hello-world 127.0.0.1:5000/hello-world` 
`docker container rm admiring_stallman`
`docker image remove 127.0.0.1:5000/hello-world `

`docker container kill registry ` <<>> "mata el proceso del docker registry"
`docker container run -d -p 5000:5000 --name registry -v $(pwd)/registry-data:/var/lib/registry registry` <<>> crea el docker con un volumen asignado

`tree registry-data/` <<>> " ver las ramas de docker"

# Docker Registry swarm

https://labs.play-with-docker.com/  <<>> "plataforma virtual de paractica"

creamos un plantilla de 5 contenedores

`docker node ls`  <<>> "ver las imagense rum"
`docker service create --name registry --publish 5000:5000 registry`
`docker service ps registry`
`docker pull hello-world`
`docker tag hello-world 127.0.0.1:5000/hello-world `
`docker push 127.0.0.1:5000/hello-world`  <<> agregaria el hello -world al catalogo en puerto 5000 hacer click
http://ip172-18-0-52-c24osbvnjsv000el1kig-5000.direct.labs.play-with-docker.com/v2/_catalog  

`docker pull nginx `  <<>> "crear nginx"
`docker tag nginx 127.0.0.1:5000/nginx`  <<>> "crea el tag"
`docker push 127.0.0.1:5000/nginx`
`docker service create --name nginx -p 80:80 --replicas 5 --detach=false 127.0.0.1:5000/nginx`  <<>> "crea una imagen en cada contenedor"
`uname -a` <<>> "ver el nombre de la maquina linux


# KUBERNETES

### k8s "k-eights"
https://play-with-k8s.com/  katacoda.com
 
 katacoda.com  

$ `kubectl version`
Client Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.3", GitCommit:"06ad960bfd03b39c8310aaf92d1e7c12ce618213", GitTreeState:"clean", 
`minikube start --wait=false`
`kubectl cluster-info`
`kubectl get nodes` 

$ `kubectl run my-nginx --image nginx`  <<>> "crear un servicio"
 kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/my-nginx created

$ `kubectl get pods` 
NAME                               READY   STATUS    RESTARTS   AGE
my-nginx-669bb4594c-s5qqx          1/1     Running   0          2m6s

$ `kubectl get all`
NAME                                   READY   STATUS    RESTARTS   AGE
pod/my-nginx-669bb4594c-s5qqx          1/1     Running   0          3m20s
NAME                       TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
service/kubernetes         ClusterIP   10.96.0.1      <none>        443/TCP        26m

$ `kubectl delete deployment my-nginx `  <<>> " borramos el servicio creado"
deployment.apps "my-nginx" deleted

### Scaling ReplicaSets
`kubectl run my-apache --image httpd` <<>> "crear un servicio"

$ `kubectl scale deployment my-apache --replicas 2` <<>> "crear las replicas"
$ `kubectl scale deploy/my-apache --replicas 2` <<>> "vercion resumida" deploy=deployment=deployments

$ `kubectl get pods`
NAME                         READY   STATUS    RESTARTS   AGE
my-apache-5d589d69c7-7kllk   1/1     Running   0          8m22s
my-apache-5d589d69c7-df4rf   1/1     Running   2          13m

$ `kubectl logs deployment/my-apache`

$ `kubectl logs deployment/my-apache --follow --tail 1` <<>> nos votara la ultima linea
Found 2 pods, using pod/my-apache-5d589d69c7-df4rf
[Wed Apr 28 21:52:00.548389 2021] [core:notice] [pid 1:tid 140655079634048] AH00094: Command line: 'httpd -D FOREGROUND'

$ `kubectl logs -l run=my-apache` <<>> logs de apache cuando corre

$ `kubectl describe pod/my-apache-5d589d69c7-7kllk` <<>> sumilar a impecionar en sward

$ `kubectl get pods -w` <<>> monitorea los cambios

`kubectl delete pod/my-apache-5d589d69c7-7kllk` <<>> borrar un pod

#  ClusterIP Service

$` kubectl  create deployment httpenv --image=bretfisher/httpenv`
$ `kubectl scale deployment/httpenv --replicas=5`
$ `kubectl expose deployment/httpenv --port 8888` <<>> creamos un servicio

$ `kubectl get service`
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
httpenv      ClusterIP   10.107.104.254   <none>        8888/TCP   95s
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP    10m

#### creamos un generador

$ `kubectl run --generator run-pod/v1 tmp-shell --rm -it --image bretfisher/netshoot -- bash` <<>> "hacede a la terminal del servicio"

probamos con `curl httpenv:8888` :

{"HOME":"/root","HOSTNAME":"httpenv-6bf64f7c4f-t4dz2","KUBERNETES_PORT":"tcp://10.96.0.1:443","KUBERNETES_PORT_443_TCP":"tcp://10.96.0.1:443","KUBERNETES_PORT_443_TCP_ADDR":"10.96.0.1",

$ `kubectl get all` <<>> "ver todos los detalles y nombres del servicio"

#### Equilibrador de carga
$ `kubectl expose deployment/httpenv --port 8888 --name httpenv-np --type NodePort` <<>> "crea un puerto de nodo"
service/httpenv-np exposed
$ `kubectl get service`
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
httpenv      ClusterIP   10.107.104.254   <none>        8888/TCP         20m
httpenv-np   NodePort    10.97.175.124    <none>        8888:30696/TCP   16s
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP          29m

`curl localhost:30696`

Equilibrador:

`kubectl expose deployment/httpenv --port 8888 --name httpenv-lb --type LoadBalancer`
`curl localhost:8888`

ELIMINAR:
$ `kubectl delete service/httpenv service/httpenv-np`

$ `kubectl delete service/httpenv-lb deployment/httpenv`

### Kubernetes DNS Service
https://github.com/kubernetes/dns/blob/master/docs/specification.md
https://www.coredns.io/plugins/kubernetes/

`kubectl get namespaces`
NAME              STATUS   AGE
default           Active   61m
kube-node-lease   Active   61m
kube-public       Active   61m
kube-system       Active   61m 

### Generadores 

`kubectl create deployment test --image nginx --dry-run `
`kubectl create deployment test --image nginx --dry-run -o yaml` <<>> mostra la salida de yemen

`kubectl create job test --image nginx --dry-run -o yaml`  <<>> crea un trabajo

`kubectl create deployment test --image nginx`
`kubectl expose deployment/test --port 80 --dry-run -o yaml` <<>> crea el servicio
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: test
  name: test
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: test
status:
  loadBalancer: {}

` kubectl delete deployment test` <<>> "borrar el servicio"

### kubectl run

`kubectl run test --image nginx --dry-run `  <<>>
`kubectl run test --image nginx --port 80 --expose --dry-run` <<>> creara el servicio y app sin usar dos comandos
`kubectl run test --image nginx --restart OnFailure --dry-run`  <<>> creara el trabajo por lotes
`kubectl run test --image nginx --restart Never --dry-run`  <<>> reiniciar en caso de falla
`kubectl run test --image nginx --schedule "*/1 * * * *" --dry-run`  <<>> crear un trabajo cronb

### kubernetes Imperative:
examples: ` kubectl run, kubectl create deployment, kubectl update`
comandos: `run, expose, scale, edit, create deployment`
Imp objects: ` create -f file.yml, replace -f file.yml delete file.yml`
https://kubernetes.io/docs/tasks/manage-kubernetes-objects/imperative-command/
https://kubernetes.io/docs/tasks/manage-kubernetes-objects/imperative-config/
### kubernetes Declarativo:
examples: `kubectl apply -f my-resources.yaml`
Decl objects: ` apply -f file.yml` or `apply -f directory\, diff`
https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/