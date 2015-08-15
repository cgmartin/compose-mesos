# compose-mesos
A [Docker Compose][] sandbox environment with [Marathon][], [Chronos][], and a [Mesos][] cluster consisting of one master and three slaves. Thanks to [dontrebootme](https://github.com/dontrebootme) for the base repo.

## Usage

`docker-compose up`

- Marathon URL: http://DOCKER_HOST:8080
- Mesos URL: http://DOCKER_HOST:5050
- Chronos URL: http://DOCKER_HOST:4400

[docker compose]: https://github.com/docker/compose
[marathon]: https://mesosphere.github.io/marathon/
[chronos]: http://mesos.github.io/chronos/
[mesos]: http://mesos.apache.org/
