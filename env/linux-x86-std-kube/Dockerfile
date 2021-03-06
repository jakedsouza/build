# Copyright 2014 The Go Authors. All rights reserved.
# Use of this source code is governed by a BSD-style
# license that can be found in the LICENSE file.

# Normal Linux builder
# Docker tag gcr.io/go-dashboard-dev/linux-x86-std-kube (staging)
# and gcr.io/symbolic-datum-552/linux-x86-std-kube (prod)

FROM debian:wheezy
MAINTAINER golang-dev <golang-dev@googlegroups.com>

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update
RUN apt-get install -y --no-install-recommends ca-certificates
# Optionally used by runtime tests for gdb:
RUN apt-get install -y --no-install-recommends gdb
# Optionally used by some net/http tests:
RUN apt-get install -y --no-install-recommends strace
# For building Go's bootstrap 'dist' prog
RUN apt-get install -y --no-install-recommends gcc libc6-dev
# For compiling cgo with fortran support (multilib for 386)
RUN apt-get install -y --no-install-recommends gfortran gfortran-multilib
# For 32-bit builds:
# TODO(bradfitz): move these into a 386 image that derives from this one.
RUN apt-get install -y --no-install-recommends libc6-dev-i386 gcc-multilib
# For downloading Go 1.4:
RUN apt-get install -y --no-install-recommends curl

# And misc basic tools:
RUN apt-get install -y --no-install-recommends procps lsof psmisc

# Required by x/mobile repo:
RUN apt-get install -y --no-install-recommends libgles2-mesa-dev libopenal-dev fonts-droid

RUN mkdir -p /go1.4-amd64 && (curl --silent https://storage.googleapis.com/golang/go1.4.linux-amd64.tar.gz | tar -C /go1.4-amd64 -zxv)
RUN mv /go1.4-amd64/go /go1.4 && rmdir /go1.4-amd64

RUN curl -o /usr/local/bin/stage0 https://storage.googleapis.com/go-builder-data/buildlet-stage0.linux-amd64-kube
RUN chmod +x /usr/local/bin/stage0

# TODO(evanbrown): optimize for image size and reduced layers
RUN rm -rf /var/lib/apt/lists /usr/share/doc
RUN rm -rf /go1.4/pkg/linux_amd64_race /go1.4/api /go1.4/blog /go1.4/doc /go1.4/misc /go1.4/test
RUN find /go1.4 -type d -name testdata | xargs rm -rf
RUN rm -rf /go1.4/pkg/linux_amd64_race /go1.4/test /go1.4/api
RUN (cd /usr/share/locale/ && ls -1 | grep -v en | xargs rm -rf)
RUN rm -rf /var/cache/debconf/*
RUN rm -rf /usr/share/man
