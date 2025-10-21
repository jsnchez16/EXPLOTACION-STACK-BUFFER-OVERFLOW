# 💥 EXPLOTACIÓN DE BINARIOS CON STACK BUFFER OVERFLOW

Este repositorio es un ejemplo de aplicación de la metodología y análisis de vulnerabilidades y explotación en binarios. Como software vulnerable se utiliza el binario **vulnserver**, que se caracteriza por ser vulnerable a **Stack Buffer Overflow** principalmente.

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
---
---

<a name="step1"></a>

## ***1. Entorno de trabajo***

Lista de las principales herramientas utilizadas que sirven como base para realizar el análisis y explotación de vulnerabilidades.

- **Vulnserver**. Es el binario vulnerable. Se trata de un servidor que queda a la espera de una conexión y ofrece una serie de parámetros de entrada tras una conexión exitosa.

- **Immunity Debugger**. Herramienta para depurar y analizar binarios, su código ensamblador, flujo de operación, etc. 

- **Mona**. Es un plugin escrito en Python que se importa en Immunity Debugger para su uso.

- **IDA Free**. Depurador, desensamblador y decompilador. Para la realización de técnicas de ingeniería inversa.

- **Metasploit Framework**. Para los pasos finales de la explotación.

- **Otros deseables:**
  
	- **Visual Studio Code**
 	- **Git**

---

<a name="step2"></a>

## ***2. Configuración del entorno de trabajo***

A continuación de muestra dónde descargar y cómo instalar y configurar las herramientas.
Se recomienda trabajar en una misma carpeta.

<a name="step2-1"></a>

### ***2.1 Descarga de vulnserver***

- Visitar la página oficial de vulnserver (https://thegreycorner.com/vulnserver.html).
- Clonar el repositorio de vulnserver.
	- Abrir un CMD en la ruta deseada para la descarga y clonar con Git:
```
git clone https://github.com/stephenbradshaw/vulnserver
```
- El contenido del repositorio incluye el código, librerías y otros archivos entre los que se encuentra el binario ejecutable.

<img width="614" height="237" alt="image" src="https://github.com/user-attachments/assets/e7244b09-c73d-4cf6-b99c-a56a12520999" />

<a name="step2-2"></a>

### ***2.2 Descarga de Immunity Debugger***

- ***Actualmente la página oficial de descarga está caída (https://debugger.immunityinc.com/).***
- Se puede descargar en el siguiente repositorio (https://github.com/kbandla/ImmunityDebugger/releases/tag/1.85).
- Click sobre el fichero .exe y se iniciará la descarga.

<img width="413" height="470" alt="image" src="https://github.com/user-attachments/assets/19762637-91b3-4689-9164-6e7add71718d" />

<a name="step2-3"></a>

### ***2.3 Instalación de Immunity Debugger***
- Una vez descargado, localizar el ejecutable setup y doble click para comenzar el proceso de instalación.
- Aceptar los términos y condiciones e instalar siguiendo el proceso.
  
<img width="481" height="331" alt="image" src="https://github.com/user-attachments/assets/4457e17c-07ac-474e-bb1c-59bb343cda00" />

- Posteriormente, localizar el ejecutable Immunity Debugger para ejecutar el programa haciendo doble click. Por defecto, en la instalación se genera la carpeta _C:\Program Files (x86)\Immunity Inc\Immunity Debugger_ 

<img width="613" height="516" alt="image" src="https://github.com/user-attachments/assets/8629c3a7-42ce-4b2c-9e54-dfe6a513ef91" />

---

<img width="613" height="516" alt="image" src="https://github.com/user-attachments/assets/22133dec-4197-48ac-9bae-adb13c81c781" />

<a name="step2-4"></a>

### ***2.4 Descarga de Mona***

- La página oficial de Mona (https://www.corelan.be/index.php/2011/07/14/mona-py-the-manual/)
- Clonar el repositorio de mona.py.
	- Abrir un CMD en la ruta deseada para la descarga y clonar con Git:
```
git clone https://github.com/corelan/mona.git
```

<a name="step2-5"></a>

### ***2.5 Instalación de Mona***

- Una vez descargado el repositorio de Mona, copiar el fichero **mona.py** a la carpeta **PyCommands** de Immunity Debugger como administrador. Por lo general en _C:\Program Files (x86)\Immunity Inc\Immunity Debugger\PyCommands_ .

- Abrir Immunity Debugger como administrador y ejecutar el comando '**!mona**' en la barra inferior para comprobar que está bien importado. Se mostrará el manual de ayuda.

<img width="1914" height="1032" alt="image" src="https://github.com/user-attachments/assets/cfcb9822-5c0f-4332-85be-febb04a73d3c" />

<a name="step2-6"></a>

### ***2.6 Descarga de IDA Free***

- La página oficial de IDA Free para la descarga (https://hex-rays.com/pricing?section=individuals)

- Seleccionar el paquete Free y descargar siguiendo las instrucciones.

<a name="step2-7"></a>

### ***2.7 Instalación de IDA Free***

- Localizar el archivo setup descargado.
- Doble click para iniciar el proceso de instalación.
- Seguir aceptando los términos y condiciones y seleccionando el directorio de instalación.
- _Al instalarlo se instala también Python 2.17 y se recomienda instalar el 3.14 o más actual._
