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
