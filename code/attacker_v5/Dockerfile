# syntax = docker/dockerfile:1-experimental
FROM golang:1.23-alpine AS build

# Install dependencies
RUN apk update && \
    apk upgrade && \
    apk add --no-cache bash git openssh make build-base

WORKDIR /build

COPY . /build/attacker-service

RUN --mount=type=cache,target=/go/pkg/mod \
    cd /build/attacker-service && go mod download

RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    cd /build/attacker-service && make

FROM alpine

RUN apk update && \
    apk upgrade && \
    apk add --no-cache bash git openssh make build-base

WORKDIR /root

COPY  --from=build /build/attacker-service/build/bin/attacker /usr/bin/attacker

ENTRYPOINT [ "/usr/bin/attacker" ]
