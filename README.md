# ExoSense™ Data Payload Define

### Reference:
1. https://github.com/exosite/industrial_iot_schema/blob/master/data-types.md
2. https://github.com/exosite/industrial_iot_schema/blob/master/channel-signal_io_schema.md

## Channel Configuration Definition Description
```
# config_io channel definition 
last_edited: "${date_timestamp}" # *required* -  e.g. 2018-03-28T13:27:39+00:00 
last_editor" : "${edited_by}" # optional but recommended - String to mark as last editor, Person user name, "user", or "device"
meta : "${meta_string_information}" # optional - This is an open section for manufacturers to include useful meta info about the channels if they see fit
locked: ${locked_config_state} # optional - Boolean, marks config as not editable by UI, assume false if not present
channels: # "device channel" as opposed to an "asset signal"
  ######### Example channel config 1 - Basic Usage Options ############
  ${device_channel_id1}: # unique channel identifier string
    display_name: "${channel_name_string}" # *required* - Human readable channel name
    description: "${channel_description_string}" # optional - One-liner description 
    properties: 
      data_type: "${defined_type_enum}"  # *required* - See "types" section - in this case it could be "BOOLEAN" or "TEMPERATURE"
      primitive_type: "${defined_primitive_type_enum}" # optional - See "types" section - in this case it would be "BOOLEAN" or "NUMERIC"
      data_unit: "${unit_enum}" # *required* - Enumerated lookup to unit types for the given type
      precision: ${precision_number_of decimal_places} # optional but recommended, example value is 2 
      locked: ${locked_config_state} # optional but recommended - Boolean, marks as not editable by UI, defaults to false if not present
    protocol_config" : # optional - if used by device client
      sample_rate : "${sample_time_in_ms}" # optional - device client's sample rate for sensor
      report_rate : "${report_time_in_ms}" # optional but recommended - rate at which data sent to platform
      timeout : "${timeout_period_time_in_ms}" # optional but recommended - used by application to provide timeout indication, typically several times expected report rate
  ######### Example channel config 2 - Expanded Options - Remote Device Configuration Usage ############
  ${device_channel_id2}:   # unique channel identifier string
    display_name: "${channel_name_string}" # *required* - Human readable channel name
    description: "${channel_description_string}" # optional - One-liner description 
    properties: 
      data_type: "${defined_type_enum}" # *required* - See "types" section - in this case it would be "TEMPERATURE"
      primitive_type: "${defined_primitive_type_enum}" # optional - See "types" section - in this case it would be "NUMERIC"
      data_unit: "${unit_enum}" # *required* - Enumerated lookup to unit types for the given type
      precision: ${precision_number_of decimal_places}  # optional but recommended
      locked: ${locked_config_state} # optional but recommended - Boolean, marks as not editable by UI, defaults to false if not present
      ## Additional Channel Properties
      min: ${channel_min_number}  # optional - channel expected value min, applies to numberic type only
      max: ${channel_max_number}  # optional - channel expected value max, applies to numberic type only
      device_diagnostic: false # optional - Tells ExoSense that this is a “meta” signal that describes an attribute of the devices health
    protocol_config :  # optional - if used by device client 
      sample_rate : ${sample_time_in_ms} # optional - device client's sample rate for sensor
      report_rate : ${report_time_in_ms} # optional but recommended - rate at which data sent to platform
      timeout : "${timeout_period_time_in_ms}" # optional but recommended - used by application to provide timeout indication, typically several times expected report rate
      ## Addtional Channel Protocol Configuration Properties
      report_on_change : "${true|false}" # optional - default false (always report on start-up)
      down_sample : "${MIN|MAX|AVG|ACT}" # optional - Minimum in window, Maximum in window, running average in window, or actual value (assume report rate = sample rate)
      application : "${fieldbus_logger_application_name}" # optional - e.g. "Modbus_RTU"
      interface : "${path_to_interface}" # optional but required if using "application" e.g. "/dev/tty0/"
      app_specific_config : # optional but maybe be required depending on "application"
        ${app_specific_config_item1} : "${app_config_item1_value}"
        ${app_specific_config_item2} : "${config_item2_value}"
      ## Addtional Channel Protocol properties to transform raw data to the channel type / unit before reporting
      input_raw : # optional, used to pre-transform data type/unit from raw sensor to channel type, example voltage or bits (ADC) to Pressure 
        max : ${raw_input_max} # optional - above this puts the channel in error
        min : ${raw_input_max} # optional - above this puts the channel in error
        unit : "${raw_input_units}" # optional - e.g. "mA", reference only
      multiplier : ${number_to_be_multiplied_into_the_raw_value}" # optional used to pre-transform data type/unit from raw sensor to channel type, example 4-20 mA to Temperature 
      offset : ${offset} # optional used to pre-transform data type/unit from raw sensor to channel type, example 4-20 mA to Temperature 
    ## Advanced Usage / Not Available by Default - Used on Platform side only (Not Device)
    iot_properties: ## Advanced use only / for use by server side only (not device) - Not available by default
      multiplier: ${number_to_be_multiplied_into_the_raw_value}" # If not present set to 1
      offset: ${offset} # if not present assume 0
      data_type: "${defined_type_name}" # See "types" section - in this case it would be "TEMPERATURE"
      primitive_type: "${defined_primitive_type_name}" # Optional, See "types" section - in this case it would be "NUMERIC"
      data_unit: "${unit_enum}" # Enumerated lookup to unit types for the given type
      conversion_name: "${name}" # Name of conversion use to fill the multiplier and offset fields.
      min: ${converted_channel_min_number}  # optional - channel expected min after conversion
      max: ${converted_channel_max_number}  # optional - channel expected max after conversion
```

