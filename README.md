No beer, no party
=================

No te pierdas la oportunidad de conocer en un taller 8 servicios de Azure en 45 minutos. Te dar� todas las piezas (aplicaciones) y conceptos. Se trata de crear tu infrastructura en Azure, conectar las piezas y disfrutar del experimento!

### Necesitas:

-   Visual Studio 2017 con asp.net core  

-   Subscripci�n Azure (vale la de prueba)

-   NO es necesario saber programar, ni ser�a viable hacerlo en 45 min! Se trata de descargar las aplicaciones y configurar settings una vez creados servicios en Azure. 

-   4 o 5 cafes en vena para estar a tope que tenemos poco tiempo y muchas piezas que conectar XD. Se puede realizar el taller en grupos de 2 o m�s personas, es incluso recomendable.
�
### Conocer�s:

-   Web Apps
-   Azure Search
-   Stream Analytics
-   Azure Functions
-   Event Hub
-   Service Bus


Escenario
=========

Estamos en visperas de la celebraci�n de la mayor feria de la cerveza en la ciudad de Barcelona es un evento anual al que se prevee que asistan unas 10.000 personas seg�n el sistema de registro. La organizaci�n dispone de un recinto con 100 stands, en cada uno pueden conectarse 10 barriles. 
Todos los artesanos y hipsters de la cerveza quieren dar a conocer su marca y s�lo les est� permitido tener un barril en cada stand. No todos los fabricantes tendr�n stock para paliar la sed de tanto cervezero a lo largo de las 24 horas que dura el evento, por tanto si un fabricante renuncia a su plaza en el stand es una oportunidad para que otro pueda llenar ese hueco con su barril y as� dar a conocer a m�s gente su querida cerveza.   

Punto de partida
----------------

-   La organizaci�n dispone de una p�gina web que puede ser consumida desde un dispositivo smart phone.
-   Todo asistente al evento debe haberse registrado previamente (tambi�n los fabricantes), esta informaci�n la tenemos disponible en un conjunto de datos. Al registrarse, el usuario recibe unas claves de acceso.   
-   Disponen de una aplicaci�n en las m�quinas registradoras de cada stand que se encarga de registrar cada compra (usuario, modelo cerveza, fecha).
�
�Qu� nos pide la organizaci�n?
------------------------------

Crear la infrastructura necesaria para dar soporte a este evento y conectar sus aplicaciones. Nos facilitan un fichero csv con la informaci�n del registro, la aplicaci�n web y la aplicaci�n de cajero para que podamos implementar all� lo que consideremos necesario.  
�
### Cervezeros (Web)

-   Buscador de cervezas entre los barriles conectados en stands.  
-   Puntuar un modelo (1 a 5).

### Fabricantes (Web)

-   Consultar la cantidad de cerveza que queda en sus barriles. 
-   Consultar huecos en stands y enviar solicitudes (las solicitudes se aprueban o deniegan autom�ticamente seg�n un algoritmo avanzado que se llama tonto el �ltimo).
�
### Organizaci�n (Cajero)

-   Almacenar todas las transacciones para explotar esa informaci�n en un futuro mediante predicciones y analisis. 


Nuestra propuesta
-----------------

### �

![](https://github.com/ccanizares/NoBeerNoParty/raw/master/assets/nobeernoparty.png)

### Azure Search

Permitir� a la web buscar modelos de cerveza entre los stands y tener informaci�n precisa en todo momento de los modelos mejor valorados, cantidad aproximada restante de cada modelo en los stands, etc. 


### Web App

Conoceremos el servicio de aplicaciones escalables en la nube de Azure publicando la aplicaci�n web desde Visual Studio.
�

### Event Hub

Los cajeros enviar�n informaci�n sobre cada compra, este servicio permite la ingesta de informaci�n codificada como stream. 

[+ info](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send)
�

### Stream Analytics 

Estar� suscrito a los eventos de Event Hub y ser� el encagado de procesar y tranformar la informaci�n. Una parte la almacener� en documentDb y otra la dejar� en una cola. 

�
### Service Bus

Soporta colas de mensajes, recibir� mensajes por parte del servicio de Stream Analytics (compras) y tambi�n recibe mensajes directamente desde la aplicaci�n web (valoraciones)

�
### Azure Functions

Este servicio tiene como objetivo procesar los mensajes de service bus, y actualizar los datos del �ndice de b�squeda del servicio Azure Search. 

�
### DocumentDB 

Ser� donde almacenemos toda aquella informaci�n que quiere disponer la organizaci�n al finalizar el evento para su futuro analisis. 

�
Go!
---

### Crear recursos en azure

1.  Grupo de recursos: Agrupa todos los recursos en un grupo de manera que ser� m�s c�modo luego gestionarlos.

2.  Web App: Alojaremos aqu� la web que nos facilitan.

3.  Azure Search: De momento no configuramos ning�n indice, lo haremos m�s adelante v�a Rest Api.

4.  Document Db: Creamos una base de datos vac�a. 

5.  Event Hub: Creamos un topic UserRate y otro BeerShop. 

6.  Stream Analytics: Conectaremos como input el Event Hub creado en el paso anterior y como ouputs uno a documentDb y otro a AzureSearch. Crearemos las transformaciones de modelo necesarias. 

�
### Conectando piezas

1.  Descarga la aplicaci�n de Consola (simulaci�n de la aplicaci�n cajero) y a�ade los datos de conexi�n a Event Hub y Azure Search.

2.  Ejecuta la aplicaci�n y pulsa opci�n 1. Esto generar� los indices necesarios en Azure Search.

3.  Ejecuta la aplicaci�n ahora pulsando 2. Esto simular� el tr�fico de compras en los cajeros enviando informaci�n a nuestro sistema.   

4.  Descarga la aplicaci�n web y a�ade las cadenas de conexi�n necesarias para comunicarse con Azure Search y Event Hub. 

5.  Despliega la web (bot�n derecho y publicar).

�
### Explotando el sistema

1.  Accede a la web que hemos publicado como usuario y usa la funcionalidad de b�squeda.

2.  Como usuario valora una cerveza.

3.  Ahora accede como fabricante y consulta el estado de tus barriles.

4.  Como fabricante busca un hueco vacio y solicita colocar tu barril. 

5.  Con�ctate al portal de Azure y comprueba que los datos del evento se est�n almacenando en Document Db. 
