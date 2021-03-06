MAKEFLAGS += --no-print-directory

export DOCKER_BUILDKIT = 1
export DOCKER_BUILD_PATH = Dockerfile ..
export DOCKER_BUILD_ARGS = --build-arg BUILDKIT_INLINE_CACHE=1

docker-help:
	@echo "Makefile for docker."
	@echo "Usage: make [TARGET]"

docker-pull:
	docker pull $(DOCKER_IMAGE) 2>/dev/null || true

docker-push:
	docker push $(DOCKER_IMAGE)

docker-tag:
	docker tag $(SOURCE_IMAGE) $(TARGET_IMAGE)

docker-image-filter:
	docker images --filter=label=docker-repository=$(DOCKER_REPOSITORY)

docker-image-prune:
	docker image prune --force --filter=label=docker-repository=$(DOCKER_REPOSITORY)
	@make docker-image-filter DOCKER_REPOSITORY=$(DOCKER_REPOSITORY)

docker-build:
	$(eval override TARGET=$(if $(TARGET), --target $(TARGET),))
	$(eval override CACHE_FROM=$(if $(CACHE_FROM), --cache-from $(CACHE_FROM),))
	docker build --label docker-repository=$(DOCKER_REPOSITORY) $(DOCKER_BUILD_ARGS) $(DOCKER_BUILD_OPTS) $(CACHE_FROM) $(TARGET) -t $(DOCKER_IMAGE) -f $(DOCKER_BUILD_PATH)
