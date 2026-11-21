

En el laboratorio actual el protocolo **SSH (Secure Shell)** se utiliza para la administración remota cifrada de los dispositivos de red, sustituyendo a protocolos inseguros como Telnet y asegurando que las credenciales y comandos viajen protegidos por la red.

### 1. Cómo está configurado SSH en los dispositivos de la topología
El acceso remoto por SSH presenta las siguientes características en las configuraciones guardadas:

* **Versión y cifrado:** Los switches (SW, SW3`, `SW4`, `SW5` y `SWM1`) tienen habilitado `ip ssh version 2` y algoritmos de cifrado AES en modo CTR (`aes128-ctr`, `aes192-ctr`, `aes256-ctr`) tanto para servidor como para cliente SSH, mientras que `R1` tiene configurado `ip ssh version 2`.
* **Autenticación local:** En los switches y en `R1` las líneas virtuales (`line vty 0 4` y `line vty 5 15`) están configuradas con `login local` y `transport input ssh`, lo que exige validar un usuario local con privilegio `15` (como `Admin` o `20260474` en los switches, o `Admin` en `R1`).
* **Diferencias entre nodos:**
  * **Switches (`SW` `SW3`, `SW4`, `SW5`, `SWM1`):** Cuentan con usuarios locales de privilegio 15, `ip domain-name itla.local`, `ip ssh version 2`, cifrado AES-CTR y líneas VTY restringidas a `transport input ssh`. .
  * **Router `R1`:** Tiene `ip ssh version 2`, usuario local `Admin` (privilegio 15) y líneas VTY con `login local` y `transport input ssh`, además de interfaces/subinterfaces IPv4 e IPv6 alcanzables por red.
  * **Hosts y servidor (`SRV1`, `EST1`, `FIN1`, `ACA1`, `GES1`, `DOC1`):** Son nodos finales/servidores (no switches/routers IOS con líneas VTY de administración de red), por lo que actúan como estaciones de origen/prueba o destinos de red según el servicio disponible en cada host.

