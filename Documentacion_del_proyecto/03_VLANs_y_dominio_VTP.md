## VLANs y Diseño del Dominio VTP

### 1. Segmentación mediante VLANs
La red separa el tráfico de usuarios, servicios y administración en VLANs específicas, combinando enlaces de acceso, enlaces troncales (`802.1Q`) y agregación de enlaces (`Port-channel 1` en modo `active` / LACP) entre `SW4` y `SW5`.

| VLAN    | Nombre        | Subred IPv4       | Prefijo IPv6        | Dispositivos donde está configurada o transportada                                                                     |
| :------ | :------------ | :---------------- | :------------------ | :--------------------------------------------------------------------------------------------------------------------- |
| **10**  | `DOCENTES`    | `172.20.8.0/24`   | `2001:25:20:1::/64` | `SW4` (creada vía EEM; acceso `Gi0/3` hacia `DOC1`; troncal `Gi0/0` y `Po1`), `SW5` (troncal `Po1`), `R1` (`Gi0/1.10`) |
| **20**  | `ESTUDIANTES` | `172.20.0.0/21`   | `2001:25:20::/64`   | `SW4` (creada vía EEM; troncal `Gi0/0` y `Po1`), `SW5` (troncal `Po1`; acceso `Gi0/0` hacia `EST1`), `R1` (`Gi0/1.20`) |
| **30**  | `GESTION`     | `172.20.10.64/27` | `2001:25:20:5::/64` | `SW4` (creada vía EEM; troncal `Gi0/0` y `Po1`), `SW5` (troncal `Po1`; acceso `Gi0/3` hacia `GES1`), `R1` (`Gi0/1.30`) |
| **40**  | `VOZ`         | `172.20.10.96/27` | `2001:25:20:6::/64` | `SW4` (creada vía EEM; troncal `Gi0/0` y `Po1`), `SW5` (troncal `Po1`), `R1` (`Gi0/1.40`)                              |
| **50**  | `FINANZAS`    | `172.20.9.128/25` | `2001:25:20:3::/64` | `SWM1` (creada vía EEM + SVI `Vlan50`; troncal `Gi0/0`), `SW3` (troncal `Gi0/0`; acceso `Gi0/1` hacia `FIN1`)          |
| **60**  | `ACADEMICOS`  | `172.20.9.0/25`   | `2001:25:20:2::/64` | `SWM1` (creada vía EEM + SVI `Vlan60`; troncal `Gi0/0`), `SW3` (troncal `Gi0/0`; acceso `Gi0/2` hacia `ACA1`)          |
| **70**  | `DATACENTER`  | —                 | —                   | `SW` (acceso en `Gi0/0` y `Gi0/1`)                                                                                     |
| **999** | `BLACKHULL`   | —                 | —                   | `SW4` y `SWM1` (creada vía EEM como VLAN de contención / sumidero)                                                     |

### 2. Funcionamiento del Dominio VTP (`producto1`)
El laboratorio utiliza **VTP versión 3** bajo el dominio **`producto1`** y la contraseña **`producto1`**, aplicando los parámetros mediante *Embedded Event Manager* (EEM) tras el arranque de los switches:

* **Servidores VTP (`vtp mode server`):**
  * **`SW4` (`VTP_SW4`, temporizador de 60 s):** Opera en modo `server` y ejecuta `vtp primary vlan force` para convertirse en servidor primario y definir las VLANs `10`, `20`, `30`, `40` y `999`.
  * **`SWM1` (`VTP_SWM1`, temporizador de 60 s):** Opera en modo `server` y ejecuta `vtp primary vlan force` para definir las VLANs `50`, `60` y `999`, reiniciando además las interfaces virtuales `Vlan50` y `Vlan60` (`shutdown` / `no shutdown`).
* **Clientes VTP (`vtp mode client`):**
  * **`SW3` (`VTP_SW3`, temporizador de 30 s):** Opera en modo `client` dentro del dominio `producto1`, recibiendo la información de VLANs por su enlace troncal `Gi0/0` desde `SWM1` y permitiendo las VLANs `50,60`.
  * **`SW5` (`VTP_SW5`, temporizador de 30 s):** Opera en modo `client` dentro del dominio `producto1`, recibiendo la información de VLANs a través de `Port-channel 1` (`Gi0/1`–`Gi0/2`) desde `SW4` con las VLANs permitidas `10,20,30,40`.
* **Switches sin configuración VTP explícita en el archivo:**
  * **`SW`:** Configura puertos de acceso en la VLAN `70` (`Gi0/0` y `Gi0/1`), sin *applet* EEM ni declaración explícita de dominio/modo VTP en su configuración guardada.

Nota: Se asignó una dirección IP a la SVI de VLAN 70 en SW para habilitar el plano de gestión del switch, requisito para el servicio SSH. La validación inicial se realizó desde SRV1 por pertenecer al mismo dominio de broadcast, lo que aísla fallas de configuración local de fallas de enrutamiento

---

### 3. Diagrama de VLANs y Roles VTP por Dispositivo

```mermaid
flowchart TD
    classDef server fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px,color:#000
    classDef client fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,color:#000
    classDef other fill:#fff8e1,stroke:#f57f17,stroke-width:2px,color:#000
    classDef router fill:#ede7f6,stroke:#4527a0,stroke-width:2px,color:#000

    subgraph VTP_DOM["Dominio VTPv3: producto1 (Pass: producto1)"]
        SW4["<b>SW4 — VTP SERVER (Primary)</b><br/>EEM (60s): vtp primary vlan force<br/>Crea VLANs: 10, 20, 30, 40, 999<br/>Troncal Gi0/0 y Po1 (Gi0/1-2): VLANs 10,20,30,40<br/>Acceso Gi0/3: VLAN 10 (DOC1)"]:::server

        SW5["<b>SW5 — VTP CLIENT</b><br/>EEM (30s): vtp mode client<br/>Recibe VLANs por Po1 (Gi0/1-2): 10,20,30,40<br/>Acceso Gi0/0: VLAN 20 (EST1)<br/>Acceso Gi0/3: VLAN 30 (GES1)"]:::client

        SWM1["<b>SWM1 — VTP SERVER (Primary)</b><br/>EEM (60s): vtp primary vlan force<br/>Crea VLANs: 50, 60, 999<br/>SVIs: Vlan50 (Finanzas), Vlan60 (Académicos)<br/>Troncal Gi0/0: VLANs 50,60"]:::server

        SW3["<b>SW3 — VTP CLIENT</b><br/>EEM (30s): vtp mode client<br/>Recibe VLANs por Gi0/0: 50,60<br/>Acceso Gi0/1: VLAN 50 (FIN1)<br/>Acceso Gi0/2: VLAN 60 (ACA1)"]:::client
    end

    SW["<b>SW — Sin VTP configurado en lab</b><br/>Acceso Gi0/0 y Gi0/1: VLAN 70<br/>Enlace hacia SRV1 (Gi0/1) y SWM1 (Gi0/0)"]:::other

    R1["<b>R1 — Router Inter-VLAN</b><br/>Subinterfaces en Gi0/1:<br/>.10 (VLAN 10) | .20 (VLAN 20)<br/>.30 (VLAN 30) | .40 (VLAN 40)"]:::router

    SW4 <-->|"Po1 (Gi0/1-2) Trunk<br/>VLANs 10,20,30,40"| SW5
    SW4 ---|"Gi0/0 Trunk<br/>VLANs 10,20,30,40"| R1
    SWM1 ---|"Gi0/0 Trunk<br/>VLANs 50,60"| SW3
    SWM1 ---|"Gi0/0"| SW
    ```