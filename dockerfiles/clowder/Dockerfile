# Install clowder server
FROM java:latest
MAINTAINER Rob Kooper <kooper@illinois.edu>

# expose some properties of the container
EXPOSE 9000
ENV CLOWDER_BRANCH="CATS-CORE" \
	CLOWDER_BUILD="latestSuccessful" \
	CLOWDER_CONTEXT="/" \
	CLOWDER_UPDATE="NO" \
	MONGO_URI="" \
	SMTP_HOST="" \
	RABBITMQ_EXCHANGE="clowder" \
	RABBITMQ_VHOST="%2F" \
	RABBITMQ_URI="" \
	RABBITMQ_MGMT_PORT="" \
	ELASTICSEARCH_CLUSTERNAME="clowder" \
	ELASTICSEARCH_SERVER="" \
	ELASTICSEARCH_PORT=""

# install requirements
RUN useradd -m -s /bin/bash clowder

# install clowder
USER clowder
RUN cd /home/clowder && \
	case ${CLOWDER_BUILD} in latest*) BB="${CLOWDER_BRANCH}/${CLOWDER_BUILD}" ;; *) BB="${CLOWDER_BRANCH}-${CLOWDER_BUILD}" ;; esac && \
	/usr/bin/wget -q -e robots=off -A "clowder-*.zip" -nd -r -N -l1 https://opensource.ncsa.illinois.edu/bamboo/browse/${BB}/artifact/JOB1/dist/ && \
	LATEST=$( /bin/ls -1rt clowder-*.zip | tail -1 ) && \
	/usr/bin/unzip -q ${LATEST} && \
	/bin/mv $( basename ${LATEST} .zip ) clowder && \
	mkdir -p /home/clowder/clowder/custom

# export custom volume
VOLUME /home/clowder/clowder/custom

# command to run when starting docker
COPY FILES.clowder/ /
ENTRYPOINT ["/entrypoint.sh"]
CMD ["server"]

