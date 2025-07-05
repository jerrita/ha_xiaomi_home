# External Temperature Sensor Feature Implementation Summary

## Overview
Added support for external temperature sensors in Xiaomi Home climate entities. This allows users to configure external temperature sensors (like `sensor.temperature`) to override the built-in temperature readings of climate devices such as `climate.lumi_cn_749560338_mcn02`.

## Files Modified

### 1. `/custom_components/xiaomi_home/config_flow.py`
**Changes:**
- Added `external_temperature_sensors` configuration option to the options flow form
- Added configuration string parsing logic to handle format: "climate_entity_id:sensor_entity_id,..."
- Added help text in description_placeholders explaining the configuration format
- Added the new configuration field to the OptionsFlowHandler class
- Ensured the configuration is saved to entry data and triggers a reload when changed

### 2. `/custom_components/xiaomi_home/climate.py`
**Changes:**
- Added import for `State` from homeassistant.core and entity_registry
- Added `FeatureExternalTemperature` class (though not directly used, kept for future extensibility)
- Modified `async_setup_entry` to read external temperature sensor configuration and pass it to entities
- Updated all climate entity constructors to accept `external_temp_entity_id` parameter:
  - `AirConditioner`
  - `Heater` 
  - `PtcBathHeater`
  - `Thermostat`
  - `ElectricBlanket`
- Added `current_temperature` property overrides in all climate entities that:
  - Check for external temperature sensor configuration
  - Use external sensor value when available and valid
  - Validate temperature range (-50 to 100 Celsius)
  - Fall back to internal sensor when external sensor is unavailable
  - Log appropriate warnings for invalid values
- Added logging to indicate when external temperature sensors are configured

## Configuration Format
Users can configure external temperature sensors using this format in the integration options:
```
climate.lumi_cn_749560338_mcn02:sensor.temperature
```

For multiple devices:
```
climate.device1:sensor.temp1,climate.device2:sensor.temp2
```

## Key Features
1. **Automatic fallback**: If external sensor is unavailable, entities fall back to built-in sensors
2. **Validation**: Temperature values are validated to be within reasonable range (-50 to 100°C)
3. **Logging**: Clear logging when external sensors are configured or when issues occur
4. **Non-breaking**: Existing functionality is preserved, feature is opt-in
5. **Universal support**: Works with all climate entity types in the integration

## Usage Example
To configure `sensor.temperature` as the external temperature source for `climate.lumi_cn_749560338_mcn02`:

1. Go to Settings > Devices & services > Xiaomi Home > CONFIGURE
2. Find "External temperature sensors" field
3. Enter: `climate.lumi_cn_749560338_mcn02:sensor.temperature`
4. Save configuration
5. Home Assistant will reload the integration
6. The climate entity will now use the external sensor for temperature readings

## Testing
- Syntax validation confirmed for both modified files
- Configuration parsing logic tested with various input formats
- Fallback behavior implemented for invalid/unavailable external sensors

## Documentation
Created comprehensive README explaining the feature usage, configuration format, and troubleshooting steps.
