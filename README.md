# Incompatibilidad quìmica

**Descripciòn**

Este proyecto de automatización está enfocado en la evaluación de la incompatibilidad de reactivos químicos durante su almacenamiento. Surge como respuesta a una necesidad común en los laboratorios de análisis, donde garantizar la seguridad es una prioridad constante.

A través de un flujo automatizado, se busca fortalecer la gestión de reactivos químicos, permitiendo identificar de forma temprana posibles incompatibilidades peligrosas entre sustancias. Esta solución contribuye a construir un sistema robusto de manejo de reactivos, que minimiza la intervención manual y reduce el riesgo de errores humanos, los cuales podrían derivar en accidentes o incidentes de seguridad.

En resumen, el proyecto ofrece una herramienta práctica y escalable que puede integrarse en entornos de laboratorio para apoyar una gestión segura, eficiente y automatizada de los reactivos.

**Tecnologias Usadas**

n8n, PubChem API, google Sheets, Telegram, gmail.

**Funcionamiento**

En la siguiente imagen se muestra e flujo de n8n usado

![Flujo de n8n](https://github.com/Robchem95/N8N-Incompatibilidad-Quimica/blob/main/Imagenes%20n8n%20incompatibilidad%20qu%C3%ACmica/Captura%20de%20pantalla%202025-07-10%20105304.png)

1. Ejecuciòn de bot de telegram, datos estructurales

Se inicia el flujo con un bot de telegram generado por el Bot father, en este se ingresa un comando que se estructura de la manera /cargar (fòrmula del reactivo)(compartimento donde se quiere almacenar), ver imagen. Enseguida se estructuran los datos en formato json.

<img src="https://raw.githubusercontent.com/Robchem95/N8N-Incompatibilidad-Quimica/main/Imagenes%20n8n%20incompatibilidad%20qu%C3%ACmica/Chat%20bot%20de%20telegram.jpg" alt="Telegram Bot" width="300"/>

2. Integraciòn de informaciòn para comparar

Se agrega el nodo "Inventario almacenado" este filtra los reactivos ya almacenados en base al compartimento del reactivo que se quiere cargar, y con el nodo de "selecciòn de reactivos" se estructura esta informaciòn en formato json.

<img src="https://github.com/Robchem95/N8N-Incompatibilidad-Quimica/blob/main/Imagenes%20n8n%20incompatibilidad%20qu%C3%ACmica/Inventario%20Google%20Sheet.png
" width="300"/>

3. Consulta API PUBCHEM

Los nodos de consulta HTTP, toman la informaciòn de la API de PUBCHEM una pagina que tiene la descripciòn de reactivos quìmicos, mas exactamente toma la informaciòn de los requisitos de almacenamiento del reactivo a cargar. 

4. Comparaciòn y criterio de compatibilidad

El nodo de "integracion de informes" organiza los datos necesarios para llevar acabo la comparaciòn necesaria en el nodo de "Busqueda de coincidencias" este busca semejanzas en la descripciòn del reactivo a almacenar, con el nombre de los reactivos almacenados, si encuentra al menos un nombre en la descripciòn decide que es incompatible, por el contrario si determina que no hay ni una coincidencia es compatible. 

5. Decisiòn

De acuerdo a la informaciòn arrojada en las coincidencias el nodo "condiciòn de compatibilidad" determina el camino para las notificaciones. 
Si es incompatible notifica tanto con un mensaje en telegram y con una imagen enviada a un mail. 

<img src="https://github.com/Robchem95/N8N-Incompatibilidad-Quimica/blob/main/Imagenes%20n8n%20incompatibilidad%20qu%C3%ACmica/8b09776f-3cc6-4c75-b37c-23c6148cf9dc.jpg" alt="Telegram Bot" width="300"/>

<img src="https://github.com/Robchem95/N8N-Incompatibilidad-Quimica/blob/main/Imagenes%20n8n%20incompatibilidad%20qu%C3%ACmica/8b09776f-3cc6-4c75-b37c-23c6148cf9dc.jpg" width="300"/>

Si es compatible notifica con una respuesta en telegram, y procede a registrar el nuevo reactivo en la planilla de inventario.

<img src="https://github.com/Robchem95/N8N-Incompatibilidad-Quimica/blob/main/Imagenes%20n8n%20incompatibilidad%20qu%C3%ACmica/Respuesta%20de%20Telegram.jpg" width="300"/>

<img src="https://github.com/Robchem95/N8N-Incompatibilidad-Quimica/blob/main/Imagenes%20n8n%20incompatibilidad%20qu%C3%ACmica/Almacenamiento%20nuevo%20reactivo.png" width="300"/>
