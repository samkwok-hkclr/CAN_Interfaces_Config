# Configuring CAN Interfaces on Linux
This guide will help you set up CAN interfaces using systemd-networkd and udev rules.

1. Create a Network Configuration File
Create a file named /etc/systemd/network/80-can.network with the following content:
```bash
[Match]
Name=can0

[CAN]
BitRate=500K
RestartSec=100ms
```
This configuration matches network interfaces with "can0" and sets the bitrate to 500K.

2. Enable systemd-networkd
Run the following command to enable systemd-networkd:
```bash
sudo systemctl enable systemd-networkd
```
This command ensures that the systemd-networkd service starts at boot.

3. Start systemd-networkd
Start the systemd-networkd service with:
```bash
sudo systemctl start systemd-networkd
```
This command will activate the networking service immediately.

4. Create Udev Rules for CAN Interfaces
Create a file named /etc/udev/rules.d/80-can.rules with the following content:
```bash
SUBSYSTEM=="net", KERNEL=="can0", ACTION=="add|change", ATTR{tx_queue_len}="1000"
```
This rule sets the transmit queue length for CAN interfaces to 1000 whenever they are added or changed.

5. Trigger an Update
After creating the udev rules, run the following commands to reload the rules and trigger an update:
```bash
sudo udevadm control --reload-rules
sudo udevadm trigger --subsystem-match=net --action=change
```
These commands ensure that the new rules take effect without needing to reboot the system.

## Summary
These steps set up CAN interfaces on your Linux system using systemd for network management and udev for device management. Make sure to adjust configurations based on your specific requirements.
