# 💥 EXPLOTACIÓN DE BINARIOS CON STACK BUFFER OVERFLOW
Este repositorio es un ejemplo de aplicación de la metodología y análisis de vulnerabilidades y explotación en binarios. Como software vulnerable se utiliza el binario **vulnserver**, que se caracteriza por ser vulnerable a **Stack Buffer Overflow** principalmente.
---
--- 
## ***📇 Índice de contenidos***
* [1. Entorno de trabajo](#step1)
* [2. Configuración del entorno de trabajo](#step2)
  * [2.1 Descarga de vulnserver](#step2-1)
---
---

<a name="step1"></a>  
## ***1. Entorno de trabajo***
Lista de las principales herramientas utilizadas que sirven como base para realizar el análisis y explotación de vulnerabilidades.

-	**Vulnserver** (https://github.com/stephenbradshaw/vulnserver). Es el binario vulnerable con el que se trabaja. Se trata de un servidor que queda a la espera de una conexión.

-	**Immunity Debugger**. Herramienta para depurar el binario vulnerable y conseguir explotar la vulnerabilidad. Al instalarlo se instala también Python 2.17 y se recomienda instalar el 3.14 o más actual. 

-	**Mona**. Herramienta de Python para facilitar la explotación. Es un plugin que se importa en Immunity Debugger para su uso.

-	**IDAFree**. Depurador, desensamblador y decompilador para la realización de técnicas de ingeniería inversa.

-	**Metasploit Framework**. Para pasos finales de la explotación.

-	**Visual Studio Code**. IDE para la gestión del código de los exploits.


<a name="step2"></a>
## ***2. Configuración del entorno de trabajo***

<a name="step2-1"></a>
## ***2.1 Descarga de vulnserver***
