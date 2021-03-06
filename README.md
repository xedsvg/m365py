# m365py [![][img_license]](#license) [![][img_loc]][loc] [![Build status](https://ci.appveyor.com/api/projects/status/ylk3eiuu65t028kv?svg=true)](https://ci.appveyor.com/project/AntonHakansson/m365py)
[img_license]: https://img.shields.io/badge/License-MIT-blue.svg
[img_loc]: https://tokei.rs/b1/github/AntonHakansson/m365py
[loc]: https://github.com/Aaronepower/tokei

A Python2.7 and Python3.x library to receive parsed BLE Xiaomi M365 scooter messages using [bluepy](https://github.com/IanHarvey/bluepy).

This library is targeted for support for Xiaomi M365 firmware version `V1.3.8` - other versions may not work as intended, confirmed to not work on `V1.5.x`.

The protocol was derived with the help of the following resources:
 - [Camilo Ruiz (@CamiAlfa at github.com)](https://github.com/CamiAlfa/M365-BLE-PROTOCOL/blob/master/protocolo)
 - [smartinick](https://github.com/smartinick/esp32_xiaomi_m365/blob/9a9fcfdcd4f577b4aeaa050f5102ef0c53a24290/esp32_m365_oled/src/m365client.h#L381)

## Installation using pip
```sh
pip install git+https://github.com/AntonHakansson/m365py.git#egg=m365py
```

## Usage
See `examples` folder for full example of all supported requests.

```python
import time
import json

from m365py import m365py
from m365py import m365message


# callback for received messages from scooter
def handle_message(m365_peripheral, m365_message, value):
    print(json.dumps(value, indent=4))
    # Will print:
    # {
    #   "battery_percent": 84,
    #   "speed_kmh": 0.0,
    #   "speed_average_kmh": 0.0,
    #   "odometer_km": 155.819,
    #   "trip_distance_m": 0,
    #   "uptime_s": 159,
    #   "frame_temperature": 24.0
    # }

scooter_mac_address = 'XX:XX:XX:XX:XX:XX'
scooter = m365py.M365(scooter_mac_address, handle_message)
scooter.connect()

scooter.request(m365message.motor_info)
time.sleep(5)

scooter.disconnect()

```

## Find MAC address for scooter

This package includes the option to scan and list nearby M365 Scooters.
Simply excecute the package as such:

```sh
sudo python -m m365py
```

## Licence
```
MIT License

Copyright (c) 2019 Anton Håkansson

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
