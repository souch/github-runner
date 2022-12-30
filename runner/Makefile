DOCKER_APP_NAME="runner-ubuntu1804"

# github parameters
TOKEN="" # token should be set here rather than in the command line
OWNER=""
REPO=""
RUNNER_NAME="ubuntu1804"

.PHONY: help
help: ## This help.
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

.DEFAULT_GOAL := help

build: ## Build the image
	docker build -t $(DOCKER_APP_NAME) .

shell: ## Creates a shell inside the container for debug purposes
	docker run -it $(DOCKER_APP_NAME) bash

start: ## start the runner (TOKEN, OWNER and REPO must be set)
	docker run -it -e NAME=$(RUNNER_NAME) -e TOKEN=$(TOKEN) -e OWNER=$(OWNER) -e REPO=$(REPO) $(DOCKER_APP_NAME)
