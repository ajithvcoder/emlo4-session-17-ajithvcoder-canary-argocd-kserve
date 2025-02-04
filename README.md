# Canary ArgoCD KServe

This example deploys a canary deployment with ArgoCD and KServe.

Create an Argo App

```bash
kubectl apply -f argo-apps
```

This will deploy the example model deployments

NOTE: The Models require an s3-read-only Service Account required to download models from S3

Make sure to create one

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: s3creds
  annotations:
     serving.kserve.io/s3-endpoint: s3.ap-south-1.amazonaws.com # replace with your s3 endpoint e.g minio-service.kubeflow:9000
     serving.kserve.io/s3-usehttps: "1" # by default 1, if testing with minio you can set to 0
     serving.kserve.io/s3-region: "ap-south-1"
     serving.kserve.io/s3-useanoncredential: "false" # omitting this is the same as false, if true will ignore provided credential and use anonymous credentials
type: Opaque
stringData: # use `stringData` for raw credential string or `data` for base64 encoded string
  AWS_ACCESS_KEY_ID: AxxxxQxxxxxxxxY2xxx
  AWS_SECRET_ACCESS_KEY: "C/dGcccuAxxxxxxxx25mxxxxxxx"

---

apiVersion: v1
kind: ServiceAccount
metadata:
  name: s3-read-only
secrets:
- name: s3creds
```
-------------

 kubectl apply -f argo-apps