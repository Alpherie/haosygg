ARG BUILD_FROM
FROM $BUILD_FROM

ARG GOLANG_VERSION=1.23.5

RUN apk update && apk add --no-cache git make musl-dev go openssl-dev ca-certificates && update-ca-certificates

# Configure Go
RUN wget https://dl.google.com/go/go$GOLANG_VERSION.src.tar.gz && tar -C /usr/local -xzf go$GOLANG_VERSION.src.tar.gz

RUN cd /usr/local/go/src && ./make.bash

ENV PATH=$PATH:/usr/local/go/bin

RUN rm go$GOLANG_VERSION.src.tar.gz

#we delete the apk installed version to avoid conflict
RUN apk del go


#ENV GOROOT=/usr/lib/go
ENV GOPATH=/go
#ENV PATH=/go/bin:$PATH

#RUN mkdir -p ${GOPATH}/src ${GOPATH}/bin

WORKDIR $GOPATH

RUN git clone https://github.com/yggdrasil-network/yggdrasil-go

WORKDIR $GOPATH/yggdrasil-go

RUN ./build

RUN mkdir /var/run/yggdrasil/

RUN cp yggdrasil /usr/bin/
RUN cp yggdrasilctl /usr/bin/

#COPY yggdrasil.conf $GOPATH

#ENTRYPOINT [ "/usr/bin/yggdrasil", "-useconffile", "/config/yggdrasil.conf", "-logto", "/config/yggdrasil.log", "-loglevel", "debug" ]
