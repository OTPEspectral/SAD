# Práctica 1 de SAD · SecureCorp — Respuestas

**Nombre y apellidos: Luis Miguel Toscano**
**Usuario: otp**

Responde con tus palabras, en 1-3 líneas. En la defensa te preguntaré lo mismo en voz alta.

**Contraseñas que has usado** (solo porque es un laboratorio; en una empresa, jamás en un fichero):

- Tu usuario: Luis 2026
- mtorres: Maria 2026

---

**1. (A1)** ¿Quién es el `issuer` de tu `ca.crt`? ¿Hasta qué fecha es válido? ¿Por qué el `subject`
y el `issuer` de la CA son iguales y los de `ldap.crt` no?
El ISSUER es C = ES, O = SecureCorp, CN = SecureCorp Root CA - ltoscano. Es válido hasta el 8 de octubre de 2036.
Porque el servidor de certificación usa su propia firma, ya que no hay nadie con mas rango que la pueda firmar por él. 
Sin embargo, el servidor si puede ser firmado por el servidor de certificación.

**2. (A3)** Pega el comando y el resultado de tus dos búsquedas:

```
a) miembros de rrhh: ldapsearch -x -LLL -H ldap://ldap.securecorp.local \
  -D "cn=admin,dc=securecorp,dc=local" -W \
  -b "dc=securecorp,dc=local" "(cn=rrhh)" \
  dn cn member uniqueMember memberUid

Los unicos miembros son lromero y mtorres.

b) cn y mail de todas las personas: ldapsearch -x -LLL -H ldap://ldap.securecorp.local \
  -D "cn=admin,dc=securecorp,dc=local" -W \
  -b "ou=people,dc=securecorp,dc=local" \
  "(objectClass=inetOrgPerson)" cn mail

cn: Lucia Romero
mail: lromero@securecorp.local

cn: Luismiguel Toscano
mail: ltoscano@securecorp.local

cn: Marta Torres
mail: mtorres@securecorp.local

```

**3. (A4)** ¿Por qué la clave `ldap.key` tiene que ser de `openldap` y tener permisos 600?
Tiene que ser de openldap para que ningún usuario pueda cambiarla o modificarla. Los permisos se hacen 
justamente para eso, para darle los permisos a él mismo y restringirlo de usuarios o grupos ajenos.

**4. (A4)** ¿Qué valor has puesto en `SLAPD_SERVICES` y por qué?
ldaps:/// y ldapi:///. Porque así configuramos el puerto 636 (ldaps) seguro como método de TLS/SSL. ldapi:/// se 
ya que permite la configuración del ldap a través de EXTERNAL.

**5. (A4)** Antes de añadir `TLS_CACERT` en el cliente, `ldaps://` no funcionaba. ¿Por qué?
Porque no estaba configurado el certificado seguro. Imagino que al no estar ahi no confiaba en quién habia
firmado el certificado.

**6. (B3)** Pega la salida de `klist` con tus dos tickets. ¿Para qué sirve cada uno? ¿Ha viajado tu
contraseña por la red?

root@CLIENTE:~# kinit ltoscano
Password for ltoscano@SECURECORP.LOCAL: 
root@CLIENTE:~# kvno host/web.securecorp.local
host/web.securecorp.local@SECURECORP.LOCAL: kvno = 1

klist:
root@CLIENTE:~# klist
Ticket cache: FILE:/tmp/krb5cc_0
Default principal: ltoscano@SECURECORP.LOCAL

Valid starting     Expires            Service principal
10/13/26 18:59:52  10/14/26 04:59:52  krbtgt/SECURECORP.LOCAL@SECURECORP.LOCAL --> Sirve para autenticarte a ti mismo.
	renew until 10/20/26 18:59:52						   A traves de este ticket puedes 
										   solicitar otros servicios en la red
										   sin tener que volver a validarte.

10/13/26 19:00:15  10/14/26 04:59:52  host/web.securecorp.local@SECURECORP.LOCAL --> Sirve para acceder a este servicio
	renew until 10/20/26 18:58:52						     En vez de enviar la contraseña,
										     usas el ticket de Kerberos para
										     decir que eres un usuario válido.



**7. (C)** En el `docker-compose.yml`, ¿qué diferencia hay entre `build:` e `image:`? ¿Qué
significa la línea `- "8081:80"` del servicio `phpldapadmin`?
Build se usa para imagenes personalizadas, mientras que image se usa para imagenes ya creadas.
Significa que estamos redirigiendo el puerto 80 de phpmyadmin al 8081 de nuestra máquina, para habilitar
la comunicación entre ambas.

**8. (C)** ¿Por qué en la máquina `web` no has tenido que escribir a mano `TLS_CACERT`, y en el
cliente sí? ¿Qué pasaría con esa línea del cliente si hicieras `./lab.sh reset`?
Porque desde el Dockerfile hemos pasado el archivo ya configurado del ldap directamente a la imagen que hemos usado
en Dockerfile. Como no tenemos configurado ningún almacenamiento en el docker-compose, al resetear las máquina se 
borrarían las configuraciones que hemos hecho.
