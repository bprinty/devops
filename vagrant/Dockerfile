#
# Dockerfile to use in conjunction with Vagrant for simpler
# vagrant/ansible workflows. This image essentially installs
# python3, ansible, and an ssh daemon so that vagrant can connect
# to it. Within vagrant, you can still use ansible to provision
# the box, so that you can mirror prod/dev workflows.
#
# --------------------------------------------------------------


# meta
# ----
FROM ubuntu:xenial
MAINTAINER Blake Printy "bprinty@gmail.com"


# install
# -------
# installing apt-get deps
RUN apt-get update && apt-get install -y software-properties-common
RUN apt-get update && apt-get install -y \
    sudo \
    locales \
    wget \
    curl \
    bzip2 \
    bc \
    make \
    man \
    less \
    rsync \
    unzip \
    openssh-server \
    libffi-dev \
    libssl-dev \
    libxml2-dev \
    libxslt1-dev \
    libjpeg-dev \
    zlib1g-dev \
    emacs24 \
    vim

# install conda
RUN wget -q https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /tmp/install-conda.sh && \
    /bin/bash /tmp/install-conda.sh -b -p /usr/local -u && \
    rm -rf /tmp/install-conda.sh

# install python
RUN add-apt-repository ppa:fkrull/deadsnakes
RUN apt-get update
RUN apt-get install -y \
    python3.5 \
    python3.5-dev \
    python3-pip
RUN ln -sf /usr/bin/python3.5 /usr/bin/python
RUN ln -sf /usr/bin/pip3 /usr/bin/pip

# install ansible
RUN pip install -U pip==10.0.1
RUN pip install \
    setuptools \
    ansible \
    ipython


# locale
# ------
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8


# config
# ------
# create vagrant account
RUN useradd --create-home -s /bin/bash vagrant
RUN echo "vagrant:vagrant" | chpasswd
RUN mkdir -p /etc/sudoers.d
RUN install -b -m 0440 /dev/null /etc/sudoers.d/vagrant
RUN echo 'vagrant ALL=NOPASSWD: ALL' >> /etc/sudoers.d/vagrant

# set upp vagrant ssh
RUN mkdir -p /home/vagrant/.ssh
RUN wget -q -O /home/vagrant/.ssh/authorized_keys https://raw.githubusercontent.com/hashicorp/vagrant/master/keys/vagrant.pub && \
    chmod 644 /home/vagrant/.ssh/authorized_keys && \
    chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys

# set up ssh daemon
RUN mkdir -p /var/run/sshd
RUN chmod 0755 /var/run/sshd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config


# run
# ---
# expose ports and start ssh daemon
RUN apt-get clean
EXPOSE 22 8080
CMD ["/usr/sbin/sshd", "-D"]

