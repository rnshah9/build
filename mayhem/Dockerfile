FROM --platform=linux/amd64 ubuntu:22.04 as builder

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential

COPY . /repo
WORKDIR /repo
RUN chmod +x bootstrap.sh
RUN ./bootstrap.sh
WORKDIR /repo/example/hello
RUN rm jamroot.jam

RUN mkdir -p /deps
RUN ldd /repo/b2 | tr -s '[:blank:]' '\n' | grep '^/' | xargs -I % sh -c 'cp % /deps;'

FROM ubuntu:22.04 as package

COPY --from=builder /deps /deps
COPY --from=builder /repo /repo
ENV LD_LIBRARY_PATH=/deps

WORKDIR /repo/example/hello
