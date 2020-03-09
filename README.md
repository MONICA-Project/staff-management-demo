# staff-management-demo MONICA example

## Overview

This repository is a working docker compose example of whole MONICA toolchain, from data simulation to visualization. The overall use case for this is staff mangagement using hardware [trackers](https://github.com/MONICA-Project/LoTrack). Main components are the following:
- Common Operational Picture
    - [COP UI](https://github.com/MONICA-Project/COP-UI) - The generic MONICA COP user interface
    - [COP APi](https://github.com/MONICA-Project/COP.API) - The MONICA COP API
    - [COP Updater](https://github.com/MONICA-Project/COPUpdater) - Is the link between the COP and the IoT Devices, pushing individual updates to the COP.
    - [COP DB](https://github.com/MONICA-Project/COP.DB) - The MONICA COP DB used for storing the state of the COP.
- [SCRAL](https://github.com/MONICA-Project/scral-framework) - Gateway for trackers to the MONICA system
- [LORA Map](https://github.com/MONICA-Project/lora-map) - Special COP developed for the pilots in Bonn, Germany using the german iconography
- [GOST](https://github.com/gost/server) -Provides the IoT database
- MQTT Broker ([Eclipse Mosquitto](https://mosquitto.org/))
- gpsfaker - Runs the emulation.

## How to get started
### Prerequisites needed
- Docker 
- Docker Compose

### Installation and execution
Clone or download and unzip this repository to a folder on your local computer. Open a command console window and change it to the newly created folder.
The first time this system is started it is best to do it stepwise to identify any problems of already occupied network ports by following this order (Used nwtwork ports are found at the end): 

#### 1, Start the situation map: 

```bash
docker-compose up -d loramap
```

Go to http://localhost:8080 to see the LORA Map for the event "Pützchens Markt" in Bonn.
![LORA Map](https://github.com/MONICA-Project/staff-management-demo/blob/master/img/FitCop.PNG "LORA Map") 

#### 2, Start up a GOST server:

```bash
docker-compose up -d gost-dashboard
```

The GOST backend Postgres database takes a few seconds to start up. Make sure that the GOST API is available at http://localhost:8090/v1.0.

![IoT Database](https://github.com/MONICA-Project/staff-management-demo/blob/master/img/IoT%20DB.PNG "GOST") 



#### 3, Start up SCRAL:

```bash
docker-compose up -d scral
```

Check if SCRAL is up and running at http://localhost:8000/scral/v1.0/gps-tracker-gw.

![SCRAL](https://github.com/MONICA-Project/staff-management-demo/blob/master/img/Scral.PNG "SCRAL") 

#### 4, Start the COP API
Start the service to launch the COP API:

```bash
docker-compose up -d copapi
```

Check if COP API is up and running at http://localhost:8800/

![COP API](https://github.com/MONICA-Project/staff-management-demo/blob/master/img/MONICA%20Cop%20API.PNG "COP API") 


#### 4, Start the COP UI
Start the service for the COP UI:

```bash
docker-compose up -d copui
```

Check if COP UI is up and running at http://localhost:8900/
*NB!* the userid is admin@monica-cop.com and the password PM2019!

![COP UI](https://github.com/MONICA-Project/staff-management-demo/blob/master/img/CNet%20Cop.PNG "COP UI") 


#### 5, Start the SCRAL
Start the service to register the GPS dummies (see below) with SCRAL:

```bash
docker-compose up -d lorascral
```
#### 6, Start the simulation dummies
Start up the GPS dummies:

```bash
docker-compose up -d gpsfaker
```

#### 7, Next start
If all went well the next time you can make 
```bash
docker-compose up -d
```
Which will bring up the whole environment immediately.


The dummies should now be registered with SCRAL at http://localhost:8000/scral/v1.0/gps-tracker-gw/active-devices.

Observations (both localizations and - from time to time - alerts) should start coming in to GOST, visible in the dashboard at http://localhost:8090/#/observations or directly via the API at http://localhost:8090/v1.0/Observations.

On the location map you will now see some moving fake staffers in the event area. The panic buttons will be pressed from time to time and you will see these as alerts, visualized as red rectangles around the icons.

### Trouble shooting
The most likely problem that is encountered is port conflicts. If a container cannot start because of conflicting ports, stop or move the conflicting ports process and restart. The next section contains the port used by the components.
If something stops working the best solution is to restart everything again.
### TCP Ports used

The following table show the list of services with opened ports (inside subnet and forward to external connections):

| Service Name | Type Port | External Port | Internal Subnet Port |
| --------------- | --------------- | --------------- | --------------- |
| copui | Service | 8900 | 8080 | 
| copapi | Service | 8800 | 80 | 
| copupdater | Service | - | - | 
| copdb | Service (PostgresSQL database)| 9998 | 5432| 
| mosquitto | Service | 1883 | 1883| 
| dashboard | GOST API Service | 8090 | 8080|
| gost-db | GOST Database | 5432 | 5432|
| loramap | Map Service | 8080 | 8080|
| lorascral | Service | 8000 | 8000|
| scral | Service | 8000 | 8000 | 
| gpsfaker | Service | - | - |



## Affiliation
![MONICA](https://github.com/MONICA-Project/template/raw/master/monica.png)  
This work is supported by the European Commission through the [MONICA H2020 PROJECT](https://www.monica-project.eu) under grant agreement No 732350.
