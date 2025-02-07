# BlueDucky
Pentesting Bluetooth

read the owner at https://github.com/pentestfunctions/BlueDucky/tree/main

<p align="center">
  <img src="./images/duckmenu.png">
</p>

🚨 CVE-2023-45866 - BlueDucky Implementation (Using DuckyScript)

🔓 Unauthenticated Peering Leading to Code Execution (Using HID Keyboard)

[This is an implementation of the CVE discovered by marcnewlin](https://github.com/marcnewlin/hi_my_name_is_keyboard)

<p align="center">
  <img src="./images/BlueDucky.gif">
</p>

## Introduction 📢
BlueDucky is a powerful tool for exploiting a vulnerability in Bluetooth devices. By running this script, you can:

1. 📡 Load saved Bluetooth devices that are no longer visible but have Bluetooth still enabled.
2. 📂 Automatically save any devices you scan.
3. 💌 Send messages via ducky script format to interact with devices.

I've successfully run this on a Raspberry Pi 4 using the default Bluetooth module. It works against various phones, with an interesting exception for a New Zealand brand, Vodafone.

## Installation and Usage 🛠️

### Setup Instructions

```bash
# update apt
sudo apt-get update
sudo apt-get -y upgrade

# install dependencies from apt
sudo apt install -y bluez-tools bluez-hcidump libbluetooth-dev \
                    git gcc python3-pip python3-setuptools \
                    python3-pydbus

# install pybluez from source
git clone https://github.com/pybluez/pybluez.git
cd pybluez
sudo python3 setup.py install

# build bdaddr from the bluez source
cd ~/
git clone --depth=1 https://github.com/bluez/bluez.git
gcc -o bdaddr ~/bluez/tools/bdaddr.c ~/bluez/src/oui.c -I ~/bluez -lbluetooth
sudo cp bdaddr /usr/local/bin/
```

## Running BlueDucky
```bash
git clone https://github.com/pentestfunctions/BlueDucky.git

or

git clone https://github.com/palacita135/BlueDucky.git

cd BlueDucky
pip install -r requirements.txt <if failed use "--break-system-packages">
sudo hciconfig hci0 up
python3 BlueDucky.py
```

## Operational Steps 🕹️
1. On running, it prompts for the target MAC address.
2. Pressing nothing triggers an automatic scan for devices.
3. Devices previously found are stored in known_devices.txt.
4. If known_devices.txt exists, it checks this file before scanning.
5. Executes using payload.txt file.
6. Successful execution will result in automatic connection and script running.

## Duckyscript 💻
🚧 Work in Progress:
- Suggest me ideas


#### 📝 Example payload.txt:
```bash
REM Title of the payload
STRING ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890!@#$%^&*()_-=+\|[{]};:'",<.>/?
GUI D
```

```bash
REM Opens a private browser to hackertyper.net
DELAY 200
ESCAPE
GUI d
ALT ESCAPE
GUI b
DELAY 700
REM PRIVATE_BROWSER is equal to CTRL + SHIFT + N
PRIVATE_BROWSER
DELAY 700
CTRL l
DELAY 300
STRING hackertyper.net
DELAY 300
ENTER
DELAY 300


If there got any problems:
move the menu_functions, magic_keyboard_hid and registered_device frome utils folder to the main folder.
open menu_functions.py line 7 and 8 delete utils.

n general you can check with journalctl in Terminal for Errors and get a hint what to look for. Difficult to say.
Try running these in terminal to see if it will wake your bluetooth up:
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
