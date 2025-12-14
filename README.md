# USB Tablet/Phone Charger
Info and ESPHome code for my USB Tablet and Phone Charger project

# Watch the full build and coding tutorial here: https://youtu.be/aw0FqZZQ76g

```yaml
esphome:
  name: charger-tablet # Use this to name your device

esp8266:
  board: d1_mini

logger:
  logs:
    sensor: none
    light: none
api:
ota:
  platform: esphome

substitutions:
  device: Tablet

# Set your Wi-Fi Name and Password
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

switch:
  - platform: gpio
    name: ${device} Charger
    pin: D2
    id: mos
    icon: mdi:usb-port
    on_turn_on:
      then:
        light.turn_on:
          id: led
          effect: "pulse"
    on_turn_off:
      then:
        light.turn_off: led          


sensor:
  - platform: homeassistant
    entity_id: sensor.nexus_7_battery_level # Change this to your 'Battery Level' sensor from the APP
    id: battery
    internal: true
    filters:
      - heartbeat: 10s
    on_value:
      - if:
          condition:
              - lambda: "return id(battery).state < 70;" # '70' is the lower level, change if needed
          then:
            - switch.turn_on: mos
          else:
            - if:
                condition:
                  - lambda: "return id(battery).state > 80 ;" # '80' is the upper level, change if needed
                then:
                  - switch.turn_off: mos

light:
  - platform: monochromatic
    id: led
    output: ledout
    effects:
      - pulse:
          name: pulse
          transition_length: 1s
          update_interval: 1s          
output:
  - platform: esp8266_pwm
    id: ledout
    inverted: true
    pin: D4
```


## Here's a simple wiring diagram:

![unknown](https://user-images.githubusercontent.com/51385971/167420700-e1254c36-e0a4-4f23-8021-fdcc0e4412bb.png)

---

<div align="center">

### 💖 Support This Project

Found this useful? Want to say thanks and fuel future creations?

**Your support keeps the creativity flowing!** 🍺✨

<table>
  <tr>
    <td align="center">
      <a href="https://www.buymeacoffee.com/3ative">
        <img src="https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Support-yellow.svg?style=for-the-badge&logo=buy-me-a-coffee&logoColor=white" alt="Buy Me A Coffee"/>
        <br/>
        <b>☕ Buy me a Coffee</b>
      </a>
    </td>
    <td align="center">
      <a href="https://www.patreon.com/3ative">
        <img src="https://img.shields.io/badge/Patreon-Become%20a%20Patron-red.svg?style=for-the-badge&logo=patreon&logoColor=white" alt="Patreon"/>
        <br/>
        <b>💖 Join on Patreon</b>
      </a>
    </td>
  </tr>
</table>

**Every contribution helps bring more awesome projects to life!** 🚀

</div>

---
