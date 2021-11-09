# autothinkfan
This script makes `/etc/thinkfan.conf` automatically based on the new locations of devices and fans of your machine. 

# Requirements

It just relies on the [thinkfan](https://github.com/vmatare/thinkfan) (the minimalist fan control program). 

# Usage 

Make the script executable with following command and then run it. 

```
chmod a+x autothinkfan
./autothinkfan
```

# Configure `systemctl` to run at every boot

As the location of devices might be relocated at boot time, it would be more convenient to configure it automatically at every boot. The following will help you to get rid of running the script manually. 

```
$ chmod a+x autothinkfan
$ sudo mv autothinkfan /usr/bin

$ echo "#/etc/systemd/system/autothinkfan.service
[Unit]
Description=Thinkfan Auto Config
Wants=lm_sensors.service
After=lm_sensors.service
After=thinkpad_acpi.service
Wants=thinkfan.service
Before=thinkfan.service

[Service]
Type=oneshot
ExecStart=/usr/bin/bash /usr/bin/autothinkfan

[Install]
WantedBy=multi-user.target" | sudo tee /etc/systemd/system/autothinkfan.service

$ sudo chmod 644 /etc/systemd/system/autothinkfan.service
$ sudo systemctl enable autothinkfan.service --nowre
```
