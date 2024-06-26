# Copyright 2021-2023 Intel Corporation
# All rights reserved.
#
# 'recipe' for building a base RHEL DAOS client docker image
#
# This Dockerfile accept the following input build arguments:
# - LINUX_DISTRO             Linux distribution identifier (default "el8")
# - DAOS_DOCKER_IMAGE_NSP    Namespace identifier of the base DAOS docker image (default "daos")
# - DAOS_DOCKER_IMAGE_TAG    Tag identifier of the DAOS client docker image (default "v2.4.0")
# - DAOS_VERSION             Version of DAOS to use (default "2.4.0-2.el8")

# Pull base image
ARG	LINUX_DISTRO="el8"
ARG	DAOS_DOCKER_IMAGE_NSP="daos"
ARG	DAOS_DOCKER_IMAGE_TAG="v2.4.0"
FROM	"$DAOS_DOCKER_IMAGE_NSP/daos-base-$LINUX_DISTRO:$DAOS_DOCKER_IMAGE_TAG"
LABEL	maintainer="daos@daos.groups.io"

# Install DAOS package
ARG	DAOS_VERSION="2.4.0-2.el8"
RUN	echo "[INFO] Installing DAOS containerization dependencies" ;                              \
	dnf install                                                                                \
		sudo                                                                               \
		xz &&                                                                              \
	echo "[INFO] Installing DAOS" ;                                                            \
	dnf install                                                                                \
		daos-client-${DAOS_VERSION} &&                                                     \
	dnf clean all

# Install DAOS agent configuration file
COPY	daos_agent.yml /etc/daos/daos_agent.yml

# Install DAOS agent launcher
COPY	run-daos_agent.sh /usr/local/sbin/run-daos_agent
RUN	chmod 755 /usr/local/sbin/run-daos_agent

# Define entrypoint and cmd:
# - ENTRYPOINT for the command to run
# - CMD for the default arguments
ENTRYPOINT ["/usr/local/sbin/run-daos_agent"]
CMD ["start"]
