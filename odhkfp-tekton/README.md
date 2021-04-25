# ODH Kubeflow Pipelines

ODH Kubeflow Pipelines component installs Kubeflow Pipelines that is namespace bound and not cluster wide. There are two pods running after installation

### Install

kustomize build | kubectl apply -f -

### Uninstall

kustomize build | kubectl delete -f -
