# Creality K2 Plus

This repository contains notes w.r.t Creality K2 Plus and my current Klipper configuration.

- [my Klipper config](./klipper-config/)
- [obtain OTA image and original Klipper configs and other blobs](upgrade-img/)

## MQTT - work in progress

- avoid reporting to Winnie the Poop MQTT clound
- bypass to local MQTT server
- note: work in progress since K2 reports a username but no password and Mosquitto requires a non-empty password

1. set up local MQTT (Mosquitto) server with `mosquitto_lan_ip`
2. create new DNS entry (rouer or Pihole): `mqtt.crealitycloud.com` -> `mosquitto_lan_ip`
3. obtain MQTT username from MQTT connection attempt (plaintext in TCP package)
   1. ssh to `mosquitto_lan_ip`
   2. `tcpdump -i <eth0> -n src host <creality_k2_ip> and dst port 1883 -w ~/mqtt-from-k2.pcap`
   3. `wireshark ~/mqtt-from-k2.pcap`
      1. enter "mqtt" to filter textfield
      2. select an arbitrary package
      3. expand "MQ Telemetry Transport Proto." header
      4. see "User Name: <xxxx>"
      5. note: no password is reported
5. TODO:
   1. convince `Mosquitto` to accept user without password
   2. setup `acl_file`
