# DXP Docker Container Basics

Docker Hub hosts [Liferay DXP](https://hub.docker.com/r/liferay/dxp) and [Liferay Portal Community Edition (CE)](https://hub.docker.com/r/liferay/portal) Docker images, bundled with Tomcat on Linux. The Liferay Docker Hub pages provide image details and tags for the different releases.

* [Liferay DXP Images](https://hub.docker.com/r/liferay/dxp)
* [Liferay Portal CE Images](https://hub.docker.com/r/liferay/portal)

Here are the fundamentals for using Liferay Docker containers:

* [Starting a DXP Container for the First Time](#starting-a-dxp-container-for-the-first-time)
* [Viewing Log Files](#viewing-log-files)
* [Stopping DXP](#stopping-dxp)
* [Restarting DXP](#restarting-dxp)

DXP containers are standard Docker containers that can be started and stopped as such. The following examples use [Docker CLI (`docker`)](https://docs.docker.com/engine/reference/commandline/docker/), but you can use whatever Docker container tools you like.

## Starting a DXP Container for the First Time

The DXP container listens on port `8080` and starts like all Docker containers.

1. [Run a container](https://docs.docker.com/engine/reference/commandline/run/) that maps a host port (e.g., `8080`) to the container's `8080` port.

    ```bash
    docker run -it --name [some name] -p 8080:8080 liferay/portal:7.3.2-ga3
    ```

    ```note::
       Naming your container is optional but can facilitate managing the container.
    ```

    The DXP container runs and prints log messages, including this startup completion message:

    ```
    INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [xx,xxx] milliseconds
    ```

1. Open the DXP UI in your browser at `https://localhost:8080`.

    ![The Liferay DXP initial landing page.](./dxp-docker-container-basics/images/01.png)

DXP is ready to use.

## Viewing Logs

DXP log messages and log files are accessible.

```tip::
   The ``[container]``` is the name you entered via ``--name [some name]`` in your ``run`` command.
```

### `docker logs` commands

The [`docker logs`](https://docs.docker.com/engine/reference/commandline/logs/) command prints container log messages.

| Command | Result |
| :------ | :----- |
| `docker logs [container]` | Outputs all of the current log messages |
| `docker logs -f [container]` | Streams new log messages, like `tail -f [file]` does |
| `docker logs -t [container]` | Appends a timestamp to each log message |

### `docker cp` command

You can use a [`docker cp`](https://docs.docker.com/engine/reference/commandline/cp/) command like the one below to copy a DXP log file to your host machine.

```bash
docker cp [container]:/opt/liferay/logs/liferay.[timestamp].log .
```

## Stopping DXP

Here is the most graceful way to stop the DXP container:

```bash
docker exec [container] /opt/liferay/tomcat/bin/shutdown.sh
```

Here are the fastest ways to stop the container:

* `Ctrl-C`: This sends a [`SIGINT` or `SIGKILL` signal to an attached container](https://docs.docker.com/engine/reference/commandline/attach/#extended-description) (a container started with the `-it` argument).
* `docker kill [container]`: This sends a `KILL` signal to the container.

## Restarting DXP

DXP containers can be restarted like all Docker containers.

```bash
docker start [container]
```

Now you know the basics of starting, stopping, and monitoring a DXP container.

## What's Next

If you want to know what the DXP container entry point does and learn the container's API, see the [DXP Container Lifecycle and API](./dxp-container-lifecycle-and-api.md). If you want to start using DXP containers, exercise one of the following use cases:

* [Configuring DXP Containers](./configuring-dxp-containers.md)
* [Installing Apps and Other Artifacts to Containers](./installing-apps-and-other-artifacts-to-containers.md)
* [Patching DXP in Docker](./patching-dxp-in-docker.md)
* [Providing Files to the Container](./providing-files-to-the-container.md)