## Data Types
### Example JSON Channel
* JSON (unit-less)
    * Key (data_type): JSON
    * Accepted Units (data_unit): Not Used
    * Primitive Type (primitive_type): JSON
    * UI Unit Abbreviation: na
    * Notes: Any JSON blob
```
{
    "channels": {
        "Front_Vibration": {
            "display_name": "Front_Vibration",
            "description": "",
            "properties": {
                "data_type": "JSON",
                "primitive_type": "JSON"
            },
            "protocol_config": {
                "report_rate": 60000,
                "timeout": 360000
            }
        },
        "Rear_Vibration": {
            "display_name": "Rear_Vibration",
            "description": "",
            "properties": {
                "data_type": "JSON",
                "primitive_type": "JSON"
            },
            "protocol_config": {
                "report_rate": 60000,
                "timeout": 360000
            }
        }
    }
}
```

### Example JSON Channel Data (data_in) Packet
```
{
    "Front_Vibration": {
        "freq":[1,2,..],
        "mag_x":[0.01,0.02,...],
        "mag_y":{[0.01,0.02,...]
    }
}
```

---

### Example Number Channel
* Temperature
    * Key (data_type): TEMPERATURE
    * Primitive Type (primitive_type): NUMERIC
    * Accepted Units (data_unit) with UI unit abbreviation:

* Battery Percentage
    * Key (data_type): BATTERY_PERCENTAGE
    * Primitive Type (primitive_type): NUMERIC
    * Accepted Units (data_unit) with UI unit abbreviation:
        * `PERCENT: %`
    * Notes: Device diagnostic

```
{
    "channels": {
        "Battery_Percentage": {
            "display_name": "Battery_Percentage",
            "description": "",
            "properties": {
                "data_type": "BATTERY_PERCENTAGE",
                "primitive_type": "NUMERIC",
                "data_unit": "PERCENT",
            },
            "protocol_config": {
                "sample_rate": 10,
                "report_rate": 5000,
                "timeout": 60000,
                "precision": 1,
                "min": 0,
                "max": 100,
                "locked": true
            }
        },
        "Temperature": {
            "display_name": "Temperature",
            "description": "",
            "properties": {
                "data_type": "TEMPERATURE",
                "primitive_type": "NUMERIC",
                "data_unit": "DEG_CELSIUS",
            },
            "protocol_config": {
                "sample_rate": 10,
                "report_rate": 5000,
                "timeout": 60000,
                "precision": 1,
                "min": 0,
                "max": 100,
                "device_diagnostic": false,
                "locked": true
            }
        }
    }
}
```

### Example Number Channel Data (data_in) Packet
```
{
  "Battery_Percentage": 95,
  "Temperature": 40
}
```

