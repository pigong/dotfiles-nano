## Run "tegrastats"

Space separated table of processor frequencies, temperatures and power consumption.
Multiple arguments, e.g. --readall

```
sudo tegrastats --readall
```

## Run jetson_clocks script to maximize performancs

```
sudo jetson_clocks
```

## Disable USB device mode temporarily

```
sudo service nv-l4t-usb-device-mode stop/start
```

## Disable USB device mode permanently

```
sudo service nv-l4t-usb-device-mode stop/start
sudo systemctl disable nv-l4t-usb-device-mode.service
```
