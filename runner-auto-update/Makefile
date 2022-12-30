DOCKER_APP_NAME="runner-autoupdate-ubuntu1804"

# github parameters
ACCESS_TOKEN=""	# token should be set here rather than in the command line
REPOSITORY_URL=""
RUNNER_NAME="ubuntu1804"


# HELP
# This will output the help for each task
# thanks to https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
.PHONY: help
help: ## This help.
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

.DEFAULT_GOAL := help

build: ## Build the image
	docker build -t $(DOCKER_APP_NAME) .

shell: ## Creates a shell inside the container for debug purposes
	docker run -it $(DOCKER_APP_NAME) bash

#shell-compose: ## Creates a shell inside the docker-compose service for debug purposes
	#docker-compose run --rm runner bash

start: ## Start the runner (ACCESS_TOKEN and REPOSITORY_URL must be set)
	docker run -it -e RUNNER_NAME=$(RUNNER_NAME) -e GITHUB_ACCESS_TOKEN=$(ACCESS_TOKEN) -e RUNNER_REPOSITORY_URL=$(REPOSITORY_URL) $(DOCKER_APP_NAME)

cleanup: ## Unregister runner from github
	docker run -it -e RUNNER_NAME=$(RUNNER_NAME) -e GITHUB_ACCESS_TOKEN=$(ACCESS_TOKEN) -e RUNNER_REPOSITORY_URL=$(REPOSITORY_URL) $(DOCKER_APP_NAME) cleanup 
