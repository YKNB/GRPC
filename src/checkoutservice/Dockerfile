# Copyright 2020 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM golang:1.20.7-alpine@sha256:29a2b239b10daa321685e58dd58ca7f5e04639f7fc99ba5af84975e2796b7cd7 as builder
RUN apk add --no-cache ca-certificates git
RUN apk add build-base
WORKDIR /src

# restore dependencies
COPY go.mod go.sum ./
RUN go mod download

COPY . .

# Skaffold passes in debug-oriented compiler flags
ARG SKAFFOLD_GO_GCFLAGS
RUN go build -gcflags="${SKAFFOLD_GO_GCFLAGS}" -o /checkoutservice .

FROM alpine:3.18.3@sha256:7144f7bab3d4c2648d7e59409f15ec52a18006a128c733fcff20d3a4a54ba44a
RUN apk add --no-cache ca-certificates

WORKDIR /src
COPY --from=builder /checkoutservice /src/checkoutservice

# Definition of this variable is used by 'skaffold debug' to identify a golang binary.
# Default behavior - a failure prints a stack trace for the current goroutine.
# See https://golang.org/pkg/runtime/
ENV GOTRACEBACK=single

EXPOSE 5050
ENTRYPOINT ["/src/checkoutservice"]
