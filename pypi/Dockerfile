#
# Dockerfile to spin up private PyPi mirror.
#
# --------------------------------------------------------------


# meta
# ----
FROM python:3.6.5
MAINTAINER Blake Printy "bprinty@gmail.com"


# config
# ------
ENV PIP_NO_CACHE_DIR="off"
ENV PIP_INDEX_URL="https://pypi.python.org/simple"
ENV PIP_TRUSTED_HOST="127.0.0.1"


# install
# -------
# create devpi user
RUN addgroup --system --gid 1000 devpi
RUN adduser --disabled-password --system --uid 1000 --home /data --shell /sbin/nologin --gid 1000 devpi

# install server
RUN pip install \
    devpi-server \
    devpi-client \
    devpi-web

# initialize devpi server
USER devpi
ENV HOME /data
WORKDIR /data
RUN devpi-server --restrict-modify root --start --host 127.0.0.1 --port 8080 --init && \
    devpi use http://127.0.0.1:8080 && \
    devpi login root --password='' && \
    devpi user -m root password=admin && \
    devpi index -y -c public pypi_whitelist='*' && \
    devpi-server --stop

# expose volume and port
VOLUME /data
EXPOSE 8080

# entrypoints
CMD devpi
ENTRYPOINT devpi-server --restrict-modify root --host 0.0.0.0 --port 8080 --start && \
           tail -f /data/.devpi/server/.xproc/devpi-server/xprocess.log
