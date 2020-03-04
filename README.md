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
The first time this system is started it is best to do it stepwise to identify any problems of already occupied network ports by following this order: 

#### 1, Start the situation map: 

```bash
docker-compose up -d loramap
```

Go to http://localhost:8080 to see the location map for the event "Pützchens Markt" in Bonn.
![General MONICA Architecture](https://github.com/MONICA-Project/HLDFAD_SourceCode/blob/master/WP6BreakdownDiagram.png "General Monica Architecture") 

Start up a GOST server:

```bash
docker-compose up -d gost-dashboard
```

The GOST backend Postgres database takes a few seconds to start up. Make sure that the GOST API is available at http://localhost:8090/v1.0.

Start up SCRAL:

```bash
docker-compose up -d scral
```

Check if SCRAL is up and running at http://localhost:8000/scral/v1.0/gps-tracker-gw.

Start the service to register the GPS dummies (see below) with SCRAL:

```bash
docker-compose up -d lorascral
```

Start up the GPS dummies:

```bash
docker-compose up -d gpsfaker
```

The dummies should now be registered with SCRAL at http://localhost:8000/scral/v1.0/gps-tracker-gw/active-devices.

Observations (both localizations and - from time to time - alerts) should start coming in to GOST, visible in the dashboard at http://localhost:8090/#/observations or directly via the API at http://localhost:8090/v1.0/Observations.

On the location map you will now see some moving fake staffers in the event area. The panic buttons will be pressed from time to time and you will see these as alerts, visualized as red rectangles around the icons.


## Affiliation
![MONICA](https://github.com/MONICA-Project/template/raw/master/monica.png)  
This work is supported by the European Commission through the [MONICA H2020 PROJECT](https://www.monica-project.eu) under grant agreement No 732350.