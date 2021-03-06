# # Hoja de ruta - Estación metereológica con NodeMcu.

_Modesto proyecto para afianzar conocimientos adquiridos en varios ámbitos._
_Este proyecto está basado en la colaboración de todas esas personas que comparten desisteresadamente sus trabajos. Está modificado para conseguir lo deseado._
_En algun lugar de este readme.me intentaré añadir todos los enlaces, gracias a los cuales, espero conseguir realizar la estación meteorológica._

Descripción del proyecto
_La estación medirá la temperatura, humedad, señal RSSI wifi, tanto interior como exterior, presión, rayos UV-A y UV-B, velocidad y dirección del viento y lluvia. Los datos se visualizarán en un dashboard creado 
en la plataforma de gestión de datos Thinger.io. Además con otro NodeMCU donde se obtendrán los datos de temperatura, humedad y señal RSSI wifi, interior, se mostrarán en una pantalla Nextion de 7".


## Material necesario 🚀

_El material utilizado se puede modificar pero se tendrá que hacer lo propio con el código._

**NodeMCU**

![alt text](https://github.com/RaulMallorca/Estacion_metereologica/blob/master/pictures/NodeMCUv3.jpg)

![alt text](https://github.com/RaulMallorca/Estacion_metereologica/blob/master/pictures/NodeMCU-pines.png)

Poco que añadir a esta placa. http://nodemcu.com/index_en.html. Basada en el ESP8266.

Tiene un puerto micro USB

Conversor Serie-USB, el CH340G

Tiene pines, un LED y dos botones, uno para reinicio y otro para flasheo.

Se alimenta a 3,3V.

3 salidas de 3,3V

1 de 5 (Solo da 5V si en las otras no hay nada conectado)

La v3 que yo tengo, solo se puede usar a 9600 bps Al establecer conexión mediante el puerto serie, *Serial.begin (9600);*

Necesataremos 2 placas.

**SENSORES**

**BME280** 	

![alt text](https://github.com/RaulMallorca/Estacion_metereologica/blob/pictures/BMP280.jpg?raw=true)

• Temperatura (rango de -40 a + 85 °C, precisión ±1 °C y resolución 0,01 °C)

• Humedad (a 100%, con una precisión de ±3% Pa y una resolución de 0.008%)
            
• Presión (300-1100 hPa, precisión de ±1 Pa, y resolución de 0,18 Pa)         

• Current consumption 
 -1.8 μA a 1Hz - Humedad y temperatura
 - 2.8 μA a 1Hz - Presión y temperatura
 - 3.6 μA a 1Hz - Humedad, presión y temperatura
 - 0.1 μA in sleep mode
- V 3,3V

Necestaremos 2 sensores.

**BH1750**

![alt text](https://github.com/RaulMallorca/Estacion_metereologica/blob/master/Sensores/BH1750/foto.png)

Las características del sensor **BH1750** son las siguientes:
- Interfaz I2C con dos posibles direcciones. 0x5C y 0x23 (por defecto).
- Representación del espectro apróximadamente a la del ojo humano.
- Alta resolución, (1 - 65535 lx) 
- Baja dependencia al tipo de luz. (Incandescente, fluorescente, halógena, LED, sol)
- Resutado de medición ajustable teniendo en cuenta el "cascarón" que cubre el sensor. (Es posible detectar un mínimo de 0,11 lx, un máximo de 100000 lx utilizando esta función.)
- Baja influencia de luz infrarroja.

Especificaciones:
- Voltaje
  - mín 2,4V
  - max 3,6V
- Resolución 4lux, 1lux, 0,5lux. Usando la resolución de 1lux permite distinguir iluminaciones por debajo de los 10lux (luz crepúscular) Para 1 lux y 4 lux se usan los 16bits de datos, llegando a los 65535 lux (día soleado sin luz directa) En el modo 0,5 lux usa 15 bits y puede representar un valor máximo de 32767 lux (exterior sin luz directa) [Fuente](http://polaridad.es/bh1750-luz-sensor-iluminacion-ambiental-i2c-medida-luminosidad-medicion/)
- Consumo
  - mín 0,01 μA
  - max 190 μA

Necestaremos 1 sensor.

**SENSOR LLUVIA YL-83**

![alt text](https://github.com/RaulMallorca/Estacion_metereologica/blob/master/pictures/Sensor_lluvia.jpg)

Output

• Maximum voltage 15 V

• Maximum current 50 mA

Input

• Control to switch heater OFF

Open circuit input enables the heater.

Connection to GND disables the heater.

• Contact rating min. 15 V, 2 mA

• Supply voltage 12 VDC ± 10 %

• Supply current

• Typical less than 150mA

• Maximum 260mA

• Supply voltage 3,3 VDC (I'm not sure of this counting)

• Supply current

• Typical less than 41mA

• Maximum 70mA


**VELOCIDAD DEL VIENTO**

**UV ML8511 y ADC ADS1115**

![alt text](https://github.com/RaulMallorca/Estacion_metereologica/blob/master/pictures/ML8511.jpg)

• Longitud de onda: 280-390nm

• Current consumption  300 μA 
                      standby current 0.1 μA

El sensor **UV ML8511** al necesitar dos pines analógicos y el NodeMCU disponer solo de 1, necesitamos un multiplexor/desmultiplexor, como el ADS1115.

![alt text](https://github.com/RaulMallorca/Estacion_metereologica/blob/master/Sensores/ML8511_ADS1115ADC/ADS1115_ADC_pines.png)

Según el ejemplo de Sparkfun, las conversiones analógicas a digitales dependen completamente del voltaje. Si alimentamos el sensor mediante la placa y esta a través de un USB, el voltaje puede variar de 5,25V a 4,75V. Por eso utilizaremos los 3,3V del NodeMCU. El pin marcado con **3,3V** del sensor irá conectado a **3,3V** y al **pin A1** del ADS1115. El pin **Out** irá al pin analógico **A0** del ADS1115. El **EN** ira a **3,3V** también. En el apartado de sensores/Ml8511...puedes ver el esquemático.


**Pantalla Nextion NX8048P070-011C-Y**

![alt text](https://github.com/RaulMallorca/Estacion_metereologica/blob/master/pictures/Nextion%20NX8048P070-011C-Y.jpg)


Pantalla táctil TFT capacitiva colores reales RGB 65K.

Memoria Flash de 128MB para código de aplicación de usuario y datos.

EEPROM: 1024 bytes

RAM: 512 KB

Interfaz: interfaz serie XH2.54 4 pin TTL

Voltaje de entrada: 4,7-7 V (estándar de 5 V)

Corriente de entrada: 750mA (brillo máximo), 170mA (modo de suspensión). Potencia recomendada: 5V mínimo 1A

Altavoz: 0,5 vatios de capacidad nominal, impedancia de 16 Ω

Tamaño de la pantalla: 7,0 pulgadas

Resolución de la pantalla: 800x480 píxeles

Brillo de la pantalla: 300 cd/m ^ 2,(0 ~ 300 nit, el intervalo de ajuste es 1%)

Ranura para tarjeta micro SD integrada, FAT32. Adecuado para una tarjeta SD de hasta 32GB

Fuente de alimentación Micro USB macho.

Visual: 154,08mm(L) x 85,92mm(W)

Dimensiones: 218,1x150x22,5mm.

**Cajas**

Una para guardar los sensores y el NodeMCU exterior.

Una para guardar los sensores y el NodeMCU interior.

Una para la pantalla Nextion

### Pre-requisitos 📋

_Software, drivers y extras a instalar_

Un IDE para crear el skecht. Yo he usado el IDE de Arduino, concretamente el 1.8.13. https://www.arduino.cc/en/pmwiki.php?n=main/software.

Drivers para el conversor Serie-USB CH340G. Lo puedes encontrar fácilmente por internet.

### Instalación 🔧

_Una serie de ejemplos paso a paso que te dice lo que debes ejecutar para tener un entorno de desarrollo ejecutandose_

_Dí cómo será ese paso_

```
Da un ejemplo
```

_Y repite_

```
hasta finalizar
```

_Finaliza con un ejemplo de cómo obtener datos del sistema o como usarlos para una pequeña demo_

## Ejecutando las pruebas ⚙️

_Explica como ejecutar las pruebas automatizadas para este sistema_

### Analice las pruebas end-to-end 🔩

_Explica que verifican estas pruebas y por qué_

```
Da un ejemplo
```

### Y las pruebas de estilo de codificación ⌨️

_Explica que verifican estas pruebas y por qué_
```
Da un ejemplo
```

## Despliegue 📦

_Agrega notas adicionales sobre como hacer deploy_

## Construido con 🛠️

_Menciona las herramientas que utilizaste para crear tu proyecto_

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - El framework web usado
* [Maven](https://maven.apache.org/) - Manejador de dependencias
* [ROME](https://rometools.github.io/rome/) - Usado para generar RSS

## Contribuyendo 🖇️

Por favor lee el [CONTRIBUTING.md](https://gist.github.com/villanuevand/xxxxxx) para detalles de nuestro código de conducta, y el proceso para enviarnos pull requests.

## Wiki 📖

Puedes encontrar mucho más de cómo utilizar este proyecto en nuestra [Wiki](https://github.com/tu/proyecto/wiki)

## Versionado 📌

Usamos [SemVer](http://semver.org/) para el versionado. Para todas las versiones disponibles, mira los [tags en este repositorio](https://github.com/tu/proyecto/tags).

## Autores ✒️

_Menciona a todos aquellos que ayudaron a levantar el proyecto desde sus inicios_

* **Andrés Villanueva** - *Trabajo Inicial* - [villanuevand](https://github.com/villanuevand)
* **Fulanito Detal** - *Documentación* - [fulanitodetal](#fulanito-de-tal)

También puedes mirar la lista de todos los [contribuyentes](https://github.com/your/project/contributors) quíenes han participado en este proyecto. 

## Licencia 📄

Este proyecto está bajo la Licencia (Tu Licencia) - mira el archivo [LICENSE.md](LICENSE.md) para detalles

## Expresiones de Gratitud 🎁

* Al usuario [Ega] https://community.thinger.io/u/ega, de la comunidad Thinger.io . Por la ayuda prestada para conseguir obtener los datos desde Thinger.io🤝 
* A Luis de [Programarfacil.com] http://programarfacil.com. Detonante para decidirme a realizar mi propia estación.🙌
* [esploradores.com] http://esploradores.com 
* etc.



---
⌨️ con ❤️ por [Villanuevand](https://github.com/Villanuevand) 😊
