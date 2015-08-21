# compose-mesos
A [Docker Compose][] sandbox environment with [Marathon][], [Chronos][], [Consul][], [HAProxy][], 
[Registrator][], and a [Mesos][] cluster consisting of one master and three slaves. 

Thanks to [dontrebootme](https://spof.io/blog/2015/06/23/mesos-sandbox-using-docker-compose/) for 
the base repo and excellent blog post.

## Usage

`docker-compose up`

Then access the various components via the correct port:

- Marathon URL: http://DOCKER_HOST:8080
- Mesos URL: http://DOCKER_HOST:5050
- Chronos URL: http://DOCKER_HOST:4400
- Consul URL: http://DOCKER_HOST:8500

## Service Orchestration

Application services can be deployed using the [Marathon][] API:

```bash
# Starts up 10 microbot apps in docker containers... See microbot.json file
$ curl -X POST -H "Content-Type: application/json" http://DOCKER_HOST:8080/v2/apps -d@microbot.json
``` 

As the containers start, [Registrator][] receives the event, notifies [Consul][], which updates the
[HAProxy][] load balancer configuration (via [Consul-Template][]).

You can then reach a microbot app through the load balancer, via:

```
http://microbot.docker.local
``` 

...as long as you've configured a wildcard DNS `*.docker.local` to point to the DOCKER_HOST IP
(or just add it to your `/etc/hosts` file):

```
# Use your DOCKER_HOST IP here
192.168.99.100   microbot.docker.local
```

Other applications can be started in the same way, automatically load balancing based off of the 
DNS name `{app}.docker.local` and the [Marathon][] app ID. See [haproxy-consul][] for more information.

[docker compose]: https://github.com/docker/compose
[marathon]: https://mesosphere.github.io/marathon/
[chronos]: http://mesos.github.io/chronos/
[mesos]: http://mesos.apache.org/
[consul]: https://www.consul.io/
[registrator]: http://gliderlabs.com/registrator/latest/
[haproxy]: http://www.haproxy.org/
[haproxy-consul]: https://github.com/CiscoCloud/haproxy-consul
