FROM 10.110.0.3:5000/zulip/postgres:13.2

RUN \
  apk --no-cache add \
    build-base \
    clang-dev \
    llvm10 \
    lz4-dev \
    msgpack-c-dev \
    zstd-dev

ENV PGROONGA_VERSION=2.2.9 \
    GROONGA_VERSION=11.0.1

RUN \
  mkdir build && \
  cd build && \
  wget https://packages.groonga.org/source/pgroonga/pgroonga-${PGROONGA_VERSION}.tar.gz && \
  tar xf pgroonga-${PGROONGA_VERSION}.tar.gz && \
  wget https://packages.groonga.org/source/groonga/groonga-${GROONGA_VERSION}.tar.gz && \
  tar xf groonga-${GROONGA_VERSION}.tar.gz -C pgroonga-${PGROONGA_VERSION}/vendor && \
  cd pgroonga-${PGROONGA_VERSION}/vendor/groonga-${GROONGA_VERSION} && \
  ./configure && \
  make -j$(nproc) && \
  make install && \
  cd ../../ && \
  make HAVE_MSGPACK=1 -j$(nproc) && \
  make install && \
  cd ../ && \
  rm -rf build \
  mkdir -p /var/lib/postgresql/data/pgdata/mnt/server/archivedir

RUN \
  apk del \
    build-base \
    clang \
    clang-dev \
    llvm10