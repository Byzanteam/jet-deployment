.DEFAULT_GOAL := help

ifneq ($(wildcard .env),)
	include .env
endif

ifndef RELEASE
	ERR = $(error Please provide `RELEASE` in `.env` file or set it in env vars)
endif

.PHONY: help install uninstall airbase rollout-airbase

help: echo-release ## List commands
	@awk 'BEGIN {FS = ":.*##"; printf ""} /^[a-zA-Z_-]+:.*?##/ { printf "  \033[36m%-20s\033[0m %s\n", $$1, $$2 }' $(MAKEFILE_LIST)

install: echo-release ## Install the release
	@helm install -f values.local.yaml $(RELEASE) ./

upgrade: echo-release ## Upgrade the relase
	@helm upgrade -f values.local.yaml $(RELEASE) ./

uninstall: echo-release ## Uninstall current release
	@helm uninstall $(RELEASE)

airbase: echo-release ## Enter the runnig airbase container
	# Entering airbase...
	@kubectl exec -it `kubectl get pods --selector "app=$(RELEASE)-jet-chart-airbase" --output=name` -- sh

tail-airbase: echo-release ## Logging the runnig airbase container
	# Logging airbase...
	@kubectl logs -f --tail=0 `kubectl get pods --selector "app=$(RELEASE)-jet-chart-airbase" --output=name`

log-airbase: echo-release ## Logging the runnig airbase container
	# Logging airbase...
	@kubectl logs -f `kubectl get pods --selector "app=$(RELEASE)-jet-chart-airbase" --output=name`

rollout-airbase: echo-release ## Restart Airbase
	@kubectl rollout restart deploy/$(RELEASE)-jet-chart-airbase

dryrun: echo-release
	@helm install -f values.local.yaml --dry-run $(RELEASE) ./

echo-release:
	$(ERR)
	# ReleaseName: `$(RELEASE)`
	# ----------------------
