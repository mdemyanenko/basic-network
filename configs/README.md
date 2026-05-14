# Конфигурации устройств

Файлы вставляются в CLI соответствующих устройств Cisco Packet Tracer.

## Порядок вставки

1. `Router2.cfg` - сначала интернет-роутер, чтобы underlay BGP был готов.
2. `Router0.cfg`
3. `Router1.cfg`
4. `Router3.cfg`
5. `MultilayerSwitch0.cfg`
6. `MultilayerSwitch1.cfg`
7. `Switch0.cfg`
8. `Switch1.cfg`
9. `Switch2.cfg`
10. `ENDPOINTS.md` - ручная настройка ПК через GUI.

## Пароли

| Назначение | Значение |
|---|---|
| Enable secret | `class` |
| Console/VTY password | `cisco` |
| IPsec pre-shared key | `cisco123` |

## Важное по интерфейсам

На схеме используются интерфейсы `Fa1/0` у `Router0`, `Router1`, `Router2`. Если в Packet Tracer у Router 2811 такого интерфейса нет, выключите роутер и добавьте FastEthernet-модуль, который дает дополнительный порт `Fa1/0`.

## Роли маршрутизаторов

| Устройство | Роль |
|---|---|
| `Router0` | edge-роутер центрального офиса, NAT, OSPF over GRE/IPsec tunnel 10 |
| `Router1` | edge-роутер центрального офиса, NAT, OSPF over GRE/IPsec tunnel 20 |
| `Router2` | имитация сети интернет/провайдера |
| `Router3` | edge-роутер филиала, GRE/IPsec tunnel 10 и tunnel 20 |

## HSRP в центральном офисе

На `MultilayerSwitch0` и `MultilayerSwitch1` настроен HSRP через команды `standby`. ПК получают gateway `.1`, это виртуальный HSRP-адрес.

| VLAN | Виртуальный gateway | Active |
|---:|---|---|
| 10 | `192.168.10.1` | `MultilayerSwitch0` |
| 20 | `192.168.20.1` | `MultilayerSwitch0` |
| 30 | `192.168.30.1` | `MultilayerSwitch1` |
| 40 | `192.168.40.1` | `MultilayerSwitch1` |

## BGP

BGP специально оставлен простым и используется только между физическими роутерами:

- `Router0` AS 65010;
- `Router1` AS 65011;
- `Router2` AS 65000;
- `Router3` AS 65010.

Маршруты LAN между центральным офисом и филиалом передаются не BGP, а OSPF через туннели `Tunnel10` и `Tunnel20`.
