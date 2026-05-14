# 6. Проверка работоспособности

## Проверка VLAN

На access-коммутаторах:

```text
show vlan brief
show interfaces trunk
```

Ожидаемый результат:

- `Switch0`: `Fa0/1` в VLAN 10, `Fa0/2` в VLAN 20, `Fa0/3` trunk;
- `Switch1`: `Fa0/1` в VLAN 30, `Fa0/2` в VLAN 40, `Fa0/3` trunk;
- `Switch2`: `Fa0/2` в VLAN 50, `Fa0/3` в VLAN 60, `Fa0/1` trunk.

## Проверка EtherChannel

На `MultilayerSwitch0` и `MultilayerSwitch1`:

```text
show etherchannel summary
show interfaces trunk
```

Ожидаемый результат:

- `Port-channel1` находится в состоянии `SU`;
- `Fa0/2` и `Fa0/3` находятся в состоянии `P`;
- состояния `I` быть не должно;
- через trunk разрешены VLAN `10,20,30,40,98,99,100,999`.

## Проверка HSRP

На `MultilayerSwitch0` и `MultilayerSwitch1`:

```text
show standby brief
show standby
```

Ожидаемый результат:

- VLAN 10 и VLAN 20 active на `MultilayerSwitch0`;
- VLAN 30 и VLAN 40 active на `MultilayerSwitch1`;
- виртуальные gateway-адреса для ПК: `192.168.10.1`, `192.168.20.1`, `192.168.30.1`, `192.168.40.1`.

## Проверка OSPF

На `Router0`, `Router1`, `Router3`, `MultilayerSwitch0`, `MultilayerSwitch1`:

```text
show ip ospf neighbor
show ip route ospf
```

Ожидаемый результат:

- `Router0` видит OSPF-соседа `MultilayerSwitch0`;
- `Router1` видит OSPF-соседа `MultilayerSwitch1`;
- `MultilayerSwitch0` и `MultilayerSwitch1` видят друг друга через `Vlan100`;
- `Router0` видит `Router3` через `Tunnel10`;
- `Router1` видит `Router3` через `Tunnel20`;
- `Router0` и `Router1` знают все офисные сети: `192.168.10.0/24`, `192.168.20.0/24`, `192.168.30.0/24`, `192.168.40.0/24`;
- маршруты филиала `192.168.50.0/24`, `192.168.60.0/24`, `192.168.97.0/24` приходят через OSPF по туннелям.

## Проверка BGP

На роутерах:

```text
show ip bgp summary
show ip route bgp
```

Ожидаемые соседства:

| Устройство | BGP-соседи |
|---|---|
| `Router0` | `10.0.0.2`, `10.0.0.6` |
| `Router1` | `10.0.0.1`, `10.0.0.10` |
| `Router2` | `10.0.0.5`, `10.0.0.9`, `10.0.0.14` |
| `Router3` | `10.0.0.13` |

BGP здесь нужен только для underlay-связности до публичных адресов туннелей. LAN-маршруты офиса и филиала идут через OSPF.

## Проверка GRE-туннелей

На `Router0`, `Router1`, `Router3`:

```text
show ip interface brief
show interface tunnel10
show interface tunnel20
```

На `Router0`:

```text
ping 172.16.10.2
```

На `Router1`:

```text
ping 172.16.20.2
```

## Проверка IPsec

На `Router0`, `Router1`, `Router3`:

```text
show crypto isakmp sa
show crypto ipsec sa
```

Ожидаемый результат:

- ISAKMP SA переходит в состояние `QM_IDLE`;
- счетчики encrypted/decrypted packets растут после ping между офисом и филиалом.

## Проверка NAT

С ПК центрального офиса (`PC0`, `PC3`, `PC4`, `PC5`):

```text
ping 8.8.8.8
```

На `Router0` или `Router1`:

```text
show ip nat translations
show ip nat statistics
```

Ожидаемый результат:

- ping до `8.8.8.8` проходит;
- появляются NAT translations.

## Проверка трафика центральный офис - филиал

С `PC0`:

```text
ping 192.168.50.1
ping 192.168.50.50
```

С `PC1`:

```text
ping 192.168.10.1
ping 192.168.30.1
```

## Проверка ACL гостевой VLAN

С `PC2`:

```text
ping 192.168.60.1
ping 192.168.10.1
ping 192.168.50.1
```

Ожидаемый результат:

- `192.168.60.1` доступен;
- корпоративные подсети недоступны из-за ACL.

На `Router3`:

```text
show access-lists BRANCH_GUEST_IN
```
