# Dockerfile for gRPC C++

Build images that contain built gRPC library for individual Ubuntu versions. A
image will be tagged as:

```
<gRPC version>-<Ubuntu version>[-<option>]
```

For example, the image for gRPC *v1.67.1* that is built on Ubuntu *Focal* and
uses *OpenSSL* as SSL provider will be tagged as `1.67.1-focal-openssl`.

## Build Examples

Build gRPC v1.67.1 for Ubuntu 24.04 (Noble):

```bash
make image GRPC_VER=1.67.1 UBUNTU_VER=noble
```

Build gRPC v1.67.1 using OpenSSL for Ubuntu 20.04 (Focal):

```bash
make image-openssl GRPC_VER=1.67.1 UBUNTU_VER=focal
```

## Usage

Copy the gRPC library directory from the image with matched Ubuntu version tag
and add the `bin` directory of the copied gRPC library into your `PATH`.

For example, install gRPC library into a Ubuntu Noble image from the
`1.67.1-noble` image:

```dockerfile
FROM ubuntu:noble

COPY --from=harku/grpc-cpp:1.67.1-noble /grpc /opt/grpc
ENV PATH="${PATH}:/opt/grpc/bin"
```
