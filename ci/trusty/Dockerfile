# KeePassXC Linux Release Build Dockerfile
# Copyright (C) 2017 KeePassXC team <https://keepassxc.org/>
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 2 or (at your option)
# version 3 of the License.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

# TIP: check this Dockerfile using this online tool: https://www.fromlatest.io

FROM ubuntu:14.04

ENV REBUILD_COUNTER=5

ENV QT5_VERSION=qt53
ENV QT5_PPA_VERSION=${QT5_VERSION}2
ENV TERM=xterm-256color

RUN set -x \
     && apt-get update -y \
     && apt-get -y install software-properties-common

RUN set -x \
    && add-apt-repository ppa:beineri/opt-${QT5_PPA_VERSION}-trusty \
    && add-apt-repository ppa:phoerious/keepassxc

RUN set -x \
    && apt-get -y update \
    && apt-get -y --no-install-recommends install \
        build-essential \
        clang-3.6 \
        libclang-common-3.6-dev \
        clang-format-3.6 \
        llvm-3.6 \
        cmake3 \
        make \
        libgcrypt20-18-dev \
        libargon2-0-dev \
        libsodium-dev \
        libcurl-no-gcrypt-dev \
        ${QT5_VERSION}base \
        ${QT5_VERSION}tools \
        ${QT5_VERSION}x11extras \
        ${QT5_VERSION}translations \
        ${QT5_VERSION}svg \
        zlib1g-dev \
        libyubikey-dev \
        libykpers-1-dev \
        # ubuntu:14.04 has no quazip (it's optional)
        # libquazip5-dev \
        libxi-dev \
        libxtst-dev \
        libqrencode-dev \
        xclip \
        xvfb

ENV PATH="/opt/${QT5_VERSION}/bin:${PATH}"
ENV CMAKE_PREFIX_PATH="/opt/${QT5_VERSION}/lib/cmake"
ENV CMAKE_INCLUDE_PATH="/opt/keepassxc-libs/include"
ENV CMAKE_LIBRARY_PATH="/opt/keepassxc-libs/lib/x86_64-linux-gnu"
ENV CPATH="${CMAKE_INCLUDE_PATH}"
ENV LD_LIBRARY_PATH="${CMAKE_LIBRARY_PATH}:/opt/${QT5_VERSION}/lib"

RUN set -x \
    && echo "/opt/${QT5_VERSION}/lib" > /etc/ld.so.conf.d/${QT5_VERSION}.conf \
    && echo "/opt/keepassxc-libs/lib/x86_64-linux-gnu" > /etc/ld.so.conf.d/keepassxc.conf

# AppImage dependencies
RUN set -x \
    && apt-get install -y \
        curl \
        libfuse2

RUN set -x \
    && curl -L "https://github.com/linuxdeploy/linuxdeploy/releases/download/continuous/linuxdeploy-x86_64.AppImage" > /usr/bin/linuxdeploy \
    && curl -L "https://github.com/linuxdeploy/linuxdeploy-plugin-qt/releases/download/continuous/linuxdeploy-plugin-qt-x86_64.AppImage" > /usr/bin/linuxdeploy-plugin-qt \
    && curl -L "https://github.com/AppImage/AppImageKit/releases/download/continuous/appimagetool-x86_64.AppImage" > /usr/bin/appimagetool \
    && chmod +x /usr/bin/linuxdeploy \
    && chmod +x /usr/bin/linuxdeploy-plugin-qt \
    && chmod +x /usr/bin/appimagetool

RUN set -x \
    && apt-get autoremove --purge \
    && rm -rf /var/lib/apt/lists/*

VOLUME ["/keepassxc"]
WORKDIR /keepassxc
