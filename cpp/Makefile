REPO := harku/grpc-cpp
GRPC_VER := latest

.PHONY: image
image:
	docker build -t "${REPO}:${GRPC_VER}" .

.PHONY: image-openssl
image-openssl:
	docker build --build-arg USE_OPENSSL=true -t "${REPO}:${GRPC_VER}-openssl" .
