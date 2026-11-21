# Direccionamiento IP, VLANs, Diseño VTP y Capacidad de Hosts — PRODUCTO: EQUIPO-3


Tabla de dirrecionamiento de la red

| VLAN / Segmento     | Nombre / Rol    | Subred IPv4     | Máscara / CIDR            | Gateway IPv4           | Prefijo IPv6        | Gateway IPv6             | Dispositivo Gateway / Host                                      |           |
| :------------------ | :-------------- | :-------------- | :------------------------ | :--------------------- | :------------------ | :----------------------- | :-------------------------------------------------------------- | --------- |
| **VLAN 10**         | `DOCENTES`      | `172.20.8.0`    | `255.255.255.0` (`/24`)   | `172.20.8.1`           | `2001:25:20:1::/64` | `2001:25:20:1::1`        | `R1` (`Gi0/1.10`) / `DOC1`                                      |           |
| **VLAN 20**         | `ESTUDIANTES`   | `172.20.0.0`    | `255.255.248.0` (`/21`)   | `172.20.0.1`           | `2001:25:20::/64`   | `2001:25:20::1`          | `R1` (`Gi0/1.20`) / `EST1`                                      |           |
| **VLAN 30**         | `GESTION`       | `172.20.10.64`  | `255.255.255.224` (`/27`) | `172.20.10.65`         | `2001:25:20:5::/64` | `2001:25:20:5::1`        | `R1` (`Gi0/1.30`) / `GES1`                                      |           |
| **VLAN 40**         | `VOZ`           | `172.20.10.96`  | `255.255.255.224` (`/27`) | `172.20.10.97`         | `2001:25:20:6::/64` | `2001:25:20:6::1`        | `R1` (`Gi0/1.40`)                                               |           |
| **VLAN 50**         | `FINANZAS`      | `172.20.9.128`  | `255.255.255.128` (`/25`) | `172.20.9.129`         | `2001:25:20:3::/64` | `2001:25:20:3::1`        | `SWM1` (`Vlan50`) / `FIN1`                                      |           |
| **VLAN 60**         | `ACADEMICOS`    | `172.20.9.0`    | `255.255.255.128` (`/25`) | `172.20.9.1`           | `2001:25:20:2::/64` | `2001:25:20:2::1`        | `SWM1` (`Vlan60`) / `ACA1`                                      |           |
| **VLAN 70**         | Acceso en `SW`  | —               | —                         | —                      | —                   | —                        | `SW` (`Gi0/0`, `Gi0/1`)                                         |           |
| **VLAN 999**        | `BLACKHULL`     | —               | —                         | —                      | —                   | —                        | VLAN de contención / sin uso                                    |           |
| **LAN Servidor**    | `SRV1` (Alpine) | `172.20.10.0`   | `255.255.255.192` (`/26`) | `172.20.10.1`          | `2001:25:20:4::/64` | `2001:25:20:4::1`        | `SWM1` (`Gi0/2`) / `SRV1` (`172.20.10.2`, `2001:25:20:4::2`)    |           |
| **Enlace Tránsito** | `R1` ↔ `SWM1`   | `172.20.10.160` | `255.255.255.252` (`/30`) | `172.20.10.161` (`R1`) | `2001:25:20:9::/64` | `2001:25:20:9::1` (`R1`) | `R1` (`Gi0/0` `.161` / `::1`) ↔ `SWM1` (`Gi0/1` `.162` / `::2`) | [cite: 1] |

### Servicios DHCP configurados
* **En `R1`:** *Pools* DHCPv4 y DHCPv6 para `DOCENTES` (`172.20.8.0/24` / `2001:25:20:1::/64`), `ESTUDIANTES` (`172.20.0.0/21` / `2001:25:20::/64`) y `GESTION` (`172.20.10.64/27` / `2001:25:20:5::/64`), apuntando como servidor DNS a `172.20.10.2` y `2001:25:20:4::2` (`SRV1`).
* **En `SWM1`:** *Pools* DHCPv4 y DHCPv6 para `ACADEMICOS` (`172.20.9.0/25` / `2001:25:20:2::/64`) y `FINANZAS` (`172.20.9.128/25` / `2001:25:20:3::/64`), también con DNS en `172.20.10.2` y `2001:25:20:4::2`.

---

## 2. Diseño de VTP en la Red

