# SPDX-FileCopyrightText: 2019-present Open Networking Foundation <info@opennetworking.org>
#
# SPDX-License-Identifier: Apache-2.0

ARG ONOS_BUILD_VERSION=undefined

FROM onosproject/golang-build:v1.0 as build

ENV GO111MODULE=on
ARG ONOS_MAKE_TARGET=build

COPY Makefile go.mod go.sum /go/src/github.com/onosproject/onos-mlb/
COPY cmd/ /go/src/github.com/onosproject/onos-mlb/cmd/
COPY pkg/ /go/src/github.com/onosproject/onos-mlb/pkg/
COPY vendor/ /go/src/github.com/onosproject/onos-mlb/vendor/
COPY api/ /go/src/github.com/onosproject/onos-mlb/api
COPY build/build-tools /go/src/github.com/onosproject/onos-mlb/build/build-tools/

RUN cd /go/src/github.com/onosproject/onos-mlb && GOFLAGS=-mod=vendor make ${ONOS_MAKE_TARGET}

FROM alpine:3.11
RUN apk add libc6-compat

USER nobody

COPY --from=build /go/src/github.com/onosproject/onos-mlb/build/_output/onos-mlb /usr/local/bin/onos-mlb

ENTRYPOINT ["onos-mlb"]
