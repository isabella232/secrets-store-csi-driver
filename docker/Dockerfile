ARG BASEIMAGE=us.gcr.io/k8s-artifacts-prod/build-image/debian-base-amd64:buster-v1.3.0

FROM golang:1.13.10-alpine3.10 as builder
WORKDIR /go/src/sigs.k8s.io/secrets-store-csi-driver
ADD . .
ARG TARGETARCH
ARG TARGETOS
ARG LDFLAGS
RUN CGO_ENABLED=0 GOOS=$TARGETOS GOARCH=$TARGETARCH go build -a -ldflags "${LDFLAGS}" -o _output/secrets-store-csi ./cmd/secrets-store-csi-driver

FROM $BASEIMAGE
COPY --from=builder /go/src/sigs.k8s.io/secrets-store-csi-driver/_output/secrets-store-csi /secrets-store-csi
RUN clean-install ca-certificates cifs-utils mount apt

LABEL maintainers="ritazh"
LABEL description="Secrets Store CSI Driver"

ENTRYPOINT ["/secrets-store-csi"]
