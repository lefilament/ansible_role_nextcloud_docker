FROM index.docker.io/tecnativa/duplicity
RUN apk add --no-cache mysql-client \
  && apk add --no-cache --update --virtual build-dependencies \
    python \
    python-dev \
    py-pip \
    build-base \
  && pip install python-swiftclient
ENV JOB_200_WHAT='mysqldump -h ${MYSQL_HOST} -u ${MYSQL_USER} -p${MYSQL_PASSWORD} ${MYSQL_DATABASE} --result-file="$SRC/mysql_${MYSQL_HOST}_${MYSQL_DATABASE}.sql"' \
    JOB_200_WHEN='daily' \
    JOB_700_WHAT='rm "$SRC/mysql_${MYSQL_HOST}_${MYSQL_DATABASE}.sql"' \
    JOB_700_WHEN='daily'
