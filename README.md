* ExoSense™ Data Schema

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
    },
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