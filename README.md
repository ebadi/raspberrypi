# Raspberrypi

## Raspberrypi zero w as PiHole:

Install Raspberry Pi OS using Raspberry Pi Imager.

- in boot : create an empty ssh file to enable ssh server

- in boot : create a wpa_supplicant.conf file

  ```
  ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
  update_config=1
  country=US
  
  network={
       ssid="Your network name/SSID (Do not use the 5GHz network, use 2.4GHz)"
       psk="Your WPA/WPA2 security key"
       key_mgmt=WPA-PSK
  }
  ```

- update config.txt :  hdmi_force_hotplug=1

`curl -sSL https://install.pi-hole.net | bash`

This step assigns a static IP address.

GroupManagement -> Adlists -> add all lists in https://firebog.net/ (space seperated)

ssh pi@ip

`vcgencmd commands`

`vcgencmd measure_temp`

```
Active Temp: 42.8 °C
Load:  0.06  0.13  0.08
Memory usage:  15.6 %
```

`sudo raspi-config`

Change hostname

More setting: Disable GUI, boot into cli instead.

Web UI password: `pihole -a -p`

## Raspberrypi zero w as Wireless monitor

```
wget -O re4son-kernel_current.tar.xz https://re4son-kernel.com/download/re4son-kernel-current/
tar -xJf re4son-kernel_current.tar.xz
cd re4son-kernel_4*
sudo ./install.sh
```
After reboot:
```
sudo iw phy phy0 interface add mon0 type monitor
sudo ifconfig mon0 up
sudo apt install aircrack-ng


wget https://dl.google.com/go/go1.14.2.linux-armv6l.tar.gz
sudo tar -C /usr/local -xzf go1.14.2.linux-armv6l.tar.gz
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/go

git clone https://github.com/sausheong/netnet.git
git clone https://github.com/ebadi/netnet.git
cd netnet
go build

```

### Manual Test:

```
sudo airodump-ng mon0
sudo airodump-ng --channel 1-13,36-165 --write dump --output-format csv mon0
./netnet -f=/home/pi/dump-01.csv
# check port 12121
```

### Automating 

- `sudo nano /lib/systemd/system/airodump.service`

```
[Unit]
Description=airodump

[Service]
Type=idle
ExecStart=/home/pi/airodump.sh
```

- `sudo nano /lib/systemd/system/netnet.service`

```
[Unit]
Description=netnet

[Service]
Type=idle
ExecStart=/home/pi/netnet/netnet -f=/home/pi/dump-01.csv
```

- `sudo /etc/rc.local`

```
sudo systemctl start airodump
sleep 15s
sudo systemctl start netnet
```

``` sudo systemctl start netnet
## Test to see if there is any error:
sudo ./etc/rc.local

sudo systemctl status airodump
sudo systemctl status netnet
```

Static IP address

```
interface wlan0
static ip_address=192.168.10.14/24
#static ip6_address=fd51:42f8:caae:d92e::ff/64
static routers=192.168.10.1
```

Temperture

```vcgencmd measure_temp
vcgencmd measure_temp
temp=57.3'C
```

