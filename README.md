# Argo local on Microk8s

Argo mount volumes to the containers to work with the data. This could be a Minio client who communicate with S3 interface

## Requirements:

- Linux distro(e.g. Ubuntu)
- installation snapd (https://snapcraft.io)
- installation microk8s (https://microk8s.io/)

Bound kubectl to microk8s.kubectl:
```
microk8s.kubectl config view --raw > $HOME/.kube/config
```

enable dashboard:
```
microk8s.enable dns dashboard
```


## Installation Persistence Volume with argo

1. Setup the persistence volume:
```bash
kubectl apply -f storage/minio-persistence-volume.yaml
```

2. Setup the persistence volume claim with Minio:
```bash
kubectl apply -f storage/minio.yaml
```

3. Create a bucket at minio

```bash
kubectl port-forward svc/minio-service 9000:9000
```

Then go to http://localhost:9000 and login with the next credentials:
```
        # Minio access key and secret key
        - name: MINIO_ACCESS_KEY
          value: "mino"
        - name: MINIO_SECRET_KEY
          value: "DONOTUSEINPRODUCTION"
```

**! USE IN PRODUCTION A SECRET !**

_create buckt with name `argo`_


4. Setup Argo
```
kubectl apply -f argo/argo.yaml
```
