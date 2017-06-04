# **Asignatura:** Sintaxis y Semantica de los Lenguajes de Programacion
## **Curso:** K2051
## **Año de cursada:** 2017
## **Numero de equipo:** 5

## **Integrantes**

### Tomas Benz	|	tomasbenz	
### Ignacio Campaño	| IgnacioCampano
### Martín DiPaolo	| martindipaolo
### Emiliano Guiggiaro	| eguiggiaro
### Agustin Yañez	| agustinyanez

## **TP:** 3 - Removedor de comentarios

***
### Autómata finito

* Diagrama de transiciones

	![Con titulo](TP3.png "Diagrama de transiciones")

* Definición formal

M = (Q, E, T, q0, F), dónde:

	* Q = {Programa,Caracteres, Cadena, InicioComentario, Multilinea, Unilinea, FinComentario};
	* E = {caracteres y símbolos ASCII};
	* q0 = Programa;
	* F = {Programa};
	* T = {Programa => ' => Caracteres, Caracteres => ' => Programa, Caracteres =>  No apóstrofo => Caracteres, Programa => \ => Cadena, Cadena => \ => Programa, Cadena => No comilla => Cadena, Programa => / => InicioComentario, InicioComentario => * => Multilinea, InicioComentario => / => Unilinea, Multilinea => No asterisco => Multilinea, Unilinea => No nueva línea => Unilinea, Multilinea => * => FinComentario, FinComentario => No barra => Multilinea, FinComentario => / => Programa, Programa => {No apostrofo, No comilla, No barra} => Programa, Unilinea => \n => Programa}.

* Expresión regular

	* Cadena: "[^"]*"
	* Caracteres: '[^']*' 
	* Comentario simple (Unilinea):  [^"']\/\/[^\/\/]*[\n]+[^"']
	* Comentario multilinea: [^"']\/\*(\*(?!\/)|[^*])*\*\/[^"']

***
### Descripción de la implementación A: rc-a.c.

La implementación A se basa en el autómata descripto, y utiliza etiquetas GOTO para hacer efectivas las transiciones.

Dentro de cada etiqueta (relacionadas con los estados correspondientes) se utilizó la sentencia switch para validar el carácter de la transición, y el estado final correspondiente. 

En la implementación, el final de la ejecución está representado por el fin del archivo (EOF).

***
### Descripción de la implementación B: rc-b.c.

La implementación B se basa en el autómata descripto, y utiliza recursividad para hacer efectivas las transiciones.

Dentro de cada función (relacionadas con los estados correspondientes) se utilizó la sentencia switch para validar el carácter de la transición, y el estado final correspondiente. 

En la implementación, el final de la ejecución está representado por el fin del archivo (EOF).
 

***

### Benchmark


| Implementación / Orden |   KB   |   MB   |   GB   |
|:----------------------:|:------:|:------:|:------:|
|        A (GOTO)        | 0.34 seg | 0.73 seg | 77 seg |
|    B (Recursividad)    | 0.57 seg | ERROR (*) | ERROR (*) |

(*) Se genera un error en la ejecución de la implementación B, tanto para el test en el orden de los MB, como en el orden de los GB. La ejecución del programa finaliza con error luego de comenzada la ejecución. Entendemos que esto se debe a las llamadas recursivas que se incluyeron en la segunda implementación. Esto hace que la implementación A sea la solución más eficaz, pero además, la  más eficiente dado que consume considerablemente menos recursos que la B.
