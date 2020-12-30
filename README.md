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



