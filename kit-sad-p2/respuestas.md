# Respuestas · Práctica 2 · Bodegas Atlántico

> Rellena este fichero en tu ordenador. Va en la entrega (`./lab.sh entregar`) y en la defensa te pueden preguntar por él.

## Bloque A · Piensa como un atacante (nmap)

Desde la calle: `./lab.sh cliente` y dentro `nmap -Pn -p 22,80,443,5432,8080 198.51.100.254`. Pega la salida:

```text
root@cliente:/# nmap -Pn -p 22,80,443,5432,8080 198.51.100.254
Starting Nmap 7.95 ( https://nmap.org ) at 2026-11-24 20:20 UTC
Nmap scan report for sad-p2-fw.sad-p2_calle (198.51.100.254)
Host is up (0.000070s latency).

PORT     STATE    SERVICE
22/tcp   filtered ssh
80/tcp   open     http
443/tcp  open     https
5432/tcp filtered postgresql
8080/tcp filtered http-proxy
MAC Address: 5A:FA:AF:7F:D4:E5 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.33 seconds

```

## Bloque A · El contador

Después de `./lab.sh comprobar`, mira `./lab.sh ver`. ¿Cuántos paquetes lleva el contador de A6? ¿Quién los ha mandado?

> iifname "dmz" oifname "lan" counter packets 12 bytes 720 drop comment "A6 Todo lo que entre de DMZ y vaya a LAN, se dropea" # handle 18
Lleva 12 paquetes, enviados por 

## Bloque B · La prueba del cifrado

En el firewall (`./lab.sh fw`): `tcpdump -ni wan host 198.51.100.62 -c 6`. Mientras, en otra terminal, el comercial
(`./lab.sh comercial`) hace `curl http://172.16.10.20:8080`. Pega lo que sale en el firewall:

```text
root@fw:/# tcpdump -ni wan host 198.51.100.62 -c 6
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on wan, link-type EN10MB (Ethernet), snapshot length 262144 bytes
13:31:54.785912 IP 198.51.100.62.46321 > 198.51.100.254.51820: UDP, length 96
13:31:54.786021 IP 198.51.100.254.51820 > 198.51.100.62.46321: UDP, length 96
13:31:54.786088 IP 198.51.100.62.46321 > 198.51.100.254.51820: UDP, length 96
13:31:54.786228 IP 198.51.100.62.46321 > 198.51.100.254.51820: UDP, length 176
13:31:54.786298 IP 198.51.100.254.51820 > 198.51.100.62.46321: UDP, length 96
13:31:54.786377 IP 198.51.100.254.51820 > 198.51.100.62.46321: UDP, length 304
6 packets captured
10 packets received by filter
0 packets dropped by kernel

```

En dos líneas: ¿qué se ve por la calle y qué NO se ve?

> Se ven los paquetes cifrados pero no se ve el contenido de ellos, puertos ni IP's.

## Bloque C · Para la defensa (C4)

Del **mismo ataque** (la inyección a la web): ¿qué ve el vigilante de la red (el IDS) y qué ve el portero de la web? ¿Por
qué hacen falta los dos?

>

## Bloque D · La nota del servidor (D0 y D4)

Nota del análisis **antes** de bastionar (D0):

>

Nota **después** (D4). ¿Por qué no llega a 100? ¿Significa que el servidor sigue inseguro? Comenta un aviso que siga saliendo.

>
