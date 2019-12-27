FROM index.docker.io/tecnativa/duplicity
RUN apk add --no-cache mysql-client
RUN apk add --no-cache --virtual .build \
        build-base \
        krb5-dev \
        libffi-dev \
        librsync-dev \
        libxml2-dev \
        libxslt-dev \
        openssl-dev \
    # Runtime dependencies, based on https://bazaar.launchpad.net/~duplicity-team/duplicity/0.8-series/view/head:/requirements.txt
    && pip install --no-cache-dir \
        # Basic dependencies
        python-keystoneclient \
    && apk del .build
ENV JOB_200_WHAT='mysqldump -h ${MYSQL_HOST} -u ${MYSQL_USER} -p${MYSQL_PASSWORD} ${MYSQL_DATABASE} --result-file="$SRC/mysql_${MYSQL_HOST}_${MYSQL_DATABASE}.sql"' \
    JOB_200_WHEN='daily' \
    JOB_700_WHAT='rm "$SRC/mysql_${MYSQL_HOST}_${MYSQL_DATABASE}.sql"' \
    JOB_700_WHEN='daily'
