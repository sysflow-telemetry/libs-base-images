FROM debian:buster

ARG FALCO_LIBS_DRIVER_VERSION
ARG FALCO_LIBS_VERSION
ARG FALCO_LIBS_CHECKSUM
ARG FALCO_FIELDS_CHECKSUM
ARG DRIVER_PACKAGE_NAME=falco
ARG DRIVER_NAME=falco
ARG SCAP_BPF_PROBE_ENV_VAR_NAME=FALCO_BPF_PROBE
ARG FALCO_VERSION
ARG MAKE_JOBS

LABEL "name"="Base image for Falco Libs"
LABEL "vendor"="Falco"
LABEL "version"="${FALCO_LIBS_VERSION}"
LABEL "release"="${FALCO_LIBS_VERSION}"
LABEL "driver-version"="${FALCO_LIBS_DRIVER_VERSION}"
LABEL "summary"="Falco Libs implements userspace libraries and drivers for system event collection."
LABEL "description"="Falco Libs implements userspace libraries and drivers for system event collection."
LABEL "io.k8s.display-name"="Falco Libs"
LABEL "io.k8s.description"="Falco Libs implements userspace libraries and drivers for system event collection."
LABEL maintainer="cncf-falco-dev@lists.cncf.io"
LABEL usage="docker build --build-arg FALCO_LIBS_DRIVER_VERSION=${FALCO_LIBS_DRIVER_VERSION} --build-arg FALCO_LIBS_VERSION=${FALCO_LIBS_VERSION} --build-arg FALCO_VERSION=${FALCO_VERSION} --build-arg FALCO_FIELDS_CHECKSUM=${FALCO_FIELDS_CHECKSUM} -f ./docker/libs/Dockerfile -t FALCO/libs ."

ENV PATH=$PATH:/usr/local/go/bin/
ENV GOPATH=/go/

ENV FALCO_LIBS_LIB_PREFIX=/usr/lib/x86_64-linux-gnu/falcosecurity
ENV FALCO_LIBS_INCLUDE_PREFIX=/usr/include/falcosecurity
ENV FALCO_LIBS_CFLAGS='-I$(FALCO_LIBS_INCLUDE_PREFIX)/ \
                               -I$(FALCO_LIBS_INCLUDE_PREFIX)/userspace/libsinsp \
                               -I$(FALCO_LIBS_INCLUDE_PREFIX)/userspace/libscap'
ENV FALCO_LIBS_LDFLAGS='-L$(FALCO_LIBS_LIB_PREFIX)/ \
                                $(FALCO_LIBS_LIB_PREFIX)/libsinsp.a $(FALCO_LIBS_LIB_PREFIX)/libscap.a $(FALCO_LIBS_LIB_PREFIX)/libjq.a $(FALCO_LIBS_LIB_PREFIX)/libonig.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libb64.a $(FALCO_LIBS_LIB_PREFIX)/libcurl.a $(FALCO_LIBS_LIB_PREFIX)/libtbb.a $(FALCO_LIBS_LIB_PREFIX)/libgrpc++.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libgrpc++_alts.a $(FALCO_LIBS_LIB_PREFIX)/libgrpc++_reflection.a  $(FALCO_LIBS_LIB_PREFIX)/libgrpc++_error_details.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libgrpc++_unsecure.a  $(FALCO_LIBS_LIB_PREFIX)/libgrpc_plugin_support.a $(FALCO_LIBS_LIB_PREFIX)/libgrpcpp_channelz.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libgrpc.a $(FALCO_LIBS_LIB_PREFIX)/libgpr.a -lcares -lprotobuf  \
                                $(FALCO_LIBS_LIB_PREFIX)/libsinsp.a $(FALCO_LIBS_LIB_PREFIX)/libscap.a $(FALCO_LIBS_LIB_PREFIX)/libjq.a $(FALCO_LIBS_LIB_PREFIX)/libonig.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_synchronization.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_graphcycles_internal.a  $(FALCO_LIBS_LIB_PREFIX)/libabsl_status.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_statusor.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_strings.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_strings_internal.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_cord.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_cordz_functions.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_cord_internal.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_cordz_info.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_cordz_handle.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_hash.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_city.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_low_level_hash.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_raw_hash_set.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_hashtablez_sampler.a  $(FALCO_LIBS_LIB_PREFIX)/libabsl_random_internal_pool_urbg.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_random_internal_randen.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_random_internal_randen_hwaes.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_random_internal_randen_hwaes_impl.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_random_internal_platform.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_random_internal_randen_slow.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_random_internal_seed_material.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_random_seed_gen_exception.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_exponential_biased.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_bad_variant_access.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_str_format_internal.a  \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_raw_logging_internal.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_malloc_internal.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_base.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_spinlock_wait.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_throw_delegate.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_log_severity.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_bad_optional_access.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_time.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_time_zone.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_int128.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_stacktrace.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_symbolize.a \
                                $(FALCO_LIBS_LIB_PREFIX)/libabsl_debugging_internal.a $(FALCO_LIBS_LIB_PREFIX)/libabsl_demangle_internal.a \
		                        -lelf -lstdc++ -lz -lrt -lanl -lssl -lcrypto -lpthread -lm -ldl -lupb -laddress_sorting -lre2'

