# Taller 1 - Packet Tracer y Subneting (Subneteo de redes)

## Sección 1

1. ¿Cuántas interfaces Fast Ethernet tiene el Switch?
2. ¿Cuántas interfaces Giga Ethernet tiene el Switch?
3. ¿Cuál es el o son los rangos de valores que se muestran en las líneas
   vty?
   4.¿Para qué sirve el comando show startup-config?,y ¿Por qué el Switch
   emite esa respuesta?
4. ¿Para qué sirve el comando show running-config?,y ¿Por qué el
   Switch emite esa respuesta?
   6.¿Puede explicar cuál es la diferencia en utilizar el comando sh startup-
   config y sh running-config?
   7.¿Qué comando utilizaría para grabar la configuración del switch y no
   pedir confirmación si desea grabar los cambios?**

Mediante el uso del comando show versión, responda
las siguientes preguntas

1. ¿Cuál es la versión de IOS de cisco que está ejecutando el Switch?
2. ¿Cuál es el nombre del archivo de imagen del sistema?
3. ¿Cuál es la dirección MAC base del Switch?

## Sección 2

1. Realice la siguiente Topología en Packet Tracer
   La topología puede ser vista en el archivo `taller1.pkt`
2. Ingrese al PC de José al Router usando cable de consola o Rocío mediante
   cable auxiliar y defina claves de acceso para cada una de las conexiones de
   inicio y modo privilegiado (enable).
3. Defina claves de acceso para login remoto (vty), queda a libre elección (Rocío
   o José).
4. Debe crear los usuarios para acceder al router de forma remota (telnet y ssh).
5. Defina mensajes de bienvenida (BANNERS) motd, login.
6. Defina y configure una dirección IP para la puerta Gigabitethernet0/1 del
   router.
7. Grabe sus configuraciones.
8. Hacer pruebas de conexión por el puerto de red del PC de Matías y Andrea
   mediante telnet y Carlos por ssh.
9. ¿Qué es TFTP y en qué se diferencia de FTP?
10. Configurar una interfaz de Red del PC. Ejemplo: ip:192.168.50.2/24) para el
    servidor TFTP.
11. Mediante el programa TFTP en el PC, verifique la configuración obtenida
    del dispositivo cisco.

## Sección 3

### Identificar la clase de red

- Clase A (/8): 1.0.0.0 -> 126.255.255.255
- Clase B (/16): 128.0.0.0 -> 191.255.255.255
- Clase C (/24): 192.0.0.0 -> 223.255.255.255

### Identificar subred de mayor tamaño

- Administración (500 IP’s)
- Recursos humanos (1000 IP’s)
- Producción (1000 IP’s)
- Finanzas (1500 IP’s)
- Marketing (2000 IP’s)
- Ventas (3500 IP’s)
- Control de Gestión (5000 IP’s),

**Total: 7 subredes**

#### Las sucursal

- Zona norte fue asignada con la red 132.50.0.0 -> Clase B
- Zona Sur quedó con la 191.25.0.0 - Clase B

### Calcular nueva máscara de subred

Debemos encontrar una nueva máscara que cumpla 2 condiciones:

1. El divisor debe ser mayor que la cantidad de subredes solicitadas.
2. El cociente debe ser mayor que la subred de mayor tamaño solicitada.

#### Total

11111111.11111111.*00000000.00000000* = 2^16 - 2 = (65.534)

11111111.11111111.000**00000.00000000** = 2^13 - 2 = 8190 >= 5000 IP's

#### Los bits restantes

Los bits restantes indican la cantidad de subredes:
11111111.11111111.[*000*]**00000.00000000** = 2^3 = 8 >= 7 subredes

Entonces, la nueva máscara de subred dividirá la red en *8 partes*, cada
una con **8190 hosts** disponibles.

El antiguo prefijo:

11111111.11111111.00000000.00000000 = /16 y Mascara = 255.255.0.0

El nuevo prefijo:

11111111.11111111.11100000.00000000 = /16 + /3 = /19

| 2^8 = 256 | 2^7 = 128 | 2^6 = 64 | 2^5 = 32 | 2^4 = 16 | 2^3 = 8 | 2^2 = 4 | 2^1 =2 | 2^0 = 1 |
| --------- | --------- | -------- | -------- | -------- | ------- | ------- | ------ | ------- |
|           | 1         | 1        | 1        | 0        | 0       | 0       | 0      | 0       |

