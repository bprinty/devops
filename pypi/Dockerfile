
# THIS DOCKERFILE HAS NOT BEEN TESTED (CURRENTLY IN DEVELOPMENT)

FROM debian:7

RUN apt-get update
RUN apt-get install -y \
    build-essential \
    python-dev \
    python-pip \
    libxml2-dev \
    libxslt-dev \
    libffi-dev \
    libpq-dev


RUN pip install \
    pypiserver \
    PASSLIB

RUN mkdir /packages
RUN echo 'password' | htpasswd -sc /home/.htaccess admin 

RUN apt-get clean

EXPOSE 80

ENTRYPOINT ['pypi-server -p 80 -P /home/.htaccess /packages']

# install apache
# create user accounts
# install pypi server
# run pypi server
