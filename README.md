# Deye
Home Assistant integrations

# DeyeInverter
Small utility to read data from DEYE Inverters through the Solarman Datalogger. Works with S/N 17*

Tests indicate that the full ModBus is available through the TCP connection. **Probably** this *can lead* to modifying Inverter Configuration via TCP calls.

Thanks to @fjcarretero https://github.com/fjcarretero for his incredible support on understanding the data from and to the datalogger.
Thanks to @xtheone https://github.com/XtheOne for his original V4 reader that was an inspiration (and I borrowed *some* code). And Also @tamato27 borrowed some code and modified for use on a deye inverter

Thanks to all. And I modified some code for x86/amd64 and also the std Pi setup

Copy the folder that you want to use on specified platform deye_logger_x86_64 for the intel based home assistant and the deye_logger_pi for the rasberry pi enviroment into your addons folder


# Configuration
Is intergrated into the addon installer, can configure inside Home assistant
Options
inverter_ip
192.168.x.x
inverter_port
8899
inverter_sn
173XXXXXXX

mqtt
[SERVERNAME]
mqtt_port
1883
mqtt_topic
/deye
mqtt_username
[MQTT_USERNAME]
mqtt_passwd
[MQTT_PASSWORD]


# Run

```python3 InverterData.py

{"Running Status()":2,
"Total Grid Produciton(kwh)":829.5,
"Total Grid Produciton(kwh)":0.0,
"Daily Energy Bought(kwh)":0.0,
"Daily Energy Sold(kwh)":15.0,
"Total Energy Bought(kwh)":21.900000000000002,
"Total Energy Bought(kwh)":0.0,
"Total Energy Sold(kwh)":1103.4,
"Total Energy Sold(kwh)":0.0,
"Daily Load Consumption(KWH)":1.2000000000000002,
"Total Load Consumption(KWH)":365.3,
"Total Load Consumption(KWH)":0.0,
"DC Temperature(℃)":149.5,
"AC Temperature(℃)":152.1,
"Total Production(KWH)":1517.4,
"Total Production(KWH)":0.0,
"Alert()":0,
"Alert()":0,
"Alert()":0,
"Alert()":0,
"Alert()":0,
"Alert()":0,
"Daily Production(KWH)":16.900000000000002,
"PV1 Voltage(V)":342.20000000000005,
"PV1 Current(A)":7.800000000000001,
"PV2 Voltage(V)":8.5,
"PV2 Current(A)":0.0,
"Grid Voltage L1(V)":241.3,
"Grid Voltage L2(V)":0.0,
"Load Voltage(V)":242.9,
"Current L1(A)":10.51,
"Current L2(A)":0.0,
"Micro-inverter Power(W)":0,
"Gen-connected Status()":0,
"Gen Power(W)":0,
"Internal CT L1 Power(W)":-2325,
"Internal CT L2  Power(W)":0,
"Grid Status()":-2365,
"Total Gird Power(W)":-2365,
"External CT L1 Power(W)":-2365,
"External CT L2 Power(W)":0,
"Inverter L1 Power(W)":2558,
"Inverter L2 Power(W)":0,
"Total Power(W)":2558,
"Load L1 Power(W)":193,
"Load L2 Power(W)":0,
"Total Load Power(W)":193,
"Battery Temperature(℃)":125.0,
"Battery Voltage(V)":10.14,
"Battery SOC(%)":0,
"PV1 Power(W)":2619,
"PV2 Power(W)":0,
"Battery Status()":0,
"Battery Power(W)":0,
"Battery Current(A)":-0.01,
"Grid-connected Status()":1,
"SmartLoad Enable Status()":16}
```

```
# Add the below to your sensor setup

# Deye Sensors. ############################################################
  - platform: mqtt
    name: "Solarpower"
    state_topic: "/deye"
    unit_of_measurement: "W"
    json_attributes_topic: "/deye/attributes"
    device_class: power
    state_class: measurement

  - platform: mqtt
    name: "Solar - Daily Energy Bought KWH"
    state_topic: "/deye/attributes"
    unit_of_measurement: "kWh"
    value_template: "{{ value_json.daily_energy_bought_kwh }}"
    device_class: energy
    state_class: total_increasing

  - platform: mqtt
    name: "Solar - Daily Energy Sold KWH"
    state_topic: "/deye/attributes"
    unit_of_measurement: "kWh"
    value_template: "{{ value_json.daily_energy_sold_kwh }}"
    device_class: energy
    state_class: total_increasing

  - platform: mqtt
    name: "Solar - Daily Production KWH"
    state_topic: "/deye/attributes"
    unit_of_measurement: "kWh"
    value_template: "{{ value_json.daily_production_KWH }}"
    device_class: energy
    state_class: total_increasing

  - platform: mqtt
    name: "Solar - Battery"
    state_topic: "/deye/attributes"
    unit_of_measurement: "%"
    value_template: "{{ value_json.battery_soc_perc }}"
    device_class: energy
    state_class: measurement

  - platform: mqtt
    name: "Solar - Battery Temperature"
    state_topic: "/deye/attributes"
    unit_of_measurement: "°C"
    value_template: "{{ value_json.battery_temperature_C }}"
    device_class: energy
    state_class: measurement

  - platform: mqtt
    name: "Solar - Grid Status"
    state_topic: "/deye/attributes"
    unit_of_measurement: " "
    value_template: "{{ value_json.grid_connected_status_ }}"
    device_class: energy
    state_class: measurement

  - platform: mqtt
    name: "Solar - Grid Usage"
    state_topic: "/deye/attributes"
    unit_of_measurement: ""
    value_template: "{{ value_json.total_grid_power_W }}"
    device_class: energy
    state_class: measurement

  ### End of Deye Sensors. #####################################################
```


# Known Issues

The inverter is not fast enough to answer, you can get timeouts if you query it too often.

# Contrib

Python is not my strongest suite, feel free to suggest, rewrite or add whatever you feel is necessary.

# Home Assistant support

The folder deye_logger contains a Homeassistant add-on. Reference the README.md file in that folder more information.
