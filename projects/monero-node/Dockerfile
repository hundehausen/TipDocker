# Usage: docker run --restart=always -v /var/data/blockchain-xmr:/root/.bitmonero -p 18080:18080 -p 18081:18081 --name=monerod -td kannix/monero-full-node
FROM ubuntu:18.04 AS build

ENV MONERO_VERSION=0.17.1.9 MONERO_SHA256=0fb6f53b7b9b3b205151c652b6c9ca7e735f80bfe78427d1061f042723ee6381

RUN apt-get update && apt-get install -y curl bzip2

WORKDIR /root

RUN curl https://dlsrc.getmonero.org/cli/monero-linux-x64-v$MONERO_VERSION.tar.bz2 -O &&\
  echo "$MONERO_SHA256  monero-linux-x64-v$MONERO_VERSION.tar.bz2" | sha256sum -c - &&\
  tar -xvf monero-linux-x64-v$MONERO_VERSION.tar.bz2 &&\
  rm monero-linux-x64-v$MONERO_VERSION.tar.bz2 &&\
  cp ./monero-x86_64-linux-gnu-v$MONERO_VERSION/monerod . &&\
  rm -r monero-*

# no more required, use --enable-dns-blocklist insted
#RUN curl https://gui.xmr.pm/files/block.txt > /root/block.txt

FROM ubuntu:18.04

RUN useradd -ms /bin/bash monero && mkdir -p /home/monero/.bitmonero && chown -R monero:monero /home/monero/.bitmonero
USER monero
WORKDIR /home/monero

COPY --chown=monero:monero --from=build /root/monerod /home/monero/monerod

# blockchain location
VOLUME /home/monero/.bitmonero

EXPOSE 38080 38081

ENTRYPOINT ["./monerod"]
CMD ["--non-interactive", "--config-file=/home/monero/.bitmonero/stagenet.conf"]