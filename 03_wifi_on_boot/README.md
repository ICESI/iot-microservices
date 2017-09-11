```
vi /etc/wpa_supplicant/wpa_supplicant.conf
```

```
country=CO
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="yourssid"
    psk="yourpskpassword"
    key_mgmt=WPA-PSK
}
```

Poner un archivo ssh en la partición /boot