128+64+32 = 224

Mascara = 255.255.224.0

### Calcular salto

El salto es la cantidad de direcciones que se deben sumar para llegar a la siguiente subred.

256 - 224 = 32 o 11111111.11111111.111[*00000*].00000000 -> 2^5 = 32

### Tablas

### Zona Norte

Usted como experto en redes deberá aplicar direccionamiento Classful a la sucursal norte y obtener los siguientes datos de cada subred, ¿Cuántas IP’s disponibles hay para cada subred?

| N°subred               | Dirección subred | 1era IP utilizable | Última IP utilizable | Máscara de red | Prefijo | Gateway      | Broadcast      |
| ----------------------- | ----------------- | ------------------ | --------------------- | --------------- | ------- | ------------ | -------------- |
| 1 - Administración     | 132.50.0.0        | 132.50.0.1         | 132.50.31.254         | 255.255.224.0   | /19     | 132.50.31.1  | 132.50.31.255  |
| 2 - Recursos humanos    | 132.50.32.0       | 132.50.32.1        | 132.50.63.254         | 255.255.224.0   | /19     | 132.50.32.1  | 132.50.63.255  |
| 3 - Producción         | 132.50.64.0       | 132.50.64.1        | 132.50.95.254         | 255.255.224.0   | /19     | 132.50.64.1  | 132.50.95.255  |
| 4 - Finanzas            | 132.50.96.0       | 132.50.96.1        | 132.50.127.254        | 255.255.224.0   | /19     | 132.50.96.1  | 132.50.127.255 |
| 5 - Marketing           | 132.50.128.0      | 132.50.128.1       | 132.50.159.254        | 255.255.224.0   | /19     | 132.50.128.1 | 132.50.159.255 |
| 6 - Ventas              | 132.50.160.0      | 132.50.160.1       | 132.50.191.254        | 255.255.224.0   | /19     | 132.50.160.1 | 132.50.191.255 |
| 7 - Control de Gestión | 132.50.192.0      | 132.50.192.1       | 132.50.223.254        | 255.255.224.0   | /19     | 132.50.192.1 | 132.50.223.254 |

### Zona Sur

En base a los cálculos realizados, ¿Cuántas IP’s disponibles hay para cada subred?

Para la sucursal Sur deberá utilizar direccionamiento classful y obtener lo siguiente: 

| N°subred               | N°host solic. | N° host encont. | Dirección subred | 1era IP utilizable | Última IP utilizable | Máscara de red | Prefijo | Gateway      | Broadcast      |
| ----------------------- | -------------- | ---------------- | ----------------- | ------------------ | --------------------- | --------------- | ------- | ------------ | -------------- |
| 1 - Administración     | 500            | 8190             | 192.25.0.0        | 192.25.0.1         | 192.25.31.254         | 255.255.224.0   | /19     | 192.25.0.1   | 192.25.31.255  |
| 2 - Recursos humanos    | 1000           | 8190             | 192.25.32.0       | 192.25.32.1        | 192.25.61.254         | 255.255.224.0   | /19     | 192.25.32.1  | 192.25.61.255  |
| 3 - Producción         | 1000           | 8190             | 192.25.64.0       | 192.25.61.1        | 192.25.91.254         | 255.255.224.0   | /19     | 192.25.61.1  | 192.25.91.255  |
| 4 - Finanzas            | 1500           | 8190             | 192.25.96.0       | 192.26.96.1        | 192.25.127.254        | 255.255.224.0   | /19     | 192.26.96.1  | 192.25.127.255 |
| 5 - Marketing           | 2000           | 8190             | 192.25.128.0      | 192.25.128.1       | 192.25.159.254        | 255.255.224.0   | /19     | 192.25.128.1 | 192.25.159.255 |
| 6 - Ventas              | 3500           | 8190             | 192.25.160.0      | 192.25.160.1       | 192.25.191.254        | 255.255.224.0   | /19     | 192.25.160.1 | 192.25.191.255 |
| 7 - Control de Gestión | 5000           | 8190             | 192.25.192.0      | 192.25.192.1       | 192.25.223.254        | 255.255.224.0   | /19     | 192.25.192.1 | 192.25.223.255 |
