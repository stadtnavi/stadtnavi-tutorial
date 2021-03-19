# stadtnavi Tutorial - Running OPT and DT jointly


## Combining all containers

In this tutorial step, we combine the customized Digitransit from step 1 and the custom configured OpenTripPlanner from step 2.

At first, we try again to run the tutorial step unchanged.

digitransit-proxy publishes the digitransit UI under <domainname> and the OpenTripPlanner and map services using the subdomain api.<domainname>.

Both domains should refer to the same IP adress. For development purposes, we add a mapping for api.localhost to the local dns config in /etc/hosts:

```sh
echo "127.0.0.1 api.localhost" | sudo tee -a /etc/hosts
```

This tutorial step will start the services highlighted in the following diagram:

Start all containers via:

```sh
$ docker-compose up -d
```

## Customizing the setup
Now again copy the `.env` file to e.g. `.env.mycity` and adapt ROUTER, GTFS_URL and OSM_URL to reflect the digitransit router config you created in step 1 and the data files you chose for step 2 already.

```sh
$ docker-compose --env-file=.env.mycity up --build -d
```

After successful start, you should be able to acccess digitransit via http://localhost:8080/

