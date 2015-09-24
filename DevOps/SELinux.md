# SELinux
## Tipos de acceso
### DAC
La mayoría de los sistemas oeprativos usan un sistema de Control de Acceso Discrecional (DAC).
DAC son fundamentalmente inadecuados para una fuerte seguridad del sistemaDAC son fundamentalmente inadecuados para una fuerte seguridad del sistema.

### MAC
Control de acceso obligatorio.
Activa politicas ds eguridad.

#### Contexto SELinux
Se puede ver con el comando ```ls -Z``` para archivos y
``` ls -dZ``` para directorios
```
ls -Z file1
-rwxrw-r-- user1 group1 unconfined_u:object_r:user_home_t:s0 file1
```
Provee de:
* Un usuario ```unconfined_u```
* Un rol ```object_r```
* Un tipo ```user_home_t```
* Un nivel ```s0```

#### Usuarios
En SO's que corren SELinux hay usuarios y usuarios SELinux.
cada usuaio Linux es mapeado a un usuario SELinux, para ver a que usuario esta mapeado cada usuario Linux:
```
    sudo semanage login -l
```

#### Protección
En SO's que corren SELinux osea seguridad DAC y MAC primero se checan los privilegios DAC y despues las politicas MAC; si en primera instancia no pasan los accesos DAC ya no se ejecutan los MAC.

El rol no aplica para archivos por lo tanto se tiene un ```object_r``` que es un objeto generico de rol.


### Booleanos
#### Listado
Para listar los booleanos que tiene SELinux:
```
semanage boolean -l
```
El siguiente comando lista los boleanos ya sea que esten activos o no, pero sin descripción.
```
getsebool -a
```

Para listar el estado de un boleano en especifico
```
getsebool >nombre del boleano>
```
#### Cambio de estado
Para cambiar de estado un booleano:
```
setsebool <nombre del boleano> on
```
Sin embargo este cambio no es persistente, para un cambio persistente se le pasa la bandera ``` -P ```
```
setsebool -P <nombre del booleano> on
```

### Cambios de contexto
Hay distintos comando para cambiar el contexto semanage fcontext chcon
##### Temporalmente
Se puede cambiar el contexto de un archivo o carpeta con el comando
```
chcon <op> <var> archivo
```
#### Devolver al conexto default
```
    restorecon -v archivo
```
##### Persistentes
Cuando un cambio es persistente se registra (en el caso de archivos) en el archivo ```/etc/selinux/targeted/contexts/files/file_contexts```
si los archivos ya estan en este archivo y se cambian los accesos estos se guardan en el archivo ```file_contexts.local```
