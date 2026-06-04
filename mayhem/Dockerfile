FROM golang:1.23 AS builder

ENV GOTOOLCHAIN=auto

WORKDIR /go/dhcp
ADD . .

RUN apt-get update && apt-get install -y clang && \
    CGO_ENABLED=1 CC=clang go test -c \
        -fuzz=FuzzDHCPv6 \
        -o /dhcpv6-fuzz.libfuzzer \
        -gcflags=all=-d=libfuzzer \
        -ldflags=-extldflags=-fsanitize=fuzzer \
        github.com/insomniacslk/dhcp/dhcpv6

FROM ubuntu:22.04
COPY --from=builder /dhcpv6-fuzz.libfuzzer /
