# Limitar uso de procesador
Limitar la cantidad de núcleos de CPU:
```
--cpus=<número de núcleos>
```

Asignar núcleos de CPU específicos:
```
--cpuset-cpus=<lista de núcleos>
```

**¿Como saber el numero de procesadores virtuales que tiene una máquina?**
## COMPLETAR
Existen distintas formas como
1. Ver desde el administrador de tareas
2. Con el Get-CimInstance Win32_Processor | Select-Object NumberOfLogicalProcessors en powershell
  3. Con Linux se puede usar nproc o lscpu
4. Dentro de docker nproc
5. O desde terminal de host docker inspect <container_id>
## Ejemplos
_Puedes copiar y ejecutar directamente cada uno de los comandos_
<img width="925" height="884" alt="image" src="https://github.com/user-attachments/assets/780548f6-b635-4430-a55b-f02f96c9dd68" />
<img width="1448" height="331" alt="image" src="https://github.com/user-attachments/assets/e64ad957-a8d7-4d96-9d2a-20fba5cb8a00" />
<img width="629" height="84" alt="image" src="https://github.com/user-attachments/assets/f48af909-8afa-4838-89f9-9dc7bf10256e" />

Limitar el uso de CPU a 1.5 núcleos
```
docker run -d --name server-nginx --cpus="1.5" nginx:alpine
```

Restringir el contenedor para que use los núcleos de CPU 0 a 2:
```
docker run -d --name server-nginx --cpuset-cpus="0-2" nginx:alpine
```

Restringir el contenedor para que use los núcleos de CPU 1 y 3:
```
docker run -d --name server-nginx --cpuset-cpus="1,3" nginx:alpine
```
