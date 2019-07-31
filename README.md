# IoT

## Thingsboard

### Rule Chain
- Root Rule Chain
- Save Telemetry
- Save Metadata
- Split Messages
- Save Workers
- Save Workgroup
- Reset Workers
- Reset Workgroup
- Report Workers
- Report Workgroup
- Scedule Reset 
- Scedule Report 

### Metadata Specification
- Entity Type: Asset
- Name: Metadata
- Type: META
- Server Attributes: 
  - reportTime: HH:mm
  - resetTime: HH:mm
  - workers: ***auto generated
  - worker_prefix: *
  - workgroup: *

### Workgroup Specification
- Entity Type: Asset
- Name: Metadata.workgroup
- Type: WORKGROUP
- Server Attributes: 
  - name: *

### Workers Specification
- Entity Type: Asset
- Name: Metadata.worker_prefix + worker_id
- Type: WORKER
- Server Attributes: 
  - name: *

### Sensors Specification
- Entity Type: Device
- Name: *
- Type: SENSOR
- Server Attributes: 
  - sensorType: ***depend on implementation
  
### Widgets
- Raw Table
- Entities Table (time series)
- Daily Report
- Daily Report (tabs)
- Entities Bar
- Telemetry Input