# Certification Tip
CKA 환경에서는 YAML 파일을 수정하는 것이 힘듬
-> `kubectl run` 명령어를 사용하여 일부 꼼수를 사용 가능.

Dry-run을 실행하면 pod를 생성하지 않음.

Create an NGINX Pod

Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

Generate Deployment YAML file (-o yaml). Don't create it(–dry-run) and save it to a file.

```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

Make necessary changes to the file (for example, adding more replicas) and then create the deployment.

```bash
kubectl create -f nginx-deployment.yaml
```

Pod, ReplicaSet, Deployment 모두 생성 가능.

OR

In k8s version 1.19+, we can specify the --replicas option to create a deployment with 4 replicas.

```bash
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```