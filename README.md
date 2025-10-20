# 💥 EXPLOTACIÓN DE BINARIOS CON STACK BUFFER OVERFLOW

Este repositorio es un ejemplo de aplicación de la metodología y análisis de vulnerabilidades y explotación en binarios. Como software vulnerable se utiliza el binario **vulnserver**, que se caracteriza por ser vulnerable a **Stack Buffer Overflow** principalmente.

--- 

## ***📇 Índice de contenidos***

* [1. Entorno de trabajo](#step1)
* [2. Configuración del entorno de trabajo](#step2)
  * [2.1 Descarga de vulnserver](#step2-1)
  * [2.2 Descarga de Immunity Debugger](#step2-2)
 
---
---

<a name="step1"></a>

## ***1. Entorno de trabajo***

Lista de las principales herramientas utilizadas que sirven como base para realizar el análisis y explotación de vulnerabilidades.

-	**Vulnserver**. Es el binario vulnerable. Se trata de un servidor que queda a la espera de una conexión y ofrece una serie de parámetros de entrada tras una conexión exitosa.

-	**Immunity Debugger**. Herramienta para depurar y analizar binarios, su código ensamblador, flujo de operación, etc.  

-	**Mona**. Es un plugin escrito en Python que se importa en Immunity Debugger para su uso.

-	**IDAFree**. Depurador, desensamblador y decompilador. Para la realización de técnicas de ingeniería inversa.

-	**Metasploit Framework**. Para los pasos finales de la explotación.

-	**Visual Studio Code**. IDE para la gestión del código de los exploits.

---

<a name="step2"></a>

## ***2. Configuración del entorno de trabajo***

A continuación de muestra dónde descargar y cómo instalar y configurar las herramientas.

<a name="step2-1"></a>

## ***2.1 Descarga de vulnserver***

- Visitar la página oficial de vulnserver (https://thegreycorner.com/vulnserver.html)
- Clonar el repositorio de vulnserver:
	- Abrir un CMD en la ruta deseada para la descarga y clonar:
```
git clone https://github.com/stephenbradshaw/vulnserver
```

 <a name="step2-2"></a>

## ***2.2 Descarga de Immunity Debugger***
- Al instalarlo se instala también Python 2.17 y se recomienda instalar el 3.14 o más actual.
