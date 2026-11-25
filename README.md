# Informe de Auditoría de Red Wi-Fi Insegura

## Análisis de tráfico HTTP y recomendaciones de seguridad

> **Actividad:** Seguridad en el aire: cómo sobrevivir a una Wi-Fi
> pública\
> **Herramienta de análisis:** Herramientas de desarrollador del
> navegador (pestaña Network)\
> **Sitio analizado:** [NeverSSL](http://neverssl.com)\
> **Archivo:** `README.md`

------------------------------------------------------------------------

## 1. Introducción

Las redes Wi-Fi públicas, como las de cafeterías, aeropuertos o espacios
compartidos, pueden facilitar el acceso a Internet, pero también
presentan riesgos para la privacidad y la seguridad de la información.
En una red de este tipo, otros usuarios conectados o un atacante que
consiga posicionarse en la comunicación podrían intentar observar o
manipular parte del tráfico.

El objetivo de este informe es analizar una solicitud realizada al sitio
NeverSSL, identificar qué información queda expuesta al utilizar HTTP y
explicar cómo una VPN puede ayudar a proteger la comunicación cuando se
utiliza una red pública.

## 2. Sitio analizado

Para realizar la práctica se accedió a **NeverSSL**, un sitio que
utiliza HTTP de forma intencional para permitir observar cómo funciona
una conexión sin el cifrado de HTTPS.

-   **Sitio:** <http://neverssl.com>
-   **URL registrada en la captura:**
    `http://goodslowgrandmorning.neverssl.com/online/`
-   **Protocolo:** HTTP
-   **Puerto remoto observado:** `80`

La URL registrada corresponde al host que apareció en la solicitud real
del navegador. Por eso, el análisis utiliza ese dato y no un dominio de
ejemplo.

## 3. Evidencia observada

Se abrió la pestaña **Network (Red)** de las herramientas de
desarrollador del navegador, se recargó la página y se inspeccionó la
solicitud correspondiente.

### 3.1 Datos de la solicitud

  Elemento         Valor observado
  ---------------- ----------------------------------------------------
  Request URL      `http://goodslowgrandmorning.neverssl.com/online/`
  Request Method   `GET`
  Status Code      `200 OK`
  Remote Address   `34.223.124.45:80`
  Host             `goodslowgrandmorning.neverssl.com`
  Protocolo        HTTP
  Puerto remoto    `80`

### 3.2 Headers enviados

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Header                              Valor observado
  ----------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------
  User-Agent                          `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/157.0.0.0 Safari/537.36 OPR/140.0.0.0 (Edition std-1)`

  Accept                              `text/html,application/xhtml+xml,application/xml;q=0.9,image/jxl,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7`

  Accept-Language                     `es-419,es;q=0.9,en;q=0.8,ar;q=0.7,th;q=0.6,ru;q=0.5`
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Interpretación:** el método `GET` indica que el navegador solicitó un
recurso al servidor. El estado `200 OK` informa que el servidor
respondió correctamente a esa solicitud. El campo `Host` identifica el
servidor solicitado, mientras que `User-Agent` aporta información sobre
el navegador y el sistema operativo. `Accept` indica los tipos de
contenido que el navegador puede recibir y `Accept-Language` expresa sus
preferencias de idioma.

Estos datos permiten reconocer información técnica de la conexión y
algunas características del cliente. En una conexión HTTP, la solicitud
y sus headers no están protegidos por cifrado durante el trayecto de
red.

## 4. Análisis técnico

### 4.1 ¿Qué protocolo utiliza el sitio?

El sitio utiliza **HTTP (Hypertext Transfer Protocol)**. Esto se
identifica en la URL, que comienza con `http://`, y en el puerto remoto
`80` que aparece en la captura.

A diferencia de HTTPS, HTTP no cifra por sí mismo la comunicación. Por
lo tanto, la información transmitida puede quedar expuesta a quien
consiga interceptar el tráfico en el camino entre el dispositivo y el
servidor.

### 4.2 ¿Qué información puede observarse durante la solicitud?

En la captura se pueden identificar varios elementos:

1.  **Host:** permite conocer el dominio al que se dirige la solicitud.
2.  **URL:** muestra el recurso específico solicitado.
3.  **Método GET:** indica que el navegador está solicitando información
    al servidor.
4.  **User-Agent:** revela datos del navegador y del sistema operativo.
5.  **Accept y Accept-Language:** muestran los formatos de contenido
    aceptados y las preferencias de idioma.
6.  **Dirección remota y puerto:** identifican la dirección IP y el
    puerto del servidor al que se realizó la conexión.

Al viajar mediante HTTP, estos elementos pueden ser observados por
alguien que tenga capacidad de capturar el tráfico de esa conexión.

### 4.3 ¿Qué riesgos existen al navegar mediante HTTP desde una red Wi-Fi pública?

El principal riesgo es que la comunicación HTTP no cuenta con cifrado de
transporte. Si un atacante logra interceptar el tráfico de la red,
podría leer la información que se transmite sin protección y, según el
contenido de la comunicación, modificarla.

Entre los riesgos se encuentran:

-   **Exposición de información:** las páginas solicitadas, los headers
    y otros datos transmitidos por HTTP pueden quedar visibles.
-   **Robo de datos:** si se envían credenciales, formularios o
    información personal mediante HTTP, podrían ser capturados.
-   **Manipulación del tráfico:** un atacante situado en el camino de la
    comunicación podría intentar alterar respuestas o redirigir al
    usuario.
-   **Seguimiento de actividad:** la información visible puede permitir
    inferir qué sitios se visitan y obtener datos sobre el dispositivo o
    el navegador.

La captura realizada demuestra que la solicitud observada utiliza HTTP y
que sus datos de petición pueden leerse en la herramienta de desarrollo.
No demuestra por sí sola que haya ocurrido un ataque ni que un tercero
haya interceptado la conexión.

### 4.4 ¿Cómo cambiaría este escenario utilizando una VPN?

Una **VPN (Virtual Private Network o red privada virtual)** crea un
túnel cifrado entre el dispositivo y un servidor VPN. El tráfico que
circula por ese túnel se protege mediante cifrado y encapsulamiento.

  -----------------------------------------------------------------------
  Concepto                            Función
  ----------------------------------- -----------------------------------
  Cifrado                             Convierte los datos en información
                                      ilegible para quien no posee las
                                      claves necesarias.

  Encapsulamiento                     Envuelve los paquetes originales
                                      dentro de otros paquetes para
                                      transportarlos a través del túnel
                                      VPN.

  Túnel seguro                        Establece una conexión protegida
                                      entre el dispositivo y el servidor
                                      VPN.

  Protección del tráfico              Dificulta que otros usuarios de la
                                      Wi-Fi pública puedan leer el
                                      contenido transportado dentro del
                                      túnel.

  Privacidad                          Reduce la exposición del tráfico
                                      frente a observadores de la red
                                      local; el proveedor de VPN pasa a
                                      ser un intermediario de confianza.
  -----------------------------------------------------------------------

Con una VPN activa, una persona que capture el tráfico en la Wi-Fi
pública normalmente verá una conexión cifrada entre el dispositivo y el
servidor VPN, en lugar del contenido interno de esa comunicación.

**Importante:** una VPN no convierte un sitio HTTP en HTTPS ni garantiza
seguridad total. El tramo entre el servidor VPN y el sitio web sigue
dependiendo de la protección que utilice el sitio. Por eso, es
recomendable combinar una VPN confiable con HTTPS, mantener los
dispositivos actualizados y evitar enviar información sensible en
páginas que no utilicen HTTPS.

## 5. Las 3 Reglas de Oro para usar redes Wi-Fi públicas

### Regla 1 --- Verificar que la conexión sea HTTPS

Antes de ingresar contraseñas, datos bancarios o información personal,
comprobar que el sitio utilice HTTPS y que el navegador no muestre
advertencias de seguridad. Evitar enviar información sensible en páginas
HTTP.

### Regla 2 --- Utilizar una VPN confiable

Cuando sea necesario conectarse a una Wi-Fi pública, utilizar una VPN de
confianza para cifrar el tráfico entre el dispositivo y el servidor VPN.
Una VPN ayuda a proteger la comunicación dentro de la red local, pero no
reemplaza HTTPS ni elimina todos los riesgos.

### Regla 3 --- Evitar redes desconocidas y compartir lo mínimo

Confirmar el nombre de la red con el establecimiento antes de
conectarse, desconfiar de redes con nombres sospechosamente parecidos y
desactivar la conexión automática a redes abiertas. También conviene
evitar operaciones sensibles en redes públicas y desactivar el uso
compartido de archivos cuando no sea necesario.

## 6. Conclusión

La práctica permitió comprobar que la solicitud analizada a NeverSSL
utiliza HTTP y que es posible observar datos como la URL, el host, el
método GET, el User-Agent y otros headers. Esta información muestra por
qué una conexión sin cifrado puede representar un riesgo, especialmente
cuando se utiliza una red Wi-Fi pública.

Una VPN aporta una capa adicional de protección mediante cifrado,
encapsulamiento y un túnel seguro entre el dispositivo y el servidor
VPN. Sin embargo, la mejor práctica es combinar esta protección con el
uso de HTTPS, redes confiables y hábitos responsables de navegación.

------------------------------------------------------------------------

## 7. Resumen de los hallazgos

  -------------------------------------------------------------------------
  Punto de auditoría                  Resultado
  ----------------------------------- -------------------------------------
  Protocolo utilizado                 HTTP

  Método de solicitud                 GET

  Respuesta del servidor              200 OK

  Host identificado                   `goodslowgrandmorning.neverssl.com`

  Puerto remoto                       80

  Información visible                 URL, host, método y headers
                                      observados

  Riesgo principal                    Exposición y posible manipulación del
                                      tráfico sin cifrado

  Medida de protección                VPN confiable, HTTPS y buenas
                                      prácticas de navegación
  -------------------------------------------------------------------------

------------------------------------------------------------------------

**Fin del informe.**

> **Entrega:** guardar este archivo con el nombre `README.md` en el repositorio público `practica-wifi-segura`. La evidencia del análisis está documentada mediante los datos reales transcritos en las tablas de este informe; no es necesario adjuntar imágenes.

