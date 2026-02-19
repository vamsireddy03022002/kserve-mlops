# KServe Sklearn Model Deployment

This project shows how I deployed a Scikit-Learn model on Kubernetes using KServe.

## Prerequisites
- Kubernetes cluster
- kubectl
- Helm


**Install Cert Manager**
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.19.0/cert-manager.yaml
kubectl get pods -n cert-manager

**Install KServe**
**Install CRDs:**
helm install kserve-crd oci://ghcr.io/kserve/charts/kserve-crd \
--version v0.16.0 -n kserve --create-namespace
**Install controller:**
helm install kserve oci://ghcr.io/kserve/charts/kserve \
--version v0.16.0 \
--set kserve.controller.deploymentMode=Standard \
-n kserve
**Verify:**kubectl get pods -n kserve

kubectl apply -f job.yaml
**Deploy InferenceService:**
kubectl apply -f inference.yaml
kubectl get inferenceservice

**Test Model**
Port forward:
kubectl port-forward service/model-predictor 8000:80
Send request:
curl -X POST \
-H "Content-Type: application/json" \
-d '{"instances":["sparrow","elephant","sunflower"]}' \
"http://localhost:8000/v1/models/model:predict"
