FROM ubuntu:trusty

ARG TAG=v1.19.1

WORKDIR /

# install build tools
RUN apt-get update \
  && apt-get install -y --no-install-recommends  software-properties-common\
  build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils python3 \ 
  # cleanup
  && rm -rf /var/lib/apt/lists/*

# install libdb
RUN add-apt-repository ppa:bitcoin/bitcoin -y \
  && apt-get update \
  && apt-get install -y --no-install-recommends libdb4.8-dev libdb4.8++-dev \
  # cleanup
  && rm -rf /var/lib/apt/lists/* \
  && apt-get remove -y software-properties-common 

# clone and build
RUN apt-get update \
  && apt-get install -y --no-install-recommends git libboost-all-dev \
  && git clone -b ${TAG} https://github.com/Pigeoncoin/pigeoncoin.git \
  && cd ./pigeoncoin \ 
  && export CC="gcc -fPIC"  \
  && export CXX="g++ -fPIC"  \
  && ./autogen.sh  \
  && ./configure --enable-cxx --disable-shared --with-pic --without-gui \
  && make  \
  && make install \
  # cleanup
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf /pigeoncoin \
  && apt-get remove -y git libboost-all-dev

# mainnet port
EXPOSE 8757

# rpc port
EXPOSE 8756

# chain data location
VOLUME /root/.pigeon

# set defaults
ENV IP = 1.1.1.1
ENV KEY = blsprivkey 

ENTRYPOINT pigeond -printtoconsole -listen -externalip=$IP -bind=$IP -masternodeblsprivkey=$KEY