## LG IR
***Transmisor IR (modos service) para LG***

![](https://github.com/Arturrito63/LG_IR/blob/main/Docs/esquema.jpg) 

### Descripción...

Es este proyecto utilizo el MCU ATtiny85-20 ya que no necesito conectar más de 3 botones y un led IR. En el diagrama de arriba se muestra alimentado con 9Vots, el cual se reduce a 5Volts mediante un regulador 78L05, pero pueden reemplazar el regulador con un conector USB y usarlo con una fuente de 5VDC regulados.

El Protocolo NEC utiliza una portadora (carrier) de 38KHhz sobre la cual se envían los pulsos de marca (inicio), dirección (addr) y datos (data).
[Protocolo NEC](https://www.sbprojects.net/knowledge/ir/nec.php)

Para obtener los 38Khz utilizo el TIMER0 en modo CTC y obtengo su salida en PB0, PB0 y PB1 se configuran como salida, donde PB0 sera una salida push-pull mientras que PB1 se configura como pull-up.
Los bits en PORTB para los pines PB1, PB2, PB3 y PB4 se configuran en 1 (pull-up), PB1 será una salida mientras que los restantes son entradas para los pulsadores.

Al conectar el LED IR con una resistencia en serie de 330 Ohms a los pines PB0 (anodo) y PB1 (catodo), este solo emitirá los 38khz presentes en PB0 cuando haya un 0 lógico en PB1. Esto permite que una vez iniciado el TIMER0 y su salida presente en PB0, el programa solo debe generar la trama de pulsos correpondientes al pulsador presionado.

*Si no se presiona ningún botón (pulsador), el programa ejecuta un Power Down y el MCU se apaga hasta que se produzca una interrupción PCINT al presionar algún botón.*

El pulsador conectado al pin PB2 envía los dos primeros bytes (addr + data) almacenados al inicio de la EEPROM, 0x0000 = addr y 0x0001 = data.  
El pulsador conectado al pin PB3 hará lo mismo con los dos siguientes bytes y asi lo hará PB4 con el tercer par de bytes.  
Los datos almacenados el el archivo "eeprom.eep" en la carpeta LG_IR\Debug contienen los tres pares de bytes para los mandos InStart, EzAdjust y PowerOnly respectivamente.  
Este archivo debe grabarse en la EEPROM del ATtiny85 junto con el archivo LG_IR.hex también el la misma carpeta que deberá grabarse en la memoria FLASH.  

Los pares de bytes son los siguientes:  
0x0000  04  
0x0001  FB  
0x0002  04  
0x0003  FF  
0x0004  04  
0x0005  EF 

### InStart
![](https://github.com/Arturrito63/LG_IR/blob/main/Docs/LG_InStart.jpg)


### EzAdjust
![](https://github.com/Arturrito63/LG_IR/blob/main/Docs/LG_EzAdjust.jpg)

### PowerOnly
![](https://github.com/Arturrito63/LG_IR/blob/main/Docs/LG_PowerOnly.jpg)

