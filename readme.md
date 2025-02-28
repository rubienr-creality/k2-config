# Creality K2 Plus

This repository contains notes w.r.t Creality K2 Plus and my current Klipper configuration.

- [my Klipper config](./klipper-config/)
- [obtain OTA image and original Klipper configs and other blobs](upgrade-img/)

## MQTT / Home Assistant

Aims:
- allows integrating into Home Assitant with Moonraker integration from HACS
- bypass to local MQTT server
- disabling reporting to Winnie the Poop MQTT clound

1. set up local MQTT (Mosquitto) server with `mosquitto_lan_ip`
   1. ```bash
      # example with Mosquitto app on TrueNas
      
      $ cat /mnt/.ix-apps/app_mounts/mosquitto/config/user.conf
      # do not specify `password_file` at all, that ensures user without password is allowed
      acl_file /mosquitto/data/acl_file
      ...

      $ cat /mnt/.ix-apps/app_mounts/mosquitto/data/acl_file
      user <see_next_steps>
      pattern readwrite #
      ...
      ```     
3. create new DNS entry (rouer or Pihole): `mqtt.crealitycloud.com` -> `mosquitto_lan_ip` 
4. sniff MQTT username from MQTT connection (plaintext in TCP package)
   1. ssh to `mosquitto_lan_ip`
   2. `tcpdump -i <eth_ifc> -n src host <creality_k2_ip> and dst port 1883 -w ~/mqtt-from-k2.pcap`
   3. `wireshark ~/mqtt-from-k2.pcap`
      1. enter "mqtt" to filter textfield
      2. select an arbitrary package
      3. expand "MQ Telemetry Transport Proto." header
      4. see "User Name: <xxxx>"
      5. note: no password is reported
5. see also K2 logs:
   1. `ssh root@<k2_lan_ip>`, U: `root`, P: `Creality_2024` (no root settigns required in the display UI)
   2. `tail -f /mnt/UDISK/creality/userdata/log/app-server.log`

