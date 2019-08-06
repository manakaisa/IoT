# IoT

## Hardware and Software Requirements
- CPU: Quad-core Processors
- RAM: 4GB
- HDD: 100GB
- LAN: 1GbE 
- OS: Ubuntu Server 18.04 LTS

## Thingsboard

### Installation
- Update Repository  
  `# sudo apt update`
- Install Java 8 (OpenJDK)  
`# sudo apt install openjdk-8-jdk`
- Install ThingsBoard  
```
# wget https://github.com/thingsboard/thingsboard/releases/download/v2.4/thingsboard-2.4.deb
# sudo dpkg -i thingsboard-2.4.deb
```
- Install PostgreSQL  
`# sudo apt install postgresql postgresql-contrib`
- Create User for PostgreSQL  
```
# sudo su - postgres
# psql
# \password
# \q
```
- Create Database for ThingsBoard  
```
# psql -U postgres -d postgres -h 127.0.0.1
# CREATE DATABASE thingsboard;
# \q
```
- Modify Configuration: /etc/thingsboard/conf/thingsboard.conf  
- Run Init Script  
`# sudo /usr/share/thingsboard/bin/install/install.sh`
- Start ThingsBoard Service  
`# sudo systemctrl restart thingsboard`

### Configurations
- /etc/thingsboard/conf/thingsboard.conf  
```
export DATABASE_ENTITIES_TYPE=sql
export DATABASE_TS_TYPE=sql
export SPRING_JPA_DATABASE_PLATFORM=org.hibernate.dialect.PostgreSQLDialect
export SPRING_DRIVER_CLASS_NAME=org.postgresql.Driver
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/thingsboard
export SPRING_DATASOURCE_USERNAME=postgres
export SPRING_DATASOURCE_PASSWORD=<password>
```

### Rule Chains
- Root Rule Chain
- Save Telemetry
- Save Metadata
- Save Workers
- Save Workgroup
- Reset Workers
- Reset Workgroup
- Report Workers
- Report Workgroup
- Scedule Reset 
- Scedule Report 
- Split Messages

### Metadata Specification
- Entity Type: Asset
- Name: $WORKGROUP_PREFIX + META
- Type: META
- Server Attributes: 
  - reportTime: HH:mm
  - resetTime: HH:mm
  - workers: ***auto generated
  - worker_prefix: *
  - workgroup: *

### Workgroup Specification
- Entity Type: Asset
- Name: META.workgroup
- Type: $WORKGROUP_PREFIX + WORKGROUP
- Server Attributes: 
  - name: *

### Workers Specification
- Entity Type: Asset
- Name: META.worker_prefix + worker_id
- Type: $WORKGROUP_PREFIX + WORKER
- Server Attributes: 
  - name: *

### Sensors Specification
- Entity Type: Device
- Name: *
- Type: $WORKGROUP_PREFIX + SENSOR
- Server Attributes: 
  - sensorType: ***depend on implementation
  
### Widgets
- Raw Table
- Entities Table (time series)
- Daily Report
- Daily Report (tabs)
- Entities Bar
- Telemetry Input

## Node-RED

### Installation
- Update Repository  
`# sudo apt update`
- Install Node.js  
```
# curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
# sudo apt install -y nodejs
```
- Install Node-RED  
`# sudo npm install -g --unsafe-perm node-red node-red-admin`
- Install Additional Flows  
```
# cd ~/.node-red
# npm install node-red-contrib-modbus
```
- Modify Configuration: ~/.node-red/settings.js  
- Create Node-RED Service  
  - Modify Configuration: /lib/systemd/system/node-red.service  
  - Enable Node-RED Service  
    ```
    # sudo systemctl daemon-reload
    # sudo systemctl enable node-red.service
    # sudo systemctl start node-red.service
    ```

### Generating Password
`# node-red-admin hash-pw`
	
### Configurations
- /lib/systemd/system/node-red.service  
```
[Unit]
Description=Node-RED
After=syslog.target network.target
Documentation=https://nodered.org

[Service]
#Environment="NODE_RED_OPTIONS=-v"
ExecStart=/usr/bin/env node-red $NODE_RED_OPTIONS
WorkingDirectory=/home/<user>
User=<user>
Group=<user>
Nice=5
SyslogIdentifier=Node-RED
StandardOutput=syslog
Restart=on-failure
KillSignal=SIGINT

[Install]
WantedBy=multi-user.target
```
- ~/.node-red/settings.js  
```
module.exports = {
  ...
  httpAdminRoot: '/admin',
  httpStatic: '~/.node-red/public',
  adminAuth: {
    type: "credentials",
    users: [{
      username: "admin",
      password: "<password>",
      permissions: "*"
    }]
  },
  contextStorage: {
    default: { module: "memory" },
    file: { module: "localfilesystem" }
  },
  logging: {
    console: {
      level: "info",
      metrics: false,
      audit: true
    }
  }
}
```

### Assets
- datatables/
  - css/
	- jquery.datatables-1.10.18.min.css
    - buttons-1.5.6.min.css
  - images/
    - sort_asc.png
    - sort_asc_disabled.png
    - sort_desc.png
    - sort_desc_disabled.png
    - sort_both.png
  - js/
	- jquery.datatables-1.10.18.min.js
    - buttons-1.5.6.min.js
    - buttons.html5-1.5.6.min.js
- chartjs-2.8.0.min.js
- jszip-2.5.0.min.js