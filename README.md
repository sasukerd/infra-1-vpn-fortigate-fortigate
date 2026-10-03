# Infraestructura 1: VPN site-to-site entre dos FortiGate

Matrícula: **2025-0873**

## Objetivo

Comunicar el usuario con el servidor web a través del enlace VPN y comprobar que esa comunicación solo fluye si el túnel está activo.

## Topología

- ISP: Ubuntu, solo enruta las IP públicas.
- FG-A: FortiGate del sitio de usuarios. VLAN 10, DHCP y NAT de salida.
- FG-B: FortiGate del sitio del servidor.
- PC-User: VPCS, cliente DHCP.
- WEB-SRV: servidor HTTPS.

La VPN no tiene cable propio. Es un túnel IPsec entre `202.5.0.2` y `8.73.0.2`.

## Direccionamiento

| Equipo | Interfaz | IP |
|---|---|---|
| ISP | ens3 | 202.5.0.1/30 |
| FG-A | port1 WAN | 202.5.0.2/30 |
| ISP | ens4 | 8.73.0.1/30 |
| FG-B | port1 WAN | 8.73.0.2/30 |
| FG-A | port2 LAN | 10.20.25.1/25 |
| PC-User | e0 | DHCP 10.20.25.20-100/25 |
| FG-B | port2 LAN | 10.8.73.1/28 |
| WEB-SRV | ens3 | 10.8.73.10/28 |

Tráfico interesante: `10.20.25.0/25` hacia `10.8.73.0/28`.

El `2025` y el `0873` de la matrícula quedan en las redes públicas `202.5.0.0/30` y `8.73.0.0/30`, y en las privadas `10.20.25.0/25` y `10.8.73.0/28`.

## Qué se configuró

- Interfaces y rutas por defecto en los dos FortiGate.
- DHCP de la VLAN 10 en FG-A.
- NAT solo en la política de salida a Internet.
- Túnel IPsec site-to-site, sin NAT en las políticas del túnel.
- Servidor web HTTPS.

## Cómo se demuestra el objetivo

1. Con el túnel arriba, el PC hace ping y traceroute a `10.8.73.10`. El primer salto es `10.20.25.1`.
2. Se baja el túnel y el mismo ping falla.
3. Se levanta el túnel y el ping vuelve a responder.

## Video

Enlace del video de la infraestructura 1:

https://

Reemplaza esa línea por la URL del video cuando lo subas.
