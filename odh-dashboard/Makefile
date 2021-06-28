OC_PROJECT ?= redhat-ods-applications
DASHBOARD_NAME ?= odh-dashboard
ROUTE_NAMESPACE ?= redhat-ods-applications
IMAGE_NAME ?= s2i-odh-dashboard
TAG_NAME ?= v0.0.1
REPLICAS ?= 2

all: output-vars

output-vars::
	@echo OC_PROJECT=$(OC_PROJECT) DASHBOARD_NAME=$(DASHBOARD_NAME) ROUTE_NAMESPACE=$(ROUTE_NAMESPACE) IMAGE_NAME=$(IMAGE_NAME) TAG_NAME=$(TAG_NAME) REPLICAS=$(REPLICAS)

set-project::
	oc project $(OC_PROJECT)

set-dashboard-name::
	kustomize cfg set . DASHBOARD_NAME $(DASHBOARD_NAME) -R

set-namespace::
	kustomize edit set namespace $(OC_PROJECT)
	kustomize cfg set . NAMESPACE_NAME $(OC_PROJECT) -R

set-route-namespace::
	kustomize cfg set . ROUTE_NAMESPACE $(ROUTE_NAMESPACE) -R

set-image::
	kustomize cfg set . IMAGE_TAG $(IMAGE_NAME):$(TAG_NAME) -R

set-replicas::
	kustomize cfg set . REPLICAS $(REPLICAS) -R

deploy: output-vars set-project set-dashboard-name set-namespace set-route-namespace set-image set-replicas
	@kubectl apply -k . || exit 0
	@unset OC_PROJECT DASHBOARD_NAME ROUTE_NAMESPACE IMAGE_NAME TAG_NAME REPLICAS && make reset

undeploy:output-vars set-project set-dashboard-name set-namespace set-route-namespace set-image set-replicas
	@kubectl delete -k . -l 'app in ('$(DASHBOARD_NAME)')' || exit 0
	@unset OC_PROJECT DASHBOARD_NAME ROUTE_NAMESPACE IMAGE_NAME TAG_NAME REPLICAS && make reset

reset: output-vars set-project set-dashboard-name set-namespace set-route-namespace set-image set-replicas

.PHONY: defaults
defaults:
	unset DASHBOARD_NAME IMAGE_NAME NAMESPACE_NAME OC_PROJECT ROUTE_NAMESPACE IMAGE_NAME TAG_NAME REPLICAS && make reset
