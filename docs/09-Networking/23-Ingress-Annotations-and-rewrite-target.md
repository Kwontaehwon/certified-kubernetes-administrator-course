# Ingress Annotations and rewrite-target
  - Take me to [Lecture](https://kodekloud.com/topic/ingress-annotations-and-rewrite-target/)

> [!Summary]
> 1. URL 경로 변경:
>     - 인그레스로 들어오는 요청의 URL 경로를 백엔드 서비스가 예상하는 경로로 변경할 수 있습니다.
>     - 예: `/something/test` -> `/test`
>     
> 2. 정규 표현식 지원:
>     - 복잡한 URL 패턴을 처리하기 위해 정규 표현식을 사용할 수 있습니다.

In this section, we will take a look at **Ingress annotations and rewrite-target**
- Different Ingress controllers have different options to customize the way it works. Nginx Ingress Controller has many options but we will take a look into the one of the option "Rewrite Target" option.
- Kubernetes Version 1.18
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-ingress
  namespace: critical-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /pay
        backend:
          serviceName: pay-service
          servicePort: 8282

```

#### Reference Docs
- https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/
- https://kubernetes.github.io/ingress-nginx/examples/
- https://kubernetes.github.io/ingress-nginx/examples/rewrite/
- https://github.com/kubernetes/ingress-nginx/blob/master/docs/troubleshooting.md