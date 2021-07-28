# Invenio is free software; you can redistribute it and/or modify it
# under the terms of the MIT License; see LICENSE file for more details.
#
ARG CENTOS_VERSION=8
FROM centos:${CENTOS_VERSION}


# set the locale
# avoid Failed to set locale, defaulting to C.UTF-8
RUN yum install -y glibc-locale-source
RUN localedef --quiet -c -i en_US -f UTF-8 en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8 zh_CN.UTF-8

# install needed packages
RUN yum install -y \
        yum-utils \
        curl \
        git \
        tmux \
        vim \
        gcc \
        emacs-nox && \
    yum install -y \
        epel-release && \
    yum groupinstall -y "Development Tools" && \
    yum install -y \
        cairo-devel \
        dejavu-sans-fonts \
        libffi-devel \
        libxml2-devel \
        libxslt-devel \
        openssl-devel \
        ImageMagick

RUN yum clean -y all

# install nodejs version 14
RUN curl -sL https://rpm.nodesource.com/setup_14.x | bash - && \
    yum install -y nodejs

# Download and install Python 3.7
# Need to remove symlink from /usr/bin/python3 -> /usr/bin/python3.6
# so alinstall does not fail
RUN curl https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tgz -O -J -L && \
    tar xzf Python-3.7.0.tgz -C /usr/src/ && \
    rm -f /usr/bin/python3 && \
    /usr/src/Python-3.7.0/configure --enable-optimizations --prefix=/usr/bin/python3 && \
    make altinstall && \
    ln -s /usr/bin/python3/bin/python3.7 /usr/bin/python3.7 && \
    rm Python-3.7.0.tgz

#Install locale
RUN localedef -c -f UTF-8 -i zh_CN zh_CN.UTF-8
ENV LC_ALL "zh_CN.UTF-8"
ENV LD_LIBRARY_PATH "$LD_LIBRARY_PATH:/usr/local/lib"

# set python3.7 as default
RUN alternatives --install /usr/bin/unversioned-python python /usr/bin/python3.7 70
RUN alternatives --set python /usr/bin/python3.7
RUN ln -s /usr/bin/unversioned-python /usr/bin/python
# Add python binaries to the PATH
ENV PATH=/usr/bin/python3/bin:$PATH

# symlink pip
RUN ln -s /usr/bin/python3/bin/pip3.7 /usr/bin/pip
RUN pip --version && pip install --upgrade pip pipenv setuptools wheel
RUN ln -s /usr/bin/python3/bin/pipenv /usr/bin/pipenv

CMD ["bin/bash"]






