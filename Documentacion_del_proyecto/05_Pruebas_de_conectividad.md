# Pruebas de Conectividad por Dispositivo de red

## 1 Equipos de Red (Enrutamiento, Gestión y Conmutación)

| Dispositivo | Comandos de Verificación y Conectividad                                                                                                                                  | Resultados de consola |
| :---------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------- |
| **`R1`**    | `show ip interface brief`<br>`show ipv6 interface brief`<br>`show ip route` / `show ipv6 route`<br>`ping <Hacia SWM1>` / `ping ipv6 <Hacia SWM1>`                        | [[Resultado_R1]]      |
| **`SWM1`**  | `show ip interface brief`<br>`show ipv6 interface brief`<br>`show ip route` / `show ipv6 route`<br>`show interfaces trunk`<br>`ping <Hacia R1>` / `ping ipv6 <Hacia R1>` | [[Resultados_SWM1]]   |
| **`SW`**    | `show ip interface brief`<br>`show vlan brief`<br>`show ip ssh`<br>                                                                                                      | [[Resultados_SW]]     |
| **`SW3`**   | `show ip interface brief`<br>`show interfaces status`<br>`show interfaces trunk`<br>`show vtp status`                                                                    | [[Resultados_SW3]]    |
| **`SW4`**   | `show ip interface brief`<br>`show interfaces trunk`<br>`show etherchannel summary`<br>`show vtp status`                                                                 | [[Resultados_SW4]]    |
| **`SW5`**   | `show ip interface brief`<br>`show interfaces trunk`<br>`show etherchannel summary`<br>`show vtp status`                                                                 | [[Resultados_SW5]]    |
|             |                                                                                                                                                                          |                       |

---

