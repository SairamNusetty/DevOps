FROM centos

MAINTAINER Sairam Nusetty <sairamnusetty@gmail.com>

#Adding the required yum repositories
RUN \
       /usr/bin/yum install -y --nogpgcheck epel-release \
       && rm -rf /var/cache/yum/* \
       && /usr/bin/yum clean all

# Setting up the required environmental variables
ENV PYTHON_MAJOR   3.8
ENV PYTHON_VERSION 3.8.8
ENV SETUP_TOOLS_VERSION 53.0.0

# Install Python Dependencies
RUN \
         /usr/bin/yum install -y --nogpgcheck xz wget tar sudo autoconf automake hostname make supervisor \
         && /usr/bin/yum install -y --nogpgcheck libffi-devel openssl-devel \
	 && /usr/bin/yum install -y --nogpgcheck gcc gcc-c++ mysql mysql-devel zlib-devel bzip2-devel libxslt-devel \
	 && /usr/bin/yum install -y --nogpgcheck screen \ 
         && /usr/bin/yum clean all


# Download, Install python3.8 & cleanup after installation is complete
RUN \
        cd /usr/local/ \
        && wget --no-check-certificate https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tar.xz \
        && tar xvf Python-${PYTHON_VERSION}.tar.xz \
        && cd Python-${PYTHON_VERSION} \
        && ./configure --prefix=/usr/local --enable-optimizations \
        && make && make altinstall \
        && ln -s /usr/local/bin/python${PYTHON_MAJOR} /usr/local/bin/python


# Installation of python setuptools & pip
RUN \ 
	cd /usr/local/ \
	&& wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-${SETUP_TOOLS_VERSION}.tar.gz \
	&& tar -xvf setuptools-${SETUP_TOOLS_VERSION}.tar.gz \
	&& rm -f setuptools-${SETUP_TOOLS_VERSION}.tar.gz \
	&& cd setuptools-${SETUP_TOOLS_VERSION} \
	&& python${PYTHON_MAJOR} setup.py install \
	&& curl https://bootstrap.pypa.io/get-pip.py | python${PYTHON_MAJOR} - \
	&& pip install virtualenv

# Installing the required Python packages & AWS CLI using pip.
RUN \
	pip install boto3 awscli cryptography avro \
	&& mkdir -p /home/sairam/codebase/ /home/sairam/pkg_binaries/ \
	&& chmod -R 777 /home/sairam/ \
        && /usr/bin/yum clean all \
        && rm -rf /var/cache/yum/* \
	&& rm -rf ~/.cache/pip/*

# Runtime command -  when staring the Docker container.
CMD ["/bin/bash"]

