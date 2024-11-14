REPO := harku/grpc-cpp
GRPC_VER := latest
UBUNTU_VER := noble

.PHONY: image
image:
	docker build \
		--build-arg UBUNTU_VERSION=${UBUNTU_VER} \
		-t "${REPO}:${GRPC_VER}-${UBUNTU_VER}" .

.PHONY: image-openssl
image-openssl:
	docker build \
		--build-arg UBUNTU_VERSION=${UBUNTU_VER} \
		--build-arg USE_OPENSSL=true \
		-t "${REPO}:${GRPC_VER}-${UBUNTU_VER}-openssl" .
