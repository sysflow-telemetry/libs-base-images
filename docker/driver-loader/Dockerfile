ARG FALCO_LIBS_VERSION
FROM ghcr.io/sysflow-telemetry/libs-base-images/libs:${FALCO_LIBS_VERSION} as libs

FROM debian:buster as runtime

ARG FALCO_LIBS_VERSION
ARG FALCO_LIBS_DRIVER_VERSION
ARG DRIVER_PACKAGE_NAME=falco

LABEL "name"="Base image for Libs consumers containing the Falco driver loader"
LABEL "vendor"="Falco"
LABEL "version"="${FALCO_LIBS_VERSION}"
LABEL "release"="${FALCO_LIBS_VERSION}"
LABEL "driver-version"="${FALCO_LIBS_DRIVER_VERSION}"
LABEL "summary"="Falco drivers implement the probes for system event collection used by Libs consumers."
LABEL "description"="Falco drivers implement the probes for system event collection used by Libs consumers"
LABEL "io.k8s.display-name"="Falco Drivers"
LABEL "io.k8s.description"="Falco drivers implement the probes for system event collection used by Libs consumers"
LABEL maintainer="cncf-falco-dev@lists.cncf.io"
LABEL usage="docker build --build-arg FALCO_LIBS_DRIVER_VERSION=${FALCO_LIBS_DRIVER_VERSION} --build-arg FALCO_LIBS_VERSION=${FALCO_LIBS_VERSION} -f ./docker/driver-loader/Dockerfile -t FALCO/driver-loader ."

ARG VERSION_BUCKET=deb
ENV VERSION_BUCKET=${VERSION_BUCKET}

ENV HOST_ROOT /host
ENV HOME /root

