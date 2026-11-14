# Copyright 2018 The Kubernetes Authors.
# Copyright 2022 Google LLC
# Copyright 2023 Preferred Networks, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Build driver go binary
FROM --platform=$BUILDPLATFORM golang:1.23 AS driver-builder

ARG STAGINGVERSION
ARG TARGETARCH

WORKDIR /meta-fuse-csi-plugin
ADD . .
RUN make driver BINDIR=/bin GOARCH=${TARGETARCH}

# Use Alpine as the runtime base to support linux/riscv64
FROM alpine:3.20

# Install util-linux for mount/umount and ca-certificates for TLS
RUN apk add --no-cache util-linux ca-certificates

COPY --from=driver-builder /bin/meta-fuse-csi-plugin /meta-fuse-csi-plugin

ENTRYPOINT ["/meta-fuse-csi-plugin"]
