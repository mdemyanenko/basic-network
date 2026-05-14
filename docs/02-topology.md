# 2. Топология сети

## Логическая схема

Основная схема проекта:

`pics/packet_tracer_topology.png`



## Оборудование

| Имя | Модель в Packet Tracer | Роль |
|---|---|---|
| `Router0` | Router 2811 | edge-роутер центрального офиса |
| `Router1` | Router 2811 | второй edge-роутер центрального офиса |
| `Router2` | Router 2811 | сеть интернет/провайдер |
| `Router3` | Router 2811 | edge-роутер филиала |
| `MultilayerSwitch0` | 3560-24PS | L3-коммутатор центрального офиса |
| `MultilayerSwitch1` | 3560-24PS | L3-коммутатор центрального офиса |
| `Switch0` | 2960-24TT | access-коммутатор центрального офиса |
| `Switch1` | 2960-24TT | access-коммутатор центрального офиса |
| `Switch2` | 2960-24TT | access-коммутатор филиала |
| `PC0`, `PC3`, `PC4`, `PC5` | PC-PT | ПК центрального офиса |
| `PC1`, `PC2` | PC-PT | ПК филиала |

## Таблица подключений

| Устройство A | Порт A | Устройство B | Порт B | Назначение |
|---|---|---|---|---|
| `Router0` | `Fa0/0` | `MultilayerSwitch0` | `Fa0/4` | OSPF inside |
| `Router0` | `Fa0/1` | `Router1` | `Fa1/0` | eBGP между edge-роутерами |
| `Router0` | `Fa1/0` | `Router2` | `Fa0/0` | internet/ISP, NAT outside |
| `Router1` | `Fa0/0` | `MultilayerSwitch1` | `Fa0/4` | OSPF inside |
| `Router1` | `Fa0/1` | `Router2` | `Fa0/1` | internet/ISP, NAT outside |
| `Router2` | `Fa1/0` | `Router3` | `Fa0/0` | internet/ISP to branch |
| `Router3` | `Fa0/1` | `Switch2` | `Fa0/1` | trunk VLAN 50,60,97 |
| `MultilayerSwitch0` | `Fa0/1` | `Switch0` | `Fa0/3` | trunk VLAN 10,20,99 |
| `MultilayerSwitch1` | `Fa0/1` | `Switch1` | `Fa0/3` | trunk VLAN 30,40,98 |
| `MultilayerSwitch0` | `Fa0/2` | `MultilayerSwitch1` | `Fa0/2` | L2 EtherChannel trunk |
| `MultilayerSwitch0` | `Fa0/3` | `MultilayerSwitch1` | `Fa0/3` | L2 EtherChannel trunk |
| `Switch0` | `Fa0/1` | `PC0` | `Fa0` | VLAN 10 |
| `Switch0` | `Fa0/2` | `PC3` | `Fa0` | VLAN 20 |
| `Switch1` | `Fa0/1` | `PC4` | `Fa0` | VLAN 30 |
| `Switch1` | `Fa0/2` | `PC5` | `Fa0` | VLAN 40 |
| `Switch2` | `Fa0/2` | `PC1` | `Fa0` | VLAN 50 |
| `Switch2` | `Fa0/3` | `PC2` | `Fa0` | VLAN 60 |

## Важное по Router 2811

В схеме используются интерфейсы `Fa1/0`. У Router 2811 в Packet Tracer они могут отсутствовать по умолчанию. Если порта нет:

1. открыть вкладку `Physical`;
2. выключить питание роутера;
3. добавить FastEthernet-модуль;
4. включить питание;
5. проверить, что появился интерфейс `FastEthernet1/0`.
