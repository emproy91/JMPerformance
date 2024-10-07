# Guía para Apache JMeter (5.6.3)
 ## Introducción ##

Este informe presenta los resultados de una serie de pruebas de rendimiento realizadas en la aplicación mediante el uso de Apache JMeter . El objetivo principal fue medir el comportamiento de la aplicación bajo diferentes cargas, así como el consumo de recursos del sistema. Se realizaron dos pruebas específicas con un número creciente de usuarios concurrentes y una prueba adicional que combina los dos casos anteriores para obtener un análisis más detallado del consumo de memoria y procesador durante el período de prueba.

Las pruebas fueron ejecutadas desde la siguiente ruta en el sistema local:

F:\ING\JMeter\PRUEBA\apache-jmeter-5.6.3\apache-jmeter-5.6.3\bin


## Objetivos de las pruebas
- Evaluar el rendimiento de la aplicación en tres escenarios diferentes:
    - Inicio de sesión.
    - Operaciones dentro del formulario.
    - Cierre de sesión.

- Diferentes cargas de usuarios similares :
    - Prueba 1: 10 usuarios simultáneos por 180 segundos (3 minutos).
    - Prueba 2: 50 usuarios simultáneos por 600 segundos (10 minutos).

- Generar informes detallados para cada uno de los escenarios, incluyendo tiempos de respuesta y análisis del consumo de recursos del sistema:
    - CPU 
    - Memoria

- Centralizar los informes utilizando el PerFoms Plugin Collector para analizar de manera integrada el consumo de recursos durante ambas pruebas.

- Estructura de los Scripts y Reportes Generados

## El proyecto de JMeter está organizado de la siguiente manera:
![Script 1.jmx - HTTP Request Login](Adds\3_2.png)

- Script 1 y Script 2 se ejecutan por separado para simular la carga de 10 y 50 usuarios respectivamente.
- Script 3 combina ambos scripts para analizar el consumo de recursos mediante el PerFoms Plugin Collector 

# Paso a Paso de la Ejecución de las Pruebas
 ## 1. Crear un Test Plan

![Apache JMeter - Test Plan](Adds\1_1.png)

- **Usuarios**
  - **Thread Properties**
    - Number of Threads (users): 10
    - Ramp-up period (seconds): 180

### 1.1. Configuración de los Usuarios (Thread Group)
Abrir Apache JMeter:

Inicia la aplicación Apache JMeter.
Crear un Nuevo Test Plan:

Haz clic en "File" > "New" para crear un nuevo test plan.

Agregar un Thread Group:

Haz clic derecho en el Test Plan y selecciona Add > Threads (Users) > Thread Group.
Configurar las Propiedades del Thread Group:

Number of Threads (usuarios):

 - Valor: 10

Descripción: Este valor indica la cantidad de usuarios virtuales que simularás en la prueba. En este caso, 10 usuarios estarán ejecutando las mismas solicitudes simultáneamente.

Ramp-up Period (seconds):

 - Valor: 180

Descripción: Este es el tiempo que JMeter tardará en iniciar todos los hilos (usuarios). En este caso, con un valor de 180 segundos, JMeter tomará 180 segundos para iniciar los 10 usuarios. Esto significa que se iniciará un nuevo usuario cada 18 segundos (180 segundos ÷ 10 usuarios). Este enfoque permite simular un aumento gradual en la carga del servidor, evitando picos repentinos que podrían dar lugar a resultados engañosos.


## 2. Configurar HTTP Request para Login

![Script 1.jmx - HTTP Request Login](Adds\1_2.png)

Agregar HTTP Request:

Haz clic derecho en el Thread Group y selecciona
 - Add > Sampler > HTTP Request.
Configurar Web Server:

- Server Name or IP: demo.serenity.is

Configurar HTTP Request:

- Method: GET (o POST si se usa ese método para el login).

- Path: /Account/Login/?ReturnUrl=%2F
Agregar Parámetros:

En la pestaña Parameters:
- Name: demouser
- Value: admin
- Name: password
- Value: serenity


## 3. Configurar HTTP Request para Formulario

![Script 1.jmx - HTTP Request Formulario](Adds\1_3.png)

- Agregar HTTP Request:

- Haz clic derecho en el Thread Group y selecciona 

    -  Add > Sampler > HTTP Request.
    Configurar Web Server:

    - Protocol: http
    - Server Name or IP: demo.serenity.is
    - Configurar HTTP Request:
    - Method: POST
    - Path: Northwind/Supplier#edit/18

- Agregar Datos del Formulario:

En la pestaña Body Data (o Parameters, según la configuración del formulario):

- Nombre de la empresa: [valor a ingresar]
- Nombre de contacto: [valor a ingresar]
- Título de contacto: [valor a ingresar]
- Dirección: [valor a ingresar]
- Región: [valor a ingresar]
- Código postal: [valor a ingresar]
- País: [valor a ingresar]
- Ciudad: [valor a ingresar]
- Teléfono: [valor a ingresar]
- Fax: [valor a ingresar]

---

## 4. Configurar HTTP Request para Cerrar Sesión


![Script 1.jmx - HTTP Request Cerrar Sesión](Adds\1_4.png)

- Agregar HTTP Request:

Haz clic derecho en el Thread Group y selecciona 
- Add > Sampler > HTTP Request.
Configurar HTTP Request:

- HTTP Request Name: Cerrar sesión

Configurar Web Server:

- Protocol: http
- Server Name or IP: demo.serenity.is

Configurar HTTP Request:

- Method: GET
- Path: Account/Signout



## 5. Ver Resultados
![Script 1.jmx - HTTP Request Cerrar Sesión](Adds\1_5.png)
Agregar Listener para Ver Resultados:

Haz clic derecho en el Thread Group y selecciona 
- Add > Listener > View Results Tree.

Resultados Esperados:

- Iniciar sesión: Verifica que la solicitud de inicio de sesión se complete correctamente.
- Formulario: Asegúrate de que los datos del formulario se envíen correctamente y se reciban respuestas adecuadas.
- Cerrar sesión: Confirma que la solicitud de cierre de sesión se ejecute sin errores.

Observa los resultados en el View Results Tree, donde podrás ver detalles de cada solicitud, incluyendo la respuesta del servidor.

## 6. Gráfico Agregado

![Test Plan - Aggregate Graph](Adds\1_6.png)

Agregar Listener para Gráfico Agregado:

Haz clic derecho en el Thread Group y selecciona 
- Add > Listener > Aggregate Report o Aggregate Graph.

Resultados del Gráfico Agregado:

- Tiempo Total: Valor: 00:02:46

- Errores: Valor: 0
- Éxitos:Valor: 10


Visualización de Resultados
