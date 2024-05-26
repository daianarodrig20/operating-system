 TP4: TLS/SSL, PKI, PGP-GPG

### EJERCICIO 1 | TLS/SSL

#### A. ¿Qué es TLS/SSL y para qué se utiliza? ¿Hay diferencia entre TLS y SSL?
SSL es el predecesor del protocolo TLS. Son protocolos criptográficos que proporcionan privacidad e integridad en la comunicación entre dos puntos en una red de comunicación. Se utiliza para garantizar que la información transmitida por dicha red no pueda ser interceptada ni modificada por elementos no autorizados, garantizando de esta forma que sólo los emisores y los receptores legítimos sean los que que tengan acceso a la comunicación de manera íntegra.

No habría muchas diferencias ya que TLS es sólo una versión más reciente de SSL, la función de las nuevas versiones de TLS son corregir algunas vulnerabilidades de seguridad en los protocolos SSL anteriores.

#### B. ¿Qué tipos de certificados utiliza y de qué modo? ¿Qué relación tiene con la criptografía asimétrica?
Tipos de certificados SSL:
- Certificados con validación de dominio
- Certificados de validación de organización
- Certificado de validación extendida

La relación que tiene con la criptografía asimétrica es que el TLS/SSL está basado en este tipo de criptografía.

#### C. Ejemplos de sitios web con certificados emitidos por distintas CA
- Facebook
- Youtube
- Spotify
- Google
- Twitter

### EJERCICIO 2 | PKI

#### A. ¿Qué es la Infraestructura de Clave Pública (PKI)?
Una PKI (Public Key Infraestructure) o Infraestructura de Clave Pública, es un conjunto de componentes y servicios que facilitan y permiten gestionar y administrar la generación, expedición, revocación y validación de certificados digitales.

#### B. ¿Cómo puedo obtener un certificado para un dominio?
Para obtener un certificado:
1. El dominio debe estar bien configurado en el DNS y propagado.
2. Comprobación de posesión del dominio.
3. Verificación de autenticidad del titular del dominio.

#### C. Explique cómo se logra una conexión exitosa por HTTPS. ¿Cómo se relaciona con PKI y cómo se valida el certificado?
- El browser inicia una conexión https:// y le pide al server que se identifique.
- El server envía una copia de su certificado SSL, que incluye su clave pública RSA.
- El browser verifica el certificado (CA de confianza, no expirado, no revocado, nombre coincide con DNS, etc.).
- Si pasan todas las validaciones, el browser crea una clave simétrica, la encripta con la clave pública del servidor, y se la manda.
- El server desencripta la clave de sesión usando su clave privada y le manda al browser un ”ACK” encriptado con la clave de sesión.
- A partir de ahí, todo el tráfico es encriptado.

#### D.
* www.google.com
* *.unnoba.edu.ar
* *.whatsapp.net

### EJERCICIO 3 | PGP-GPG

#### A. ¿Qué es PGP y qué relación tiene con GPG?
PGP (Pretty Good Privacy) es un programa creado por Phil Zimmermann que nos ayuda a proteger nuestra privacidad. GPG (GNU Privacy Guard) es un derivado libre de PGP y su utilidad es la de cifrar y firmar digitalmente, siendo además multiplataforma.

#### B. Diferencias y similitudes entre PGP y SSL/TLS
Diferencias entre PGP y SSL/TLS:
- SSL/TLS asegura la confidencialidad de los datos transmitidos por la red, mientras que PGP y GPG proporcionan una implementación de los algoritmos criptográficos para proteger datos almacenados.
- PGP tenía un esquema descentralizado de red de confianza, en contraste con el sistema centralizado PKI de SSL/TLS.

Similitudes entre PGP y SSL/TLS:
- Ambos usan criptografía de clave pública para autenticación de documentos.

### Cifrado simétrico con GPG
Utilizar la herramienta GPG para encriptar un mensaje con clave simétrica:

```shell
echo "hola, que tal ?" > texto.txt
```
```shell
gpg -c texto.txt
```

```shell
cat texto.txt.gpg
```

¿Cómo desencripto el archivo? ¿Qué formato de encriptación utiliza por defecto?
Para desencriptar el archivo se escribe el siguiente comando:
```shell
gpg text.txt.gpg
```

El formato de encriptación es de cifrado híbrido que usa una combinación convencional de
criptografía de claves simétricas para la rapidez y criptografía de claves públicas para el fácil
compartimiento de claves seguras, típicamente usando recipientes de claves públicas para cifrar
una clave de sesión que es usada una vez. Este modo de operación es parte del estándar
OpenPGP y ha sido parte del PGP desde su primera versión.

### Cifrado asimétrico con GPG
#### ¿Qué es un anillo de confianza? ¿Cuál es su utilidad práctica? ¿Qué dificultades conlleva su construcción? ¿Es obligatoria su construcción?

En criptografía, una red de confianza es un concepto usado en PGP, GnuPG, y otros sistemas
compatibles con OpenPGP para certificar la autenticidad de que una clave pública pertenece a su
dueño.
Se basa en que los participantes en la red firman entre sí sus claves públicas con sus claves privadas,
certificando de esta manera que la clave pública pertenece a la persona física a la que se atribuye. La
persona que firma debe establecer esta correspondencia sobre la base de un conocimiento externo,
como por ejemplo haberse encontrado físicamente con la persona de la que certifica la clave en el
mundo real. El interés del concepto está en que todos los demás conocidos del firmante, aunque se
encuentren en un lugar remoto, tienen ahora un vínculo que les asegura que pueden confiar en los
mensajes (por ejemplo, código de software) firmados con la clave privada de la persona certificada.
La validación de la clave es más difícil. Si no conocemos personalmente a la persona cuya clave
queremos firmar, no es posible que podamos firmar su clave personalmente. Debemos fiarnos de las
firmas de otras personas y esperar que encontraremos una cadena de firmas que nos lleven desde la
clave en cuestión hasta la nuestra. Para poder encontrar una cadena, debemos tomar la iniciativa y
conseguir que nuestra clave sea firmada por otras personas fuera de nuestro anillo de confianza inicial.
Un modo efectivo de conseguir esto es participando en “reuniones de firmas”.
De todos modos, debemos tener en cuenta que todo esto es opcional. No existe ningún tipo de
obligación de hacer públicas nuestras claves o de firmar las claves de otros. GnuPG es lo
suficientemente flexible como para adaptarse a nuestros requerimientos de seguridad, sean éstos los
que sean. Sin embargo, la realidad social es que necesitaremos tomar la iniciativa si queremos
engrosar nuestro anillo de confianza, y hacer todo el uso posible de GnuPG.


### EJERCICIO 4 | Mailvelope - Enigmail

#### A. Mailvelope
Es una extensión para los navegadores que incorpora el estándar OpenPGP para el cifrado y descifrado de texto en los correos electrónicos.

#### B. Enigmail
Es una extensión adicional para Mozilla y Mozilla Thunderbird para cifrar y firmar digitalmente correos electrónicos.

Es para las versiones que funcionan bajo los sistemas Microsoft Windows y de tipo Unix, como GNU/Linux. Enigmail no es
un motor criptográfico por sí mismo, sino que utiliza el GNU Privacy Guard (GnuPG o GPG) para
realizar las operaciones de cifrado criptográfico, facilitando el trabajo ya que no es necesario cifrar
o firmar ficheros de texto desde la línea de comandos y luego pegarlos manualmente al mensaje
de correo electrónico.
