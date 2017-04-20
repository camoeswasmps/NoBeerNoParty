No beer, no party
=================

No te pierdas la oportunidad de conocer en un taller 8 servicios de Azure en 45 minutos. Te dar� todas las piezas (aplicaciones) y conceptos. Se trata de crear tu infrastructura en Azure, conectar las piezas y disfrutar del experimento!

### Necesitas:

-   Visual Studio 2017 con asp.net core  

-   Subscripci�n Azure (vale la de prueba)

-   NO es necesario saber programar, ni ser�a viable hacerlo en 45 min! Se trata de descargar las aplicaciones y configurar settings una vez creados servicios en Azure. 

-   Se puede realizar el taller en grupos de 2 o m�s personas, es incluso recomendable.
�
### Conocer�s:

-   Web Apps
-   Azure Search
-   Event Hubs
-   ... 


Escenario
=========

Estamos en visperas de la celebraci�n de la mayor feria de la cerveza en la ciudad de Barcelona es un evento anual al que se prevee que asistan unas 10.000 personas seg�n el sistema de registro. La organizaci�n dispone de un recinto con 10 stands, en cada uno pueden conectarse 10 barriles. 

Punto de partida
----------------

-   La organizaci�n dispone de una p�gina web que puede ser consumida desde un dispositivo smart phone.
-   Todo asistente al evento debe haberse registrado previamente (tambi�n los fabricantes), esta informaci�n la tenemos disponible en un conjunto de datos. Al registrarse, el usuario recibe unas claves de acceso.   
-   Disponen de una aplicaci�n en las m�quinas registradoras de cada stand que se encarga de registrar cada compra (usuario, modelo cerveza, fecha).


�Qu� nos pide la organizaci�n?
------------------------------

Crear la infrastructura necesaria para dar soporte a este evento y conectar sus aplicaciones. Nos facilitan un fichero csv con la informaci�n del registro, la aplicaci�n web y la aplicaci�n de cajero para que podamos implementar all� lo que consideremos necesario.
�
### Cervezeros (Web)

-   Buscador de cervezas entre los barriles conectados en stands.  
-   Puntuar un modelo (simulado por la aplicaci�n de consola..)

### Organizaci�n (Cajero)

-   Almacenar todas las transacciones para explotar esa informaci�n en un futuro.


Nuestra propuesta
-----------------

![](https://github.com/ccanizares/NoBeerNoParty/raw/master/assets/nobeernoparty.png)

### Azure Search
Permitir� a la web buscar modelos de cerveza entre los stands y tener informaci�n precisa en todo momento de los modelos mejor valorados, cantidad aproximada restante de cada modelo en los stands, etc. 

### Event Hub
Los cajeros enviar�n informaci�n sobre cada compra, este servicio permite la ingesta de informaci�n codificada como stream. 

### Web App
Servicio de aplicaciones escalables en la nube de Azure publicando la aplicaci�n web desde Visual Studio.

### Azure Functions
Este servicio tiene como objetivo insertar los datos que pasan por event hub en document db.

### DocumentDB 
Ser� donde almacenemos toda aquella informaci�n que quiere disponer la organizaci�n al finalizar el evento para su futuro analisis.


Go!
---

### Crear recursos en azure

1.  Grupo de recursos: Agrupa todos los recursos en un grupo de manera que ser� m�s c�modo luego gestionarlos.

2.  Web App: Alojaremos aqu� la web que nos facilitan.

3.  Azure Search: De momento no configuramos ning�n indice, lo haremos m�s adelante v�a Rest Api.

4.  Document Db: Creamos una base de datos vac�a. (Si vas justo de tiempo o quieres evitar el coste del recurso, este recurso + Azure functions no son necesarios para la demo de Azure Search que tenemos implementada en la web)

5.  Event Hub: Creamos dos hubs, rating y tickets. 

6.  Azure Function: Creamos dos funciones, ratings y tickets. Como trigger apuntamos a los correspondientes hubs de eventos y outputs a documentdb. El c�digo las functions est� disponible aqu�. 

�
### Conectando piezas

1.  Descarga la aplicaci�n de Consola (simulaci�n de la aplicaci�n cajero) y a�ade los datos de conexi�n a Event Hub, Azure Search y si has optado por crear documentDb.

2.  Descarga la aplicaci�n web y a�ade las cadenas de conexi�n necesarias para comunicarse con Azure Search. 

3.  Despliega la web (bot�n derecho y publicar).

4.  Ejecuta la aplicaci�n y elige una opci�n.
<ul>
<li>Opci�n 1: Esto popula la base de datos de document db. (Opcional, no es necesario para la simulaci�n de Azure Search)</li>
<li>Opci�n 2: Crea el �ndice de b�squeda en Azure Search.</li>
<li>Opci�n 3: Popula el �ndice de b�squeda con los datos iniciales.</li>
<li>Opci�n 4: Inicia la simulaci�n. (Aseg�rate de que todo est� funcionando ok..).</li>
</ul>

�
### Explotando el sistema

1.  Accede a la web que hemos publicado y usa la funcionalidad de b�squeda.

A medida que se generan tickets y valoraciones deber�as ir viendo como van cambiiando las cervezas m�s valoradas y la cantidad que hay en cada dispensador.

2.  Con�ctate al portal de Azure y comprueba que los datos del evento se est�n almacenando en Document Db. 

Ves al explorador de b�squeda de document db y mira los documentos que hemos almacenado en formato json. 