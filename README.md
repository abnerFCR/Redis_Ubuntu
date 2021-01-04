# Redis_Ubuntu
Redis en Ubuntu 

Para instalar redis
```sh
$ sudo apt-get install redis-server
```

Para modificar password y permitir al exterior hay que modificar el archivo de configuracion
```sh
$ sudo nano /etc/redis/redis.conf
```

Buscar linea con "requierepass", quitarle el comentario y definir la contraseña.

Para permitir al exterior buscar la linea "bind 127.0.0.1::1" y comentarla.


Reiniciar el servidor de redis
```sh
$ sudo /etc/init.d/redis-server stop
$ sudo /etc/init.d/redis-server start
```

Para entrar a redis en la consola colocamos el comando.

```sh
$ redis-cli
```
Posteriormente como modificamos para definir un password nos pedira que nos autentiquemos, esto lo logramos a traves de la contraseña que definimos anteriormente.

```sh
[IP:PUERTO]> auth [NUESTRA_CONTRASEÑA]
```

En este punto ya podremos manejar nuestros datos con redis, y realizar las operaciones que necesitemos. 

## Introduccion de comandos basicos de redis

1. Para salir de redis 

```sh
[IP:PUERTO]> exit
```
 
 2. Para seleccionar una base de datos.
 
```sh
[IP:PUERTO]> select [index_db]
[IP:PUERTO][index_db_seleccionada]> 
```

3. Para ingresar un valor

```sh
[IP:PUERTO][index_db_seleccionada]>set [clave] [valor] 
```

4. Obtener un valor ingresado

```sh
[IP:PUERTO][index_db_seleccionada]>get [clave] 
```

## Conexion a Redis usando Python 

El codigo necesario para conectarse a Redis utilizando python es el siguiente

```sh
import redis

pool = redis.ConnectionPool(host="[DIRECCION_IP]", port=[NUMERO_PUERTO], password = "[password]", db=[index_db], decode_responses = True)
r = redis.Redis(connection_pool = pool)

#val = r.get("a")   para obtener un valor 
#print(val)         imprimir ese valor
```

### Instalacion de Linkerd

Seguir los pasos de la documentacion oficial

https://linkerd.io/2/getting-started/
```sh
#curl -sL https://run.linkerd.io/install | sh
#export PATH=$PATH:$HOME/.linkerd2/bin #add line to ~/.profile
#linkerd version
#linkerd check --pre
#linkerd install | kubectl apply -f -
```

### Instalacion Helm

```sh
wget https://get.helm.sh/helm-v3.2.4-linux-amd64.tar.gz
tar -xzvf [nombre_archivo_descargado]
sudo mv linux-amd64/helm /sbin
helm repo add stable 	https://charts.helm.sh/stable
helm search repo stable
```

Despues se crea un namespace con el nombre nginx-ingress
```sh
$ kubectl create ns nginx-ingress
```

Ahora Helm+Nginx Ingress
```sh
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx 
helm repo update 
helm install nginx-ingress ingress-nginx/ingress-nginx -n nginx-ingress
```

### Inyectar Ingress
Una vez que ya hemos instalado nginx-ingress tenemos que inyectarlo. 

Primero obtenemos la ip que se le asigno 
```sh
$ kubectl get service -n nginx-ingress
```

Obtenemos el nombre del deploy que instalamos con helm 
```sh
$kubectl get deployments -n nginx-ingress
```

Con el nombre del deployment podemos inyectar el ingress configurando su archivo de configuracion

```sh
kubectl get deployment [NOMBRE_DEPLOYMENT]-n nginx-ingress -o yaml | linkerd inject --ingress - | kubectl apply -f -
```
 
Para verificar que el proceso se haya realizado correctamente se debe ingresar este comando 

```sh
kubectl describe pods [NOMBRE_POD] -n nginx-ingress | grep "linkerd.io/inject: ingress"
```

Obteniendo la siguiente respuesta

```sh
linkerd.io/inject: ingress
```

