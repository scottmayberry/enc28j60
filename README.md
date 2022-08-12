# enc28j60
![enj28j60 module](enc28j60_module_pic.jpg?raw=true)
The enc28j60 is an ethernet module with an SPI interface. This project includes the schematics, gerber files, BOM, and pick and place files for a SMT only enc28j60 board. This board can either be ordered directly, or be integrated into other projects. On the raspberry pi zero W, speeds up to 4Mb/s have been seen.

This board was designed with EasyEda, and can be easily ordered from JLCPCB.com. The easyeda zip file can be loaded into the online editor https://easyeda.com/editor.

Project also availble on OSHWLab: https://oshwlab.com/sctmayberry/ethernet_raspi

# Schematics and PCB
![Schematic of enj28j60 module](Schematic_ethernet_raspi_2022-08-12.png?raw=true)
![PCB of enj28j60 module](PCB_PCB_ethernet_raspi.png?raw=true)

# Raspberry Pi Integration
## Setup
Software integration steps are taken from: https://www.raspberrypi-spy.co.uk/2020/05/adding-ethernet-to-a-pi-zero/. For posterity sake, they are listed here as well.

Enable Spi and enable dtoverlay of enc28j60

`sudo nano /boot/config.txt`

Uncomment (delete # character) from the line:
`#dtparam=spi=on`
Add line:
`dtoverlay=encj28j60`
Reboot
`sudo reboot`
The device should now be working.

## Set Mac Address
edit the file:
`sudo nano /lib/systemd/system/setmac.service`
and add:
`[Unit]
Description=Set MAC address for ENC28J60 module
Wants=network-pre.target
Before=network-pre.target
BindsTo=sys-subsystem-net-devices-eth0.device
After=sys-subsystem-net-devices-eth0.device
[Service]
Type=oneshot
ExecStart=/sbin/ip link set dev eth0 address b8:27:eb:00:00:01
ExecStart=/sbin/ip link set dev eth0 up
[Install]
WantedBy=multi-user.target`
Finally, run:
`sudo chmod 644 /lib/systemd/system/setmac.service
sudo systemctl daemon-reload
sudo systemctl enable setmac.service`
And reboot:
`sudo reboot`
