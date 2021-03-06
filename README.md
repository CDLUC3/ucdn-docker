Cloudhost Docker
================

This is the build and deployment instructions for the CLoudhost service under Docker.

Requirements
------------

- Docker 
- If Docker is run as non-root, user must have Docker privileges 
  https://docs.docker.com/install/linux/linux-postinstall/

Building Docker image
---------------------

This needs only be performed by UC3.  For others, the image will be served by Dockerhub. 

- docker build --tag=cdluc3/cloudhost .

Deploying image
---------------

This will initialize the container and start the service.  This needs only be performed once
per Cloudhost instance.  See admin section for subsequent start/stop functions.

Define host ports and mount points to map with Docker ports and mounts.
If you would like the log files and data to be owned by a non-root user, then specify
that user in the --user argument as a numeric UID/GID.  Default will be the root user.
Naming of your container is also available.  

- docker run \
        --detach \
        --restart unless-stopped \
        --user ${USERID}:${GROUPID} \
        --name ${NAME} \
        --publish ${PORT_SSL}:30443 \
        --publish ${PORT_HTTP}:38080 \
        --volume ${DATADIR}:/apps/cloudhost/fileCloud \
        --volume ${LOGDIR}:/apps/cloudhost/logs \
        cdluc3/cloudhost

Test the container
------------------

The following URL on the deployed host will check Cloudhost status:
    http://localhost:${PORT_HTTP}/cloudhost/state?t=xml 

${DATADIR} will house the pairtree data.
${LOGDIR} will house the log files.

Administration tools
--------------------

To stop the Cloudhost service, just stop the container.  Same applies for starting.
These commands must reference the container name defined during initialization.

Stop/Start container
- docker container stop ${NAME}
- docker container start ${NAME}

To examine Cloudhost container for debugging.
- docker exec -it ${NAME} /bin/bash

Template scripts
----------------

Provided scripts can be used as templates for much of the functionality described here.
