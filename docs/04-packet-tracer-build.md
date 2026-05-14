# 4. Сборка в Cisco Packet Tracer

## 1. Разместить устройства

Добавить:

- 4 роутера `2811`;
- 2 multilayer switch `3560-24PS`;
- 3 switch `2960-24TT`;
- 6 ПК `PC-PT`.

Имена устройств:

`Router0`, `Router1`, `Router2`, `Router3`, `MultilayerSwitch0`, `MultilayerSwitch1`, `Switch0`, `Switch1`, `Switch2`, `PC0`, `PC1`, `PC2`, `PC3`, `PC4`, `PC5`.

## 2. Добавить FastEthernet-модуль

Если у `Router0`, `Router1`, `Router2` нет `Fa1/0`:

1. открыть вкладку `Physical`;
2. выключить питание роутера;
3. вставить FastEthernet-модуль в свободный слот;
4. включить питание;
5. дождаться загрузки устройства.

## 3. Подключить кабели

Использовать таблицу из [02-topology.md](02-topology.md).

Для простоты можно выбрать `Automatically Choose Connection Type`. Если выбирать вручную:

- router-router: Copper Cross-Over;
- router-switch: Copper Straight-Through;
- switch-PC: Copper Straight-Through.

## 4. Вставить конфиги

Рекомендуемый порядок:

| Устройство | Файл |
|---|---|
| `Router2` | `configs/Router2.cfg` |
| `Router0` | `configs/Router0.cfg` |
| `Router1` | `configs/Router1.cfg` |
| `Router3` | `configs/Router3.cfg` |
| `MultilayerSwitch0` | `configs/MultilayerSwitch0.cfg` |
| `MultilayerSwitch1` | `configs/MultilayerSwitch1.cfg` |
| `Switch0` | `configs/Switch0.cfg` |
| `Switch1` | `configs/Switch1.cfg` |
| `Switch2` | `configs/Switch2.cfg` |

## 5. Настроить ПК

На всех ПК:

`Desktop` -> `IP Configuration` -> `DHCP`

Ожидаемые подсети указаны в `configs/ENDPOINTS.md`.

## 6. Сохранить проект

Итоговый файл:

`packet-tracer/enterprise-store-warehouse.pkt`

## 7. Добавить скриншоты

Минимально нужен один скриншот:

`pics/packet_tracer_topology.png`

Дополнительно можно добавить:

- `pics/packet_tracer_bgp.png`;
- `pics/packet_tracer_hsrp.png`;
- `pics/packet_tracer_etherchannel.png`;
- `pics/packet_tracer_ipsec.png`;
- `pics/packet_tracer_nat.png`;
- `pics/packet_tracer_pings.png`.
