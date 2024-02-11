SNMP Notification Receiver Service for Networking Labs
======================================================

A simple SNMP notification receiver service for networking labs. The service
uses [Net-SNMP snmptrapd][SnmptrapdManual].

[SnmptrapdManual]: https://net-snmp.sourceforge.io/docs/man/snmptrapd.html

Basic Usage
-----------

Specify access control settings in `snmptrapd.conf`.

Start the service:

```sh
docker compose up --detach
```

Print logs:

```sh
docker compose logs
```

Stop the service:

```sh
docker compose down
```

Testing
-------

Add the following settings to the `snmptrapd.conf` file:

```
authCommunity log public

createUser -e 0x800002b801c0a80001 foo SHA 12345678 AES
authUser log foo authPriv
```

Send a test SNMP trap (`CISCO-SYSLOG-MIB::clogMessageGenerated`):

```sh
snmptrap \
    -v 2c \
    -c public \
    localhost \
    "" \
    .1.3.6.1.4.1.9.9.41.2.0.1

snmptrap \
    -v 3 \
    -e 0x800002b801c0a80001 \
    -u foo \
    -l authPriv \
    -a SHA -A 12345678 \
    -x AES -X 12345678 \
    localhost \
    "" \
    .1.3.6.1.4.1.9.9.41.2.0.1
```
