
# Data Science Toolbox with Docker

## Spark

The spark docker container is based on `ubuntu:15.04` image and forked script from [https://github.com/gettyimages/docker-spark]().

The current setting is:

1. Spark 1.5.2
2. Hadoop 2.6.2

Change the `ENV` on top of the `Dockerfile` to rebuild with other version.

**single local mode**

Launch the docker container with current working directory mouted as `/data` by entrypoint

`spark-shell` by 

```sh
docker run --rm -it -p 4040:4040 -v $(pwd):/data rickyking/spark bin/spark-shell
```

Or `pyspark` by

```sh
docker run --rm -it -p 4040:4040 -v $(pwd):/data rickyking/spark bin/pyspark
```

**spark standalone cluster mode**

This requires `docker-compose`, the prepared `yaml` file is in `spark\docker-compose.yml`. Launch by `docker-compose up`.

