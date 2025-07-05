# External Temperature Sensor Configuration for Xiaomi Home Climate Entities

This feature allows you to configure external temperature sensors for Xiaomi Home climate entities. This is useful when the built-in temperature sensor of the climate device is not accurate or not positioned optimally.

## Configuration

To configure external temperature sensors for your climate entities:

1. Go to **Settings** > **Devices & services** > **Xiaomi Home** > **CONFIGURE**
2. In the configuration options, find the **External temperature sensors** field
3. Enter the configuration in the following format:

```
climate_entity_id:sensor_entity_id,climate_entity_id2:sensor_entity_id2
```

### Example

To configure `sensor.temperature` as the external temperature sensor for `climate.lumi_cn_749560338_mcn02`:

```
climate.lumi_cn_749560338_mcn02:sensor.temperature
```

For multiple climate entities:

```
climate.lumi_cn_749560338_mcn02:sensor.temperature,climate.air_conditioner:sensor.room_temp
```

## How It Works

When an external temperature sensor is configured:

1. The climate entity will use the external sensor's temperature reading for the `current_temperature` property
2. If the external sensor is unavailable or has an invalid value, the climate entity will fall back to its built-in temperature sensor
3. The climate entity will log when it starts using an external temperature sensor

## Supported Climate Entity Types

This feature is supported for all Xiaomi Home climate entity types:
- Air conditioners
- Heaters  
- PTC bath heaters
- Thermostats
- Electric blankets

## Requirements

- The external temperature sensor entity must exist in Home Assistant
- The sensor should provide temperature readings as numeric values
- The sensor state should not be 'unknown' or 'unavailable' for the external reading to be used

## Finding Your Climate Entity ID

To find the entity ID of your climate device:

1. Go to **Settings** > **Devices & services** > **Entities**
2. Search for your Xiaomi climate device
3. The entity ID will be shown in the format `climate.device_name`

## Troubleshooting

- Check the Home Assistant logs for any warnings about invalid external temperature values
- Ensure the external temperature sensor entity ID is correct and the sensor is available
- If the external sensor becomes unavailable, the climate entity will automatically fall back to the built-in sensor
