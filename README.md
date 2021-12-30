## Prereqs

### Install Helm
```
$ mkdir helm
$ cd helm
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

### Grab the Helm values
```
$ curl -O https://raw.githubusercontent.com/elastic/helm-charts/master/elasticsearch/examples/minikube/values.yaml
$ helm repo add elastic https://helm.elastic.co
```

### Set up kubectl 
```
# Add the following to your ~/.bashrc
alias kubectl="minikube kubectl --"
```

## Running the cluster

### Setting up Minikube
```
$ minikube config set cpus 2
$ minikube config set memory 4096
$ minikube start --kubernetes-version=v1.21.4 --nodes 2
$ kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```

### Run the Elasticsearch components
```
$ helm install elasticsearch-multi-master elastic/elasticsearch -f ./master.yaml
$ helm install elasticsearch-multi-data elastic/elasticsearch -f ./data.yaml
$ helm install elasticsearch-multi-client elastic/elasticsearch -f ./client.yaml
$ helm install kibana elastic/kibana
$ helm install metricbeat elastic/metricbeat
```