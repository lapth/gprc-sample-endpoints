# Deploy gRPC Server to Kubernetes
Detail: https://cloud.google.com/endpoints/docs/grpc/get-started-kubernetes-engine

# Prepare devlopment environment
1. Checkout
```
git clone https://github.com/lapth/grpc-sample-endpoints.git
```
2. Download protoc (if needed): https://github.com/protocolbuffers/protobuf/releases
3. Generate pb file (if needed): protoc --include_imports --include_source_info greet.proto --descriptor_set_out greet.pb
4. Replace <MY_PROJECT_ID> in api_config.yaml by real one
5. Replace <MY_PROJECT_ID> in deployment.yaml by real ones

# Deploy Endpoints
**Verify project**
```
gcloud config list project
```
**Config project**
```
gcloud config set project <PROJECT_ID>
```
**Deploy**
```
gcloud endpoints services deploy greet.pb api_config.yaml
```
# Required services

1. servicemanagement.googleapis.com => Service Management API
2. servicecontrol.googleapis.com => Service Control API
3. endpoints.googleapis.com => Google Cloud Endpoints

**Verify Service**
```
gcloud services list
```
**Enable Service**
```
gcloud services enable SERVICE_NAME
```
# Deploying the API backend
1. Go to: https://console.cloud.google.com/kubernetes/list?_ga=2.94570965.814323556.1581560215-1866080428.1557550333
2. Create cluster

Name: greet-cluster

Zone: us-central1-a

Nodes: 3

3. Authenticating kubectl
```
gcloud container clusters get-credentials greet-cluster --zone us-central1-a
```
4. Start services:
```
kubectl create -f deployment.yaml
```

# Verify the external IP
**Verify service**
```
kubectl get service
```
You should see your external IP there

**Run greet client to see the result**

# Cleaning up
**Delele Service**
```
gcloud endpoints services delete greet.endpoints.<MY_PROJECT_ID>.cloud.goog
```
**Delete the GKE cluster**
```
gcloud container clusters delete greet-cluster --zone us-central1-a
```