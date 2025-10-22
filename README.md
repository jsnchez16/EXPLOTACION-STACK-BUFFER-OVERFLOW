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
	
---
---

<a name="step1"></a>

## 💼 ***1. Entorno de trabajo***

Lista de las principales herramientas utilizadas que sirven como base para realizar el análisis y explotación de vulnerabilidades.

- **Vulnserver**. Es el binario vulnerable. Se trata de un servidor que queda a la espera de recibir conexiones en el puerto 9999 y ofrece una serie de parámetros de entrada tras una conexión exitosa.

- **Immunity Debugger**. Herramienta para depurar y analizar binarios, su código ensamblador, flujo de operación, etc. 

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

### ***3. Vulnserver en Immunity Debugger***

- Una vez abierto Immunity Debugger, en la barra de herramientas superior seleccionar _View -> CPU_ y maximizar.
- A continuación ir a _File -> Open_ y seleccionar el ejecutable de vulnserver.
- De esta manera se ejecuta una instancia del binario y se obtiene la información del binario, código ensamblador, registros, direcciones que utiliza, etc.
- En la esquina inferior derecha se indica el estado de ejecución del binario.

<img width="1918" height="1027" alt="image" src="https://github.com/user-attachments/assets/fcc776d6-0b8f-4a60-931c-5205f66d3081" />

---

<a name="step4"></a>

### ***4. Vulnserver en IDA Free***

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

### 🖥️ ***5. Ejecución del binario y conexión al servidor vulnerable***

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

### 🔎 ***6. Análisis***

Antes de comenzar con la explotación hay que entender la funcionalidad y estructura del programa que se pretende explotar. Para ello, el análisis de código tanto si se tiene acceso al código fuente como al ensamblador del binario y la ingeniería inversa son pasos esenciales previos a la explotación.

---

<a name="step6-1"></a>

### ***6.1. Análisis de ejecución***

Una de las principales maneras de entender en este caso el binario a explotar es ejecutándolo y comprobar qué se puede hacer con el en una ejecución normal, qué parámetros acepta, qué muestra, etc. Entra dentro de lo que se denomina **análisis dinámico**.

En el caso de nuestro binario vulnserver una vez realizada la conexión con el servidor se muestra un prompt para introducir el comando HELP. Al hacerlo, se muestra una lista de los comandos válidos y cómo utilizarlos. Cada comando debe venir acompañado de una cadena de caracteres.

<img width="455" height="356" alt="image" src="https://github.com/user-attachments/assets/cdabd144-395b-43bc-9be4-e52a25615223" />

---

<a name="step6-2"></a>

### ***6.2. Ingeniería inversa:***

- **Análisis de código fuente:** en el caso de disponer del código fuente del progarma, el análisis del mismo puede darnos pistas importantes sobre vulnerabilidades latentes. Esto puede deberse principalmente al uso de funciones inseguras como son en C _strcpy, gets, etc_.
	- _En este ejemplo se trabaja como si se careciera del código fuente._

En el caso de no disponer del código fuente se aplica ingeniería inversa sobre el binario ejecutable. Descomponiendo y analizando el comportamiento interno del softwar. Va de la mano con el análisis de ejecución.

El objetivo es identificar funciones, estructuras, rutas de ejecución y patrones sospechosos. Ejecutar el programa en un entorno controlado y observar su comportamiento en tiempo real utilizando debuggers y depuradores ayuda a comprender cómo se comporta el software ante determinadas entradas, cómo gestiona la memoria y qué validaciones aplica o no aplica.

Un ejemplo claro de lo que sería aplicar estas técnicas de ingeniería inversa es desensamblar o decompilar el binario a través de herramientas como IDA, Ghidra, Immunity Debugger o xDBG, obteniendo de esta manera el código ensamblador e incluso el pseudocódigo.

---

<a name="step6-2-1"></a>

### ***6.2.1 Ingeniería inversa en IDA Free***

Para obtener y entender el código ensamblador del binario así como el flujo de control del programa se puede emplear la herramienta IDA Free.

- Una vez abierto el programa con el binario cargado, **ir a View -> Open subviews -> Generate pseudocode (F5)**.

<img width="538" height="128" alt="image" src="https://github.com/user-attachments/assets/ed009768-aca5-45fc-ad55-9acd7a449a86" />

- En la ventana de IDA View-A, **clic derecho -> Synchronize with -> Pseudocode-A** . Se obtiene una sincronización entre el código ensamblador y el pseudocódigo generado al seleccionar cualquier línea de código. 

<img width="985" height="674" alt="image" src="https://github.com/user-attachments/assets/f243a022-4cc8-452a-8920-376b64505a07" />

De esta manera se obtiene una imagen más visual de a qué corresponde cada parte del código ensamblador.

<img width="1489" height="686" alt="image" src="https://github.com/user-attachments/assets/def9d2bf-3dcb-4547-82d5-ae17ff0027af" />

---

<a name="step7"></a>

### 💥 ***7. Explotación***

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

Por cada envío incremental se abre y cierra la conexión al servidor hasta que se produce una excepción en la ejecución el binario, indicando el tamaño del buffer enviado con el que se ha producido el crash.
