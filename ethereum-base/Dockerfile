FROM alpine:3.3

MAINTAINER Steven Bakhtiari

ENV DIRPATH /opt/eth
ENV PATH $DIRPATH/bin:$PATH
WORKDIR $DIRPATH

COPY ./genesis-block.json .

RUN mkdir bin && \
    apk add --update go git make gcc musl-dev         && \
    git clone https://github.com/ethereum/go-ethereum && \
    (cd go-ethereum && git checkout develop)          && \
    (cd go-ethereum && make geth)                     && \
    cp go-ethereum/build/bin/geth bin/                && \
    apk del go git make gcc musl-dev                  && \
    rm -rf go-ethereum && rm -rf /var/cache/apk/*

RUN geth --datadir ./chain init ./genesis-block.json
RUN mkdir -p ~/.ethash && geth --datadir ./chain makedag 0 ~/.ethash