RUN cp /etc/skel/.bashrc /root && cp /etc/skel/.profile /root

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
	bash-completion \
	bc \
	bison \
	clang-7 \
	ca-certificates \
	curl \
	dkms \
	flex \
	gnupg2 \
	gcc \
	jq \
	libc6-dev \
	libelf-dev \
	libmpx2 \
	libssl-dev \
	llvm-7 \
	netcat \
	xz-utils \
	&& rm -rf /var/lib/apt/lists/*

# gcc 6 is no longer included in debian stable, but we need it to
# build kernel modules on the default debian-based ami used by
# kops. So grab copies we've saved from debian snapshots with the
# prefix https://snapshot.debian.org/archive/debian/20170517T033514Z
# or so.

RUN curl -L -o cpp-6_6.3.0-18_amd64.deb https://download.falco.org/dependencies/cpp-6_6.3.0-18_amd64.deb \
	&& curl -L -o gcc-6-base_6.3.0-18_amd64.deb https://download.falco.org/dependencies/gcc-6-base_6.3.0-18_amd64.deb \
	&& curl -L -o gcc-6_6.3.0-18_amd64.deb https://download.falco.org/dependencies/gcc-6_6.3.0-18_amd64.deb \
	&& curl -L -o libasan3_6.3.0-18_amd64.deb https://download.falco.org/dependencies/libasan3_6.3.0-18_amd64.deb \
	&& curl -L -o libcilkrts5_6.3.0-18_amd64.deb https://download.falco.org/dependencies/libcilkrts5_6.3.0-18_amd64.deb \
	&& curl -L -o libgcc-6-dev_6.3.0-18_amd64.deb https://download.falco.org/dependencies/libgcc-6-dev_6.3.0-18_amd64.deb \
	&& curl -L -o libubsan0_6.3.0-18_amd64.deb https://download.falco.org/dependencies/libubsan0_6.3.0-18_amd64.deb \
	&& curl -L -o libmpfr4_3.1.3-2_amd64.deb https://download.falco.org/dependencies/libmpfr4_3.1.3-2_amd64.deb \
	&& curl -L -o libisl15_0.18-1_amd64.deb https://download.falco.org/dependencies/libisl15_0.18-1_amd64.deb \
	&& dpkg -i cpp-6_6.3.0-18_amd64.deb gcc-6-base_6.3.0-18_amd64.deb gcc-6_6.3.0-18_amd64.deb libasan3_6.3.0-18_amd64.deb libcilkrts5_6.3.0-18_amd64.deb libgcc-6-dev_6.3.0-18_amd64.deb libubsan0_6.3.0-18_amd64.deb libmpfr4_3.1.3-2_amd64.deb libisl15_0.18-1_amd64.deb \
	&& rm -f cpp-6_6.3.0-18_amd64.deb gcc-6-base_6.3.0-18_amd64.deb gcc-6_6.3.0-18_amd64.deb libasan3_6.3.0-18_amd64.deb libcilkrts5_6.3.0-18_amd64.deb libgcc-6-dev_6.3.0-18_amd64.deb libubsan0_6.3.0-18_amd64.deb libmpfr4_3.1.3-2_amd64.deb libisl15_0.18-1_amd64.deb

# gcc 5 is no longer included in debian stable, but we need it to
# build centos kernels, which are 3.x based and explicitly want a gcc
# version 3, 4, or 5 compiler. So grab copies we've saved from debian
# snapshots with the prefix https://snapshot.debian.org/archive/debian/20190122T000000Z.

RUN curl -L -o cpp-5_5.5.0-12_amd64.deb https://download.falco.org/dependencies/cpp-5_5.5.0-12_amd64.deb \
	&& curl -L -o gcc-5-base_5.5.0-12_amd64.deb https://download.falco.org/dependencies/gcc-5-base_5.5.0-12_amd64.deb \
	&& curl -L -o gcc-5_5.5.0-12_amd64.deb https://download.falco.org/dependencies/gcc-5_5.5.0-12_amd64.deb \
	&& curl -L -o libasan2_5.5.0-12_amd64.deb	https://download.falco.org/dependencies/libasan2_5.5.0-12_amd64.deb \
	&& curl -L -o libgcc-5-dev_5.5.0-12_amd64.deb https://download.falco.org/dependencies/libgcc-5-dev_5.5.0-12_amd64.deb \
	&& curl -L -o libisl15_0.18-4_amd64.deb https://download.falco.org/dependencies/libisl15_0.18-4_amd64.deb \
	&& curl -L -o libmpx0_5.5.0-12_amd64.deb https://download.falco.org/dependencies/libmpx0_5.5.0-12_amd64.deb \
	&& dpkg -i cpp-5_5.5.0-12_amd64.deb gcc-5-base_5.5.0-12_amd64.deb gcc-5_5.5.0-12_amd64.deb libasan2_5.5.0-12_amd64.deb libgcc-5-dev_5.5.0-12_amd64.deb libisl15_0.18-4_amd64.deb libmpx0_5.5.0-12_amd64.deb \
	&& rm -f cpp-5_5.5.0-12_amd64.deb gcc-5-base_5.5.0-12_amd64.deb gcc-5_5.5.0-12_amd64.deb libasan2_5.5.0-12_amd64.deb libgcc-5-dev_5.5.0-12_amd64.deb libisl15_0.18-4_amd64.deb libmpx0_5.5.0-12_amd64.deb

# Since our base Debian image ships with GCC 7 which breaks older kernels, revert the
# default to gcc-5.
RUN rm -rf /usr/bin/gcc && ln -s /usr/bin/gcc-5 /usr/bin/gcc

RUN rm -rf /usr/bin/clang \
	&& rm -rf /usr/bin/llc \
	&& ln -s /usr/bin/clang-7 /usr/bin/clang \
	&& ln -s /usr/bin/llc-7 /usr/bin/llc

# Some base images have an empty /lib/modules by default
# If it's not empty, docker build will fail instead of
# silently overwriting the existing directory
RUN rm -df /lib/modules \
	&& ln -s $HOST_ROOT/lib/modules /lib/modules

# debian:stable head contains binutils 2.31, which generates
# binaries that are incompatible with kernels < 4.16. So manually
# forcibly install binutils 2.30-22 instead.
RUN curl -L -o binutils_2.30-22_amd64.deb https://download.falco.org/dependencies/binutils_2.30-22_amd64.deb \
	&& curl -L -o libbinutils_2.30-22_amd64.deb https://download.falco.org/dependencies/libbinutils_2.30-22_amd64.deb \
	&& curl -L -o binutils-x86-64-linux-gnu_2.30-22_amd64.deb https://download.falco.org/dependencies/binutils-x86-64-linux-gnu_2.30-22_amd64.deb \
	&& curl -L -o binutils-common_2.30-22_amd64.deb https://download.falco.org/dependencies/binutils-common_2.30-22_amd64.deb \
	&& dpkg -i *binutils*.deb \
	&& rm -f *binutils*.deb

# Copy the driver loader and entrypoint scripts
COPY --from=libs /usr/bin/falco-driver-loader /usr/bin/
COPY --from=libs /usr/bin/docker-entrypoint.sh /
COPY --from=libs /usr/src/falco-${FALCO_LIBS_DRIVER_VERSION} /usr/src/falco-${FALCO_LIBS_DRIVER_VERSION}

ENTRYPOINT ["/docker-entrypoint.sh"]
