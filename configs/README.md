# Конфигурации устройств

В папке находятся конфигурации устройств проекта Cisco Packet Tracer.

## Состав

| Файл | Устройство |
|---|---|
| `Router0.cfg` | edge-роутер центрального офиса |
| `Router1.cfg` | второй edge-роутер центрального офиса |
| `Router2.cfg` | провайдерская сеть / интернет |
| `Router3.cfg` | edge-роутер филиала |
| `MultilayerSwitch0.cfg` | L3-коммутатор центрального офиса |
| `MultilayerSwitch1.cfg` | L3-коммутатор центрального офиса |
| `Switch0.cfg` | access-коммутатор центрального офиса |
| `Switch1.cfg` | access-коммутатор центрального офиса |
| `Switch2.cfg` | access-коммутатор филиала |
| `ENDPOINTS.md` | параметры конечных устройств |

## Учетные данные

| Назначение | Значение |
|---|---|
| Enable secret | `class` |
| Console/VTY password | `cisco` |
| IPsec pre-shared key | `cisco123` |

## Используемые интерфейсы

В топологии используются интерфейсы `Fa1/0` у `Router0`, `Router1` и `Router2`.

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

BGP используется только между физическими роутерами:

- `Router0` AS 65010;
- `Router1` AS 65011;
- `Router2` AS 65000;
- `Router3` AS 65010.

Маршруты LAN между центральным офисом и филиалом передаются не BGP, а OSPF через туннели `Tunnel10` и `Tunnel20`.
