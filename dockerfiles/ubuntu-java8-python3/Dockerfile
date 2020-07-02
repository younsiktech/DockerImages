# Pull base image.
FROM ubuntu:18.04

LABEL maintainer="younsik.tech@gmail.com"
LABEL version="1.0"
LABEL description="docker image for local development(python3 & java8)"

# chagne apt repo
RUN echo "deb http://ftp.daum.net/ubuntu/ bionic main restricted universe multiverse" > /etc/apt/sources.list && \
		echo "deb http://ftp.daum.net/ubuntu/ bionic-security main restricted universe multiverse" >> /etc/apt/sources.list && \
		echo "deb http://ftp.daum.net/ubuntu/ bionic-updates main restricted universe multiverse" >> /etc/apt/sources.list && \
		echo "deb http://ftp.daum.net/ubuntu/ bionic-proposed main restricted universe multiverse" >> /etc/apt/sources.list && \
		echo "deb http://ftp.daum.net/ubuntu/ bionic-backports main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb http://01.archive.ubuntu.com/ubuntu/ trusty main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb-src http://01.archive.ubuntu.com/ubuntu/ trusty main restricted universe multiverse" >> /etc/apt/sources.list
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils && apt-get install -y locales tzdata

# locale
# RUN locale-gen en_US.UTF-8

RUN echo en_US.UTF-8 UTF-8 >> /etc/locale.gen && \
    locale-gen && \
    update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# set timezone
ENV TZ=Asia/Seoul

RUN echo $TZ > /etc/timezone && \
    rm /etc/localtime && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# using apps
RUN apt-get -y install net-tools curl vim git unzip wget

# install JAVA(openjdk8) & Python3(pip, venv) => Ubuntu18.04 has Python3.6.x
RUN apt-get -y install openjdk-8-jdk python3-pip python3-venv

# add user account
RUN useradd -ms /bin/bash devlocal
RUN chown devlocal.devlocal /home/devlocal -R

RUN echo 'root:root' | chpasswd
RUN echo 'devlocal:devlocal' | chpasswd

USER devlocal

# make application home
RUN mkdir -p /home/devlocal/apps && \
		mkdir -p /home/devlocal/logs && \
		mkdir -p /home/devlocal/config && \
		mkdir -p /home/devlocal/bin && \
		mkdir -p /home/devlocal/build

# JAVA 1.8 setting
ENV JAVA_HOME /usr/lib/jvm/java-1.8.0-openjdk-amd64
ENV PATH $PATH:$JAVA_HOME/bin

# Gradle
RUN cd /home/devlocal/build && \
    curl -O -L https://services.gradle.org/distributions/gradle-5.4.1-bin.zip && \
    unzip -o /home/devlocal/build/gradle-5.4.1-bin.zip -d /home/swproj/apps && \
    rm gradle-5.4.1-bin.zip

# Gradle setting
ENV GRADLE_HOME /home/devlocal/apps/gradle-5.4.1
ENV PATH $PATH:$GRADLE_HOME/bin

WORKDIR /home/devlocal

RUN echo "export JAVA_HOME=$JAVA_HOME" >> ~/.bashrc
RUN echo "export GRADLE_HOME=$GRADLE_HOME" >> ~/.bashrc
RUN echo "export PATH=$PATH" >> ~/.bashrc
RUN echo "alias python3=python3.6" >> ~/.bashrc

