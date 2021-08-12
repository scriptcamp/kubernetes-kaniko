# kubernetes-kaniko
Builiding Docker images using kaniko on Kubernetes pods

## Github + Dockerhub + Kubernetes

Just a change

### Create Kube Secret
```
kubectl create secret \
    docker-registry regcred \
    --docker-server=https://index.docker.io/v1/ \
    --docker-username=$REGISTRY_USER \
    --docker-password=$REGISTRY_PASS \
    --docker-email=$REGISTRY_EMAIL
```






