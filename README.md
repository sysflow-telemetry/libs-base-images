# Libs builder and runtime images

This repository packages two base images for creating [libs](https://github.com/falcosecurity/libs) consumers. These are intented to be used in multi-stage builds where one first builds the Libs consumer using the [libs](ghcr.io/sysflow-telemetry/libs/libs) builder image in an initial stage, and then copies the target executable in a second stage that is derived from the [runtime](ghcr.io/sysflow-telemetry/libs/runtime) image. This should result in very small images for release.

## Base images

| **Image** | **Description** | **Dockerfile** | **Environment** |
|---|---|---|---|
| ghcr.io/sysflow-telemetry/libs-base-images/libs | A base image containing the pre-installed Falco Libs and tools for building Libs consumers | [docker/libs](https://github.com/sysflow-telemetry/libs-base-images/blob/master/docker/libs/Dockerfile) | FALCOSECURITY_LIBS_CFLAGS<br>FALCOSECURITY_LIBS_LDFLAGS |
| ghcr.io/sysflow-telemetry/libs-base-images/runtime | A base image containing the Falco Libs driver loader, to be used to build Libs consumer release images | [docker/driver-loader](https://github.com/sysflow-telemetry/libs-base-images/blob/master/docker/driver-loader/Dockerfile) | |
| ghcr.io/sysflow-telemetry/libs-base-images/runtime-ubi | A base image containing the Falco Libs driver loader based on Red Hat UBI, to be used to build Libs consumer release images | [docker/driver-loader](https://github.com/sysflow-telemetry/libs-base-images/blob/master/docker/driver-loader/Dockerfile.ubi) | |


The libs builder image defines two built-in environment variables that can be used in build automation for Libs consumers (e.g., see this [Makefile](examples/cppscap/Makefile)):

* FALCOSECURITY_LIBS_CFLAGS: defines the CFLAGS for including the Libs headers

* FALCOSECURITY_LIBS_LDFLAGS: defines the LDFLAGS for linking the Libs libraries and dependencies

## Creating Libs consumer images

Using this SDK, you can easily create Docker images for your Libs consumer. Example dockerfiles are providede in the `examples` directory.

To build the docker images for the examples, run:

```bash
docker build \
    --build-arg FALCOSECURITY_LIBS_VERSION=$(FALCOSECURITY_LIBS_VERSION) \
    --build-arg FALCOSECURITY_DRIVER_VERSION=$(FALCOSECURITY_DRIVER_VERSION) \
    -f examples/cppscap/Dockerfile -t cppscap .
```

To perform a live capture with the example consumer, run:

```bash
./examples/cppscap/run.sh
```