RUN mkdir -p /build && cd build && \
    apt update && apt install -y --no-install-recommends \
        git \
        cmake \
        build-essential \
        pkg-config \
        autoconf \
        libtool \
        libelf-dev \
        wget && \
    apt install -y --reinstall ca-certificates && \
    git config --global http.sslCAinfo /etc/ssl/certs/ca-certificates.crt && \
    wget https://github.com/falcosecurity/falco/archive/refs/tags/${FALCO_VERSION}.tar.gz && \
    tar xvf ${FALCO_VERSION}.tar.gz && cd falco-${FALCO_VERSION} && \
    ([ -z "$FALCO_FIELDS_CHECKSUM" ] || sed -i "s/\(#define FALCO_FIELDS_CHECKSUM \).*/\1\"${FALCO_FIELDS_CHECKSUM}\"/" userspace/engine/falco_engine_version.h) && \
    mkdir build && cd build && \
    cmake  \
	-DFALCO_VERSION=${FALCO_VERSION} \
        -DDRIVER_VERSION=${FALCO_LIBS_DRIVER_VERSION} \
        -DFALCOSECURITY_LIBS_VERSION=${FALCO_LIBS_VERSION} \
        -DFALCOSECURITY_LIBS_CHECKSUM=${FALCO_LIBS_CHECKSUM} \
        -DDRIVER_NAME=${DRIVER_NAME} \
        -DDRIVER_PACKAGE_NAME=${DRIVER_PACKAGE_NAME} \
        -DCMAKE_INSTALL_PREFIX=/usr/ \
        -DCMAKE_BUILD_TYPE=Release \
        -DUSE_BUNDLED_GTEST=ON \
        -DUSE_BUNDLED_DEPS=ON \
        -DCREATE_TEST_TARGETS=OFF \
        -DBUILD_BPF=OFF  \
        -DBUILD_DRIVER=OFF ../. && \
    make ${MAKE_JOBS:+-j ${MAKE_JOBS}} install && \
    cp /build/falco-${FALCO_VERSION}/docker/falco/docker-entrypoint.sh /usr/bin/ && \
    cp /build/falco-${FALCO_VERSION}/docker/ubi/docker-entrypoint.sh /usr/bin/docker-entrypoint-ubi.sh && \
    wget https://go.dev/dl/go1.17.7.linux-amd64.tar.gz && \
    tar -C /usr/local -xzf go1.17.7.linux-amd64.tar.gz 
    #&& \
    #rm -rf /build && rm -rf /var/lib/apt/lists/* && \
    #rm /usr/bin/falco
