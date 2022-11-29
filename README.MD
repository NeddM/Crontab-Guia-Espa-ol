# ¿Qué es Crontab?

Es un [**servicio**](<https://es.wikipedia.org/wiki/Daemon_(inform%C3%A1tica)>) o [**demonio**](<https://es.wikipedia.org/wiki/Daemon_(inform%C3%A1tica)>) (programa que se ejecuta en segundo plano, sin la participación activa del usuario) disponible en Linux y Mac, que nos permite la ejecución de procesos en intervalos regulares de tiempo y distintos eventos.

Esto quiere decir que puedes programar la ejecución de un script para que se realice cada cinco minutos, o para que el script se ejecute cada día, etc...

# Uso de Cron

Para comenzar a usar la herramienta Cron, tenemos que comprobar que el servicio está activo:

```bash
/etc/init.d/cron status
```

![Cron status](/img/1.png)

Cron se puede iniciar desde `/etc/init.d` _o en_ `/etc/rc.d`, dependiendo de la distribución.

## Uso desde las carpetas predefinidas

Una de las formas más sencillas es introducir el script dentro de las carpetas predefinidas de cron, dentro de `/etc`.

![Carpetas predefinidas](/img/2.png)

-   `cron.hourly/`: Los scripts se ejecutan cada hora.
-   `cron.daily/`: Los scripts se ejecutan diariamente.
-   `cron.monthly/`: Los scripts se ejecutan mensualmente.
-   `cron.weekly/`: Los scripts se ejecutan semanalmente.

Es una manera sencilla de programar nuestros scripts, aunque no nos presta las posibilidades de programar las tareas manualmente.

## Uso manual

Aquí viene lo interesante de Crontab; para programar manualmente nuestras tareas tenemos que editar el archivo de texto `/etc/crontab`.

**¡Importante editar el archivo como adminsitrador!**

```bash
sudo nano /etc/crontab
```

_o_

```bash
sudo vim /etc/crontab
```

Dentro del archivo `crontab` se nos muestra información muy útil para programar nuestras tareas:
![/etc/crontab](/img/3.png)

Bajo la ruta de la shell también podemos programar el envío de un correo cada vez que se ejecute una de las tareas.

```bash
MAILTO=root
```

Lo interesante es lo siguiente, se nos explica que cada **asterisco** tiene un valor distinto.

```bash
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
```

-   _Asterisco 1_: El valor de los minutos, de **0 a 59**.
-   _Asterisco 2_: El valor de las horas, de **0 a 23**.
-   _Asterisco 3_: El valor del día del mes, de **1 a 31**.
-   _Asterisco 4_: El valor del del mes, de **1 a 12**.
-   _Asterisco 5_: El valor del día de la semana, de **0 a 6** _Siendo el Domingo el 0 o el 7_, también se puede escribir el _sun, mon, tue, wed, thu, fri, sat_

Y a continuación de los valores temporales, introducimos el script que queremos ejecutar.

Es interesante exportar la información del script en un archivo, para comprobar que todo se esté ejecutando correctamente.

Algunos ejemplos útiles:
| **Código** | **Función** |
|------------|------------|
| 01 \* \* \* \* | Se ejecuta en el minuto 1 de cada hora, todos los días. |
| 15 8 \* \* \* | Se ejecuta a las 8:15 a.m de cada día. |
| 15 8 \* \* \* | Se ejecuta a las 08:15 de cada día. |
| 15 20 \* \* \* | Se ejecuta a las 20:15 de cada día. |
| 00 5 \* \* 0 | Se ejecuta a las 05:00 todos los Domingos. |
| 45 19 1 \* \* | Se ejecuta a las 19:45, el primer día de cada mes. |
| 01 \* 20 7 \* | Se ejecuta en el minuto 1 de cada hora del 20 de Julio. |
| 10 1 \* 12 1 | Se ejecuta a las 01:10, todos los lunes de Diciembre. |
| 00 12 16 \* Wen | Se ejecuta a las 12:00 los días 16 de cada mes, siempre que sea Miércoles. |
| 30 9 20 7 4 | Se ejecuta a las 09:30 del día 20 de Julio, siempre que sea Jueves. |
| 30 9 20 7 \* | Se ejecuta a las 09:30 del día 20 de Julio. |
| 20 \* \* \* 6 | Se ejecuta a en el minuto 20 de cada hora de los Sábados. |
| 20 \* \* 1 6 | Se ejecuta a en el minuto 20 de cada hora de los Sábados de Enero. |

También hay eventos predefinidos:
| **Código** | **Función** |
|------------|------------|
| @reboot | Se ejecuta una vez, después del inicio. |
| @yearly | Se ejecuta una vez al año (0 0 1 1 \*). |
| @annually | Es igual que @yearly. |
| @monthly | Se ejecuta una vez al mes (0 0 1 \* \*). |
| @weekly | Se ejecuta una vez a la semana (0 0 \* \* 0). |
| @daily | Se ejecuta una vez al día (0 0 \* \* \*). |
| @midnight | Es igual que @daily. |
| @hourly | Se ejecuta una vez por hora (0 \* \* \* \*). |

Ejemplo de cron que realiza un `ls` del directorio `/home/Miguelito/Desktop`, y que guarda el resultado en el archivo `/home/Miguelito/Desktop/listaEscritorio.log`.

```bash
0 */6 * * * ls /home/Miguelito/Desktop >> /home/Miguelito/Desktop/listaEscritorio.log 2>&1
```

Dejo [**este enlace**](https://www.generateit.net/cron-job/), en esta página puedes establecer el código de manera gráfica.
