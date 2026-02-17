# ESPHome based sensor gateway

A sensor controller connected via PoE Ethernet using [ESPHome](https://esphome.io/) for connecting a variety of sensors to [Home Assistant](https://www.home-assistant.io/).

## Hardware

The gateway is based on the [ESP32-S3-POE-ETH-8DI-8DO](https://www.waveshare.com/esp32-s3-poe-eth-8di-8do.htm) device from Waveshare, which is a fully featured ESP32-S3 based system including 8 digital inputs & outputs, CAN, RS-485, 2.4GHz WiFi/Bluetooth LE and Ethernet with IEEE 802.3af compliant PoE. It has a case with DIN rail mounting capability.

![Waveshare ESP32-S3-POE-ETH-8DI-8DO device](/images/waveshare-device.jpg)

## Firmware

The system runs ESPHome.

A sample `secrets.yaml` file is included which will need populating before compilation.

The firmware can then be compiles & uploaded using the ESPHome dashboard or via the CLI:

```
esphome compile sensor-gateway.yaml
esphome upload sensor-gateway.yaml
```

### Core

The `core.yaml` configures the basic system, including the ESP32 settings, PSRAM and Ethernet controller. The Home Assitant API connection is encrypted and the OTA update capability is password protected, in line with the [Security Best Practices](https://esphome.io/guides/security_best_practices/) guide.

Additionally it sets up I2C plus the I2C expander and configures the RTC (Real Time Clock). This is set to update automatically each time there is a connection to the Home Assistant API.

Finally two entities are exposed to Home Assistant, a button to control the buzzer & a light controlling the RGB LED. The buzzer plays RTTTL tones, with the button set to play the `long` sample found in the [Common Beeps](https://esphome.io/components/rtttl/#common-beeps) section of the RTTTL Buzzer documentation.

![Core entities in Home Assistant](/images/ha-core.png)

#### Configuration

There are two configuration options found in the `substitutions` section of `sensor-gateway.yaml`:

| Substitution Key | Description              |
| ---------------- | ------------------------ |
| `name`           | ID for device            |
| `friendly_name`  | Friendly name for device |

#### Secrets

There are two secrets required for the core functionality:

| Secret Name          | Description |
| -------------------- | ------------- |
| `ota_password`       | Password for OTA updates |
| `api_encryption_key` | Encryption key for Home Assistant API communication. Can be generated using the tool on the [Native API Component](https://esphome.io/components/api/#configuration-variables) documentation page |
