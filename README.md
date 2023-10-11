# Compilar con Docker
## Que es Docker?
En terminos *muy amplios*, docker es un programa que permite crear distintos `containers`. Estos `containers`, se parecen en muchos aspectos a una maquina virtual, solo que son mucho mas livianos y consumen recursos (dado que emulan menos componentes de una computadora que una maquina virtual tradicional).

## Por que necesito Docker?
En la industria, docker se utiliza para solucionar los problemas como *"en mi computadora funciona, en la de otro no"*. En el caso de la materia, los que tengan Mac (M1/M2) van a necesitar usar docker para esta segunda parte, para sacar todo el provecho a la flag `-fsanitize=address`.
El problema en estas computadoras es que dicha flag no detecta algunos errores de `memory-leak`. El ejemplo mas basico para probar esto es hacer un `malloc` sin un `free` en un programa: los usuarios de MAC M1 y M2 podran ver que no se produciran errores de `memory-leak`, pero si se producira para los que utilizan Linux o Windows + WSL.
Esto representa un problema, ya que a veces (incluyendo las guias y en especial para el TP final) puede ser dificil darse cuenta que estamos cometiendo errores con el manejo de memoria si nadie nos avisa.
A continuacion, vamos a explicar como utilizar docker de manera simple para que puedan compilar sin problemas y se detecten TODOS los errores.

## Como instalo Docker?
Seguir los pasos en https://docs.docker.com/desktop/install/mac-install/ para instalar la aplicacion de escritorio `Docker Desktop`.
Seleccionar la opcion de descarga de `MAC With Apple Silicon`, dado que los que tengan MAC con intel no necesitan usar docker.

## Como configuro mi container de Docker?
Ustedes probablemente tienen una carpeta llamada `PI` (o algo similar) en la que tienen todos los TP. Para esta guia vamos asumir que el path absoluto a esa carpeta es `/home/user/PI`, pero la deben reemplazar por el path real.
Lo que vamos a hacer ahora es crear nuestro container de docker, utilizando una imagen de `ubuntu`. Este paso se debe hacer UNA SOLA vez (una vez creado el container, no volver a crearlo).
Le vamos a dar el nombre `ubuntu-PI` a nuestro container, y nuestros archivos quedaran en el path `/PI` del container.
1. Abrir la aplicacion de docker haciendo doble click (una vez abierta la pueden minimizar, no hace falta tocar nada)
2. Ejecutar el comando ```docker create -it --name ubuntu-PI -v /home/user/PI:/PI ubuntu /bin/bash```
3. Verificar que el container se haya creado correctamente ejecutando `docker ps --all`. El nombre `ubuntu-PI` deberia aparecer en pantalla junto al resto de los containers que tengan (si es que ya tenian docker)

## Como compilo con Docker
1. Abrir la aplicacion de docker haciendo doble click (una vez abierta la pueden minimizar, no hace falta tocar nada)
2. Ejecutar el comando `docker start ubuntu-PI` para "encender" el container
3. Ejecutar el comando `docker exec -it ubuntu-PI /bin/bash` para abrir una consola
4. Ejecutar `cd /PI` para posicionarnos en nuestro directorio de PI
5. **NOTA:** Si es la primera vez que abris el container y notas que `GCC` no esta instalado en el sistema (probar ejecutando `gcc --version`), podes instalarlo ejecutando `apt update && apt upgrade && apt install gcc`
6. Felicitaciones, si llegaste a este paso ya podes compilar con todos los warnings y errores de `GCC` disponibles
7. Para salir del container, ejecutar `exit` en el container
8. Para "apagar" el container, ejecutar `docker stop ubuntu-PI` en la terminal de nuestro sistema
9. En este paso ya podemos cerrar la aplicacion Docker Desktop y apagar la computadora

## Nota
Aunque puede representar una molestia hacer varios pasos para abrir un container cada vez que queremos compilar algo, es importante que no ignoremos la importancia de la flag `-fsanitize=address`. Dicha flag nos hace saber cada vez que cometemos un error en el manejo de la memoria (en especial cuando pedimos memoria y no la devolvemos al terminar el programa). Omitir su uso puede provocar que pensemos que el programa funciona bien cuando esto NO es asi.