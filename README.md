# apache-hop-minimal-project

Minimal Apache Hop project to use for various test, e.g. with containers. This project has no extra dependencies.

- project name: apache-hop-minimum-project
- environments: dev
- pipeline run config: local
- workflow run config: local
- workflow: main.hwf
- pipeline: main.hpl

In some situations you might want to register this project with Hop on your env:

```shell
${HOP_INSTALL_DIR}="/path/to/hop/install/dir"
${HOP_PROJECT_HOME}="/path/to/hop/project/dir"
${HOP_ENV_CONFIG}="${HOP_PROJECT_HOME}/config-dev.json"

cd ${HOP_INSTALL_DIR}
# Create Hop project
./hop-conf.sh \
--project=apache-hop-minimum-project \
--project-create \
--project-home="${HOP_PROJECT_HOME}" \
--project-config-file=project-config.json \
--project-metadata-base='${PROJECT_HOME}/metadata' \
--project-datasets-base='${PROJECT_HOME}/datasets' \
--project-unit-tests-base='${PROJECT_HOME}' \
--project-enforce-execution=true
```

Let’s define a Hop **environment**:

```shell
# Create Hop environment
./hop-conf.sh \
--environment=dev \
--environment-create \
--environment-project=apache-hop-minimum-project \
--environment-purpose=development \
--environment-config-files="${HOP_ENV_CONFIG}"
```

Example of running this project with the official **Docker image**:

```shell
HOP_DOCKER_IMAGE=apache/incubator-hop:0.70-SNAPSHOT

HOP_DOCKER_IMAGE=apache-hop:20210201221224
PATH_TO_LOCAL_PROJECT_DIR=/Users/diethardsteiner/git/apache-hop-minimal-project

docker run -it --rm \
  --env HOP_LOG_LEVEL=Basic \
  --env HOP_FILE_PATH='${PROJECT_HOME}/main.hwf' \
  --env HOP_PROJECT_DIRECTORY=/files \
  --env HOP_PROJECT_NAME=apache-hop-minimum-project \
  --env HOP_ENVIRONMENT_NAME=dev \
  --env HOP_ENVIRONMENT_CONFIG_FILE_NAME_PATHS=/files/dev-config.json \
  --env HOP_RUN_CONFIG=local \
  -v ${PATH_TO_LOCAL_PROJECT_DIR}:/files \
  --name my-simple-hop-container \
  ${HOP_DOCKER_IMAGE}
  
  
#echo "2020-12-25,23030" > /tmp/sales.csv
#echo "2020-12-26,24089" >> /tmp/sales.csv
#echo "2020-12-27,29231" >> /tmp/sales.csv

docker run -it --rm \
  --env HOP_LOG_LEVEL=Basic \
  --env HOP_FILE_PATH='${PROJECT_HOME}/read-from-csv-file.hpl' \
  --env HOP_PROJECT_DIRECTORY=/files \
  --env HOP_PROJECT_NAME=apache-hop-minimum-project \
  --env HOP_ENVIRONMENT_NAME=dev \
  --env HOP_ENVIRONMENT_CONFIG_FILE_NAME_PATHS=/files/dev-config.json \
  --env HOP_RUN_CONFIG=local \
  -v ${PATH_TO_LOCAL_PROJECT_DIR}:/files \
  --name my-simple-hop-container \
  ${HOP_DOCKER_IMAGE}
```
