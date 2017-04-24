### adonisv79's Grafana4 ###

This image is forked from [Kamon's repository on the Docker Hub](https://hub.docker.com/u/kamon/). It is a blueprint which is used to build a Docker image that contains a sensible default configuration of StatsD, Graphite and Grafana.

- OS Version: Ubuntu 14.04
- Grafana Version: Grafana 4.2.0

### Exposed Docker ports ###

- `80`: the Grafana web interface.
- `81`: the Graphite web port
- `2003`: the Graphite data port
- `8125`: the StatsD port.
- `8126`: the StatsD administrative port.

### Docker Data Volumes ###

The following paths will be shared by the Docker conatainers
- The Grafana Dashboard and Plugin changes: /opt/grafana/data
- The Graphite data: /opt/graphite/storage

### Using the Docker Index (Linux/Unix) ###

#### Linux

All you need as a prerequisite is having `docker`, `docker-compose`, and `make` installed on your machine. The container exposes the following ports:

To start a container with this image you just need to run the following command:

```bash
$ make up
```

To stop the container
```bash
$ make down
```

To run container's shell
```bash
$ make shell
```

To view the container log
```bash
$ make tail
```

If you already have services running on your host that are using any of these ports, you may wish to map the container
ports to whatever you want by changing left side number in the `--publish` parameters. You can omit ports you do not plan to use. Find more details about mapping ports in the Docker documentation on [Binding container ports to the host](https://docs.docker.com/engine/userguide/networking/default_network/binding/) and [Legacy container links](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/).

#### Windows

For windows (which does not have make command), you can just start the container using the default docker run command.
Note: replace ports like '80' with the port where grafana will be accesible to. In the example below we replaced the grafana port 80 with port '4400'

```bash

docker run -d -p 4400:80 -p 81:81 -p 2003:2003 -p 8125:8125/udp -p 8126:8126 --name grafana4 -v /opt/grafana/data -v /opt/graphite/storage adonisv79/grafana4

```


### Building the image yourself ###

The Dockerfile and supporting configuration files are available in our [Github repository](https://github.com/adonisv79/docker-grafana-graphite).
This comes specially handy if you want to change any of the StatsD, Graphite or Grafana settings, or simply if you want
to know how the image was built. The repo also has `build` and `start` scripts to make your workflow more pleasant.


### Using the Dashboards ###

#### Grafana
- open your browser pointing to http://localhost:80 (or another port if you changed it like '4400')
  - Docker with VirtualBox on macOS: use `docker-machine ip` instead of `localhost`
- login with the default username (admin) and password (admin)
- open existing dashboard (or create a new one) and select 'Local Graphite' datasource
- play with the dashboard at your wish...

#### Graphite
- open your browser pointing to http://localhost:81 (or another port if you also changed it)
  - Docker with VirtualBox on macOS: use `docker-machine ip` instead of `localhost`
- login with the default username (admin) and password (admin)
- play with the dashboard at your wish...

### Data Population ###
This image allows your apps to send time-series data using [edgy's StatsD](https://codeascraft.com/2011/02/15/measure-anything-measure-everything/).
- For Node developers we recommend using [node-statsd](https://github.com/sivy/node-statsd)
- The StatsD UDP port is set to 8125.

### Persisted Data ###

When running `make up`, directories are created on your host and mounted into the Docker container, allowing graphite and grafana to persist data and settings between runs of the container.


### Now go explore! ###

We hope that you have a lot of fun with this image and that it serves it's
purpose of making your life easier. This should give you an idea of how the dashboard looks like when receiving data
from one of our toy applications:

![Kamon Dashboard](http://kamon.io/assets/img/kamon-statsd-grafana.png)
![System Metrics Dashboard](http://kamon.io/assets/img/kamon-system-metrics.png)
