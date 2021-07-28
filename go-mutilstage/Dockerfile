FROM golang:1.16.6-alpine as builder

# Create and change to the app directory.
WORKDIR /data



# Copy local code to the container image.
COPY . /data

# Retrieve application dependencies.
# This allows the container build to reuse cached dependencies.
# Expecting to copy go.mod and if present go.sum.
RUN go mod download
# Build the binary.
RUN go build -v -o intellgentUnderwriting

# Use the official Debian slim image for a lean production container.
# https://hub.docker.com/_/debian
# https://docs.docker.com/develop/develop-images/multistage-build/#use-multi-stage-builds
#RUN set -x && apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
#    ca-certificates && \
#    rm -rf /var/lib/apt/lists/*
FROM alpine
# add basic commands and shell hints
RUN  apk add util-linux pciutils usbutils coreutils binutils findutils grep curl
RUN  apk add bash bash-doc bash-completion
# Copy the binary to the production image from the builder stage.
COPY --from=builder /data /data


# Run the web service on container startup.
CMD ["/bin/bash","/data/bin/start.sh"]