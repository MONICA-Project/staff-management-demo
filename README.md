# staff-management-demo

Start the situation map: 

```bash
docker-compose up -d loramap
```

Go to http://localhost:8080 to see the location map for the event "Pützchens Markt" in Bonn.

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

Start the service to register the GPS fakers (see below) with SCRAL:

```bash
docker-compose up -d lorascral
```

Start up as many fakers as you like (up to 11). Please start them one after another to workaround an overloading issue that is still being analyzed:

```bash
docker-compose up -d gpsfaker1
docker-compose up -d gpsfaker2
docker-compose up -d gpsfaker3
docker-compose up -d gpsfaker4
docker-compose up -d gpsfaker5
docker-compose up -d gpsfaker6
docker-compose up -d gpsfaker7
docker-compose up -d gpsfaker8
docker-compose up -d gpsfaker9
docker-compose up -d gpsfaker10
docker-compose up -d gpsfaker11
```

The fakers should now be registered with SCRAL at http://localhost:8000/scral/v1.0/gps-tracker-gw/active-devices.

Observations (both localizations and - from time to time - alerts) should start coming in to GOST, visible in the dashboard at http://localhost:8090/#/observations or directly via the API at http://localhost:8090/v1.0/Observations.

On the location map you will now see some moving fake staffers in the event area. The panic buttons will be pressed from time to time and you will see these as alerts, visualized as red rectangles around the icons.
