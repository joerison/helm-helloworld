# Jenkins
## Install with Kubernetes
#### (loses container data when restarting the container.)
    kubectl apply -f jenkins-emptydir.yaml

## Create jenkins admin (with bearer token)
    kubectl apply -f service-account.yaml

## Get bearer token in terminal
    kubectl cluster-info

    # Check all possible clusters, as your .KUBECONFIG may have multiple contexts:
    kubectl config view -o jsonpath='{"Cluster name\tServer\n"}{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'

    # Select name of cluster you want to interact with from above output:
    export CLUSTER_NAME="some_server_name"
    
    # Point to the API server referring the cluster name
    APISERVER=$(kubectl config view -o jsonpath="{.clusters[?(@.name==\"$CLUSTER_NAME\")].cluster.server}")
    
    # Gets the token value
    TOKEN=$(kubectl get secrets -o jsonpath="{.items[?(@.metadata.annotations['kubernetes\.io/service-account\.name']=='default')].data.token}"|base64 --decode)
    
    # Explore the API with TOKEN
    curl -X GET $APISERVER/api --header "Authorization: Bearer $TOKEN" --insecure

References:
- https://kubernetes.io/docs/tasks/administer-cluster/access-cluster-api/
- https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/
