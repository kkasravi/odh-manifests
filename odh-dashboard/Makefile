OC_PROJECT ?= redhat-ods-applications
DASHBOARD_NAME ?= odh-dashboard
ROUTE_NAMESPACE ?= redhat-ods-applications
IMAGE_NAME ?= s2i-odh-dashboard
TAG_NAME ?= v0.0.1
REPLICAS ?= 2

##@ General

help: ## Display this help.
	@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n  make \033[36m<target>\033[0m\n"} /^[a-zA-Z_0-9-]+:.*?##/ { printf "  \033[36m%-20s\033[0m %s\n", $$1, $$2 } /^##@/ { printf "\n\033[1m%s\033[0m\n", substr($$0, 5) } ' $(MAKEFILE_LIST)

all: show-vars

##@ Setting and Showing Variables

show-vars:: ## Show OC_PROJECT DASHBOARD_NAME ROUTE_NAMESPACE IMAGE_NAME TAG_NAME REPLICAS values
	@echo OC_PROJECT=$(OC_PROJECT) DASHBOARD_NAME=$(DASHBOARD_NAME) ROUTE_NAMESPACE=$(ROUTE_NAMESPACE) IMAGE_NAME=$(IMAGE_NAME) TAG_NAME=$(TAG_NAME) REPLICAS=$(REPLICAS)

set-project:: ## Set the current project or namespace to OC_PROJECT
	oc project $(OC_PROJECT) || exit 0

set-dashboard-name:: ## Call 'kustomize cfg set ' with DASHBOARD_NAME
	@kustomize cfg set . DASHBOARD_NAME $(DASHBOARD_NAME) -R 1>/dev/null || exit 0

set-namespace::
	@kustomize cfg set . NAMESPACE_NAME $(OC_PROJECT) -R 1>/dev/null || exit 0

set-route-namespace::
	@kustomize cfg set . ROUTE_NAMESPACE $(ROUTE_NAMESPACE) -R 1>/dev/null || exit 0

set-image::
	@kustomize cfg set . IMAGE_NAME $(IMAGE_NAME) -R 1>/dev/null || exit 0
	@kustomize cfg set . TAG_NAME $(TAG_NAME) -R 1>/dev/null || exit 0

set-replicas::
	@kustomize cfg set . REPLICAS $(REPLICAS) -R 1>/dev/null || exit 0

##@ Deployment 

deploy: show-vars set-project set-dashboard-name set-namespace set-route-namespace set-image set-replicas ## Set Variables then call 'kubectl apply '
	kubectl apply -k . || exit 0
	unset OC_PROJECT DASHBOARD_NAME ROUTE_NAMESPACE IMAGE_NAME TAG_NAME REPLICAS && make reset

undeploy: show-vars set-project set-dashboard-name set-namespace set-route-namespace set-image set-replicas ## Set Variables then call 'kubectl delete '
	kubectl delete -k . -l 'app in ('$(DASHBOARD_NAME)')' || exit 0
	unset OC_PROJECT DASHBOARD_NAME ROUTE_NAMESPACE IMAGE_NAME TAG_NAME REPLICAS && make reset

reset: show-vars set-project set-dashboard-name set-namespace set-route-namespace set-image set-replicas

.PHONY: defaults
defaults: ## Reset Variables to their defaults
	unset DASHBOARD_NAME IMAGE_NAME NAMESPACE_NAME OC_PROJECT ROUTE_NAMESPACE IMAGE_NAME TAG_NAME REPLICAS && make reset
