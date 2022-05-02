# exowings-ubuntu
Repositorio de archivos varios e instrucciones para hacer funcionar Ubuntu GNU/Linux en la notebook Exo Wings K2200.
Tomando la ayuda de ARCADENEA que realizo la instalación de fedora 29


Hardware:
=========
- CPU: Intel(R) Atom(TM) x5-Z8300  CPU @ 1.44GHz (CherryTrail)
- RAM: 2 GB DIMM DDR3 Synchronous 1066 MHz (0,9 ns)
- Video: VGA compatible controller: Intel Corporation Atom/Celeron/Pentium Processor x5-E8000/J3xxx/N3xxx Series PCI Configuration Registers (rev 22)
- Audio: Ensoniq es8316 (Intel SST)
- WiFi: AMPAK AP6212 - Broadcom 43438a0
- Bluetooth: AMPAK AP6212 - ?
- Touchscreen: Silead gsl1680
- Touchpad: 1017:1006 Speedy Industrial Supplies, Pte., Ltd
- Teclado: 1017:1006 Speedy Industrial Supplies, Pte., Ltd
- Pantalla: Resolución máxima 800x1280
- Acelerometro: Bosch 0200
- Cámara delantera: Omnivision OVTI2680
- Cámara trasera:  Omnivision OVTI2680

ICs
===
- 1x GSl3676
- 1x AP6212
- 1x ES8316 - EB34X405
- 4x 6MP47 - D9SHD
- 1x SR29Z
- 1x BJT631 - 25lQ64CVIG
- 1x AXP288C

Instalación
=========== 
Para la instalación de UBUNTU 20.04 (64 bit) se realizó mediante la creación de un usb booteable, mediante el programa rufus


WiFi
====
La notebook integra un adaptador SDIO combinado de Wifi+Bluetooth, un AMPAK AP6212. Ubuntu trae integrado este módulo por lo que no hacer falta instalar nada, funciona perfectamente.

Teclado
=======
Funciona correctamente.


Touchpad
========
Funciona correctamente.


Bluetooth
=========
Funciona correctamente


Pantalla
========
El control de brillo funciona correctamente

La pantalla durante la instalación aparece correctamente orientada, no así el puntero del mouse que se encuentra intertido, y la orientación 
del puntero al presionar tambien aparece invertido.
Por lo que realice la instalación del mismo si usar el mouse solo con las conbinaciones de las teclas

Touchscreen
===========
Funciona, solo es necesario despues de realizar la instalación realizar lo siguiente:
Verificar si existe la carpeta silead en la dirección /lib/firmware/ si no se encuentra se debe crear mediante el siguiente comando:

sudo mkdir firmware_00.fw /lib/firmware/silead

luego se debe realizar el comando que copia el archivo:

sudo cp firmware_00.fw /lib/firmware/silead/mssl1680.fw

Luego configurar la matriz con los siguientes pasos:

1) Crear un archivo de configuración:
sudo touch /etc/udev/rules.d/98-touchscreen-cal.rules

2) Editarlo y copiar el texto incluido:
sudo vi /etc/udev/rules.d/98-touchscreen-cal.rules

ATTRS{name}=="silead_ts", ENV{LIBINPUT_CALIBRATION_MATRIX}="0 -3.7 1 -2.4 0 1 0 0 1"


3) Recargar reglar de udev:
sudo udevadm control --reload-rules
sudo udevadm trigger


Audio
=====
Funciona, perfecto no se debe hacer nada

Acelerómetro
============
La pantalla viene por defecto configurada en modo vertical, para rotarla permanentemente es necesario ajustar el acelerómetro. Entonces ejecutar lo siguiente:

1) Crear una copia del archivo "/usr/lib/udev/hwdb.d/60-sensor.hwdb" en la carpeta Documentos para tener una copia
      sudo cp 60-sensor-hwdb /home/(nombre de usaurio)/Documentos

3) Modificar el archivos "/usr/lib/udev/hwdb.d/60-sensor.hwdb":
    
    sudo nano /usr/lib/udev/hwdb.d/60-sensor.hwdb
    " en la linea 287 escribir lo siguiente:
      ###########################################
      # EXO WINGS
      ###########################################
      sensor:modalias:acpi:BOSC0200*:dmi:bvnINSYDECorp.:bvrEXO9x.WT210P.VnBJREA01:*
       ACCEL_MOUNT_MATRIX=-1, 0, 0; 0, 1, 0; 0, 0, 1
   Se presiona Ctrl+o, y luego Ctrl+x"
   
4) Ejecutar los siguientes comandos:

sudo systemd-hwdb update
sudo udevadm trigger

5) Se debe reiniciar el sistema.


Cámara
============
No he podido hacerla funcionar