La red está configurada con **VTP versión 3**, utilizando el dominio **`producto1`** y la contraseña **`producto1`**, automatizada mediante *applets* del *Embedded Event Manager* (EEM) al iniciar los switches:

* **Dominio y autenticación:** Todos los switches participantes emplean `vtp version 3`, `vtp domain producto1` y `vtp password producto1`.
* **Servidores VTP y promoción primaria (`vtp primary vlan force`):**
  * **`SW4`** se configura como `vtp mode server` y ejecuta `vtp primary vlan force` (tras un temporizador de 60 s), creando y administrando las VLANs **10 (`DOCENTES`)**, **20 (`ESTUDIANTES`)**, **30 (`GESTION`)**, **40 (`VOZ`)** y **999 (`BLACKHULL`)**.
  * **`SWM1`** también se configura como `vtp mode server` y ejecuta `vtp primary vlan force` (temporizador de 60 s), creando las VLANs **50 (`FINANZAS`)**, **60 (`ACADEMICOS`)** y **999 (`BLACKHULL`)**, además de realizar un ciclo de `shutdown` / `no shutdown` en las interfaces `Vlan50` y `Vlan60`.
* **Clientes VTP:**
  * **`SW3`** y **`SW5`** se configuran como `vtp mode client` (temporizador de 30 s) dentro del dominio `producto1` para recibir la base de datos de VLANs.
* **Explicacion del diseño:** Tener dos switches distintos (`SW4` y `SWM1`) declarados como `vtp mode server` y ejecutando `vtp primary vlan force` en distintos segmentos de la topología responde a que la base de datos de VLANs de cada rama se administra desde su respectivo servidor VTPv3 (por ejemplo, `SW4` con VLANs 10–40 y `SWM1` con VLANs 50–60), mientras que los enlaces troncales filtran explícitamente las VLANs permitidas (`switchport trunk allowed vlan ...`).
* **Aclaracion tecnica**: debido a la ausencia de archivos claves para el funcionamiento de la red del laboratorio en un primer inicio como lo es ==🔴**vlan.dat**== para guardar las vlan creadas en los switches , se tuvo que implementar el script correspondiente a la creacion de las mismas con EEM

---

## 3. Cantidad de Hosts Suplidos en la Red


| Segmento / Subred | Máscara / Prefijo | Bits de host ($h$) | Hosts IPv4 utilizables ($2^h - 2$) | Rango IPv4 útil |
| :--- | :--- | :---: | :---: | :--- |
| **VLAN 20 (`ESTUDIANTES`)** | `/21` (`255.255.248.0`) | 11 | **2,046** | `172.20.0.1` – `172.20.7.254` |
| **VLAN 10 (`DOCENTES`)** | `/24` (`255.255.255.0`) | 8 | **254** | `172.20.8.1` – `172.20.8.254` |
| **VLAN 60 (`ACADEMICOS`)** | `/25` (`255.255.255.128`) | 7 | **126** | `172.20.9.1` – `172.20.9.126` |
| **VLAN 50 (`FINANZAS`)** | `/25` (`255.255.255.128`) | 7 | **126** | `172.20.9.129` – `172.20.9.254` |
| **LAN Servidor (`SRV1`)** | `/26` (`255.255.255.192`) | 6 | **62** | `172.20.10.1` – `172.20.10.62` |
| **VLAN 30 (`GESTION`)** | `/27` (`255.255.255.224`) | 5 | **30** | `172.20.10.65` – `172.20.10.94` |
| **VLAN 40 (`VOZ`)** | `/27` (`255.255.255.224`) | 5 | **30** | `172.20.10.97` – `172.20.10.126` |
| **Enlace `R1` ↔ `SWM1`** | `/30` (`255.255.255.252`) | 2 | **2** *(punto a punto)* | `172.20.10.161` – `172.20.10.162` |[cite: 1]

* **Total de direcciones IPv4 utilizables en las subredes de usuarios, voz y servidores:**
  $$2,046 + 254 + 126 + 126 + 62 + 30 + 30 = \mathbf{2,604\text{ hosts IPv4}}$$
  
* **En IPv6:** Cada segmento `/64` proporciona un espacio de direcciones de $2^{64}$ (`18,446,744,073,709,551,616` direcciones), suficiente para cubrir ampliamente el crecimiento de hosts sin agotamiento de direcciones.