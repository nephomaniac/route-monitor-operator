FROM quay.io/redhat-services-prod/openshift/boilerplate:image-v7.2.0 AS builder

WORKDIR /workspace
# Copy the Go Modules manifests
COPY go.mod go.mod
COPY go.sum go.sum
# Copy the go source
COPY . .

# Build
# RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 GO111MODULE=on go build -mod vendor -o manager main.go
RUN make go-build

# Use distroless as minimal base image to package the manager binary
# Refer to https://github.com/GoogleContainerTools/distroless for more details
FROM registry.access.redhat.com/ubi8/ubi-minimal:8.10-1255
WORKDIR /
COPY --from=builder /workspace/build/_output/bin/* /manager
USER nonroot:nonroot

ENTRYPOINT ["/manager"]